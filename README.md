# docker-cleanup
Cleanup docker host from dangling and cache data that are consuming storage

1. Remove dangling images.
    Docker images consist of multiple layers. Dangling images are layers that have no relationship to any tagged images. They are no longer used and just consume disk space.
    
    ```shell
    docker images -f dangling=true -q | xargs -r docker rmi -f
    ```

2. Remove dangling volumes.
    The "dangling volume" semantic is a bit different from what the "dangling image" means. A dangling volume is one that exists and is no longer connected to any containers — just an unused volume.
    
    ```shell
    docker volume ls -qf dangling=true | xargs -r docker volume rm
    ```
    
4. Clean the Docker builder cache.
    Normally, Docker caches the results of the first build of a Dockerfile, allowing subsequent builds to be fast.
    
    ```shell
    DOCKER_BUILDKIT=1 docker builder prune --all --force
    ```
