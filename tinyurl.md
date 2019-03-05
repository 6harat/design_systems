# design a tinyurl service:
* requirements:
    + functional:
        - shorten the original url
        - redirect to the original url when accessed
        - allow user to set custom url for a link
        - expiry options of a link
    + non-functional:
        - service highly scalable and available
        - url redirection in real-time
        - shorten urls should not be guessable
    + extended:
        - support for analytics
        - exposure via ReST APIs
* capacity estimation:
    + assumptions:
        - new urls/month = 500M
        - read/write ratio = 100
        - record size = 500 bytes
        - default retention period = 5 years
    + derivations:
        - url redirection/second = 500M * 100 = 50B
        - new urls/second = 500M / (30 * 24 * 3600) ≈ 200
        - url redirection/second ≈ 200 * 100 = 20K
        - storage requirement = (urls for 5 years) * (record size) 
                              = (500M urls/month * 12 months * 5 years) * (500 bytes)
                              = (30B) * (500) = 15 TB
        - bandwidth for write requests = (url/second) * (record size) = 200 * 500 = 100KB/s
        - bandwidth for read requests = (url/second) * (record size) = 20K * 500 = 10MB/s
        - cache memory size = 20% of (daily traffic) * (record size)
                            = 0.2 * (20K * 24 * 3600) * (500)
                            = 0.2 * (1.7B) * (500)
                            = 170GB
* api design:
    + read  : GET /url/:id
    + create: POST /url
    + update: PUT /url/:id
    + delete: DELETE /url/:id
* schema design:
    + URL
        id          (PK)
        original_url
        create_date
        expiry_date
        user_id     (FK:user.id)
    + USER
        id          (PK)
        name
        email
        create_date
        last_login
* architecture and algorithm:
    + encoding:
        - num keys available = (num of chars in base 64 encoding) * (len of hash)
            for key length 6 = 64^6 = 68719476736 ≈ 68B
            for key length 8 = 64^8 = 281474976710656 ≈ 281T
            5yrs storage req = 30B
            => key-length = 6 should suffice
    + key generation:
        - MD5/SHA256: they will generate long keys which will have to truncated.
            also there is a chance of duplicates which will impact performance.
        - pre-generated keys: separate service which keeps pre-generated keys into the system.
            allocates the key as per demand by the server.
            storage requirement for key-db = (key length) * (size of 1 char) * (total keys)
                                           = (6) * (1 byte) * (68B) = 408 GB ≈ 400 GB
        - custom-keys: custom key size to be limited to a certain length for ease of storage
    + storage operations:
        - partitioning: use consistent hashing to gracefully handle the event of 
            adding and removing nodes
        - caching: LRU cache using a single node/cluster of nodes for the top URLs.
            can be tweaked later based on analytics of cache-hit rates.
        - cleanup: delete record when accessed post expiry / batch operation
    + load balancing:
        - client          <-> application server
        - database server <-> application server
        - cache server    <-> application server