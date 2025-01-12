# Commands  run local Gitlab instance
1.Create local network
```shell
docker network create gitlab-network
```
2.Run Local Gitlab
```shell
docker run -d \
  --name gitlab \
  --network gitlab-network \
  -p 8080:80 -p 443:443 -p 22:22 \
  -v gitlab-config:/etc/gitlab \
  -v gitlab-logs:/var/log/gitlab \
  -v gitlab-data:/var/opt/gitlab \
  gitlab/gitlab-ce:latest
```
3.Get Gitlab root password
```shell
docker exec -it gitlab cat /etc/gitlab/initial_root_password
```
4.Run Local Gitlab Runner in same network
```shell
docker run -d --name gitlab-runner --network gitlab-network \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v gitlab-runner-config:/etc/gitlab-runner \
  gitlab/gitlab-runner:latest
```
5.Perform Runner Registration
```shell
docker exec -it gitlab-runner gitlab-runner register \
  --url http://gitlab/ \
  --registration-token YOUR_REGISTRATION_TOKEN
```
## Reference
- https://www.youtube.com/watch?v=Rvh7OZbDJ_o (from Build with LaL)
