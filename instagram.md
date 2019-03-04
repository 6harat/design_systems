# design an instagram service:
* requirements:
    + functional:
        - user should be able to view/upload/download photos/videos
        - user can perform searches based on photo/video titles
        - user can follow other users
        - user should shown a news feed on the landing page
    + non-functional:
        - service should be highly available
        - acceptable latency for newsfeed: 200ms
        - highly reliably (no loss of content/photos/videos)
    + deferred:
        - comments
        - likes
        - suggestions for who to follow
        - tags on photos/videos
* capacity estimation:
    + assumptions:
        - total users = 500M
        - daily active users = 1M
        - photos uploaded per active user per day = 2
        - avg photo size = 200 KB
    + derivations:
        - photos uploaded per day = (photos uploaded per active user per day) * (active users)
                                  = (2) * (1M) = 2M
        - storage requirement for photos/day = (photos uploaded per day) * (avg photo size)
                                             = (2M) * (200 KB) = 400 GB
        - storage requirement for 10 years = (storage requirement for photos/day) * (num days)
                                           = (400 GB) * (10 * 365) = 1.46 PB
* api endpoints:
    + 
* schema design:
    + PHOTO:
        id              (PK) (IDX_1)
        create_date          (IDX_1)
        user_id         (FK:user.id)
    + USER:
        id              (PK)
        name
        email
        date_of_birth
        create_date
        last_login
    + USER_FOLLOW:
        follower        (C_PK)
        followee        (C_PK)