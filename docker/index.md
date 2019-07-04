
docker run -d \
    --hostname gitlab \
    --publish 443:443 --publish 8089:80 --publish 2222:22 \
    --name gitlab \
    --restart always \
    --volume $GITLAB_HOME/config:/etc/gitlab \
    --volume $GITLAB_HOME/logs:/var/log/gitlab \
    --volume $GITLAB_HOME/data:/var/opt/gitlab \
    gitlab/gitlab-ce

sudo docker exec -it gitlab /bin/bash