# day3 build image

* Docker Build
`docker build .`
* check new image
`docker image ls`
* 在本機上運行containerized app
docker run -p 3000:3000 -it <imageID>

# [Day 4] 上傳 Docker Image 到 Docker Hub
* create a new 資料夾 on docker Hub
* tag the docker image
`docker tag <imageID> <tag name>`
`docker tag 9a26b8765953 kimlin20011/docker_node_test_v2`
`docker tag bc884c7f3fb9 kimlin20011/docker_node_test:v2.0.0`
`docker tag 3b505be17388 kimlin20011/docker_node_test:v3.0.0`
* push image to docker hub
`docker push kimlin20011/docker_node_test:v2.0.0`
`docker push kimlin20011/docker_node_test:v3.0.0`