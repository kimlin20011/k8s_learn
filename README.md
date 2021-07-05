# [K8s學習](https://ithelp.ithome.com.tw/articles/10192519)

# Day3 docker
* 安裝 Docker Engine 
* 製作檔案資料夾，並新增dockerfile

```dockerfile=
FROM node:6.2.2  #node v6.2.2
WORKDIR /app  #在這個 Docker 中的 Linux 即將會建立一個目錄 /app
ADD . /app #代表會將本機端與 Dockerfile 同一層的所有檔案加到 Linux 的 /app 目錄底下
RUN npm install #npm install 會下載 nodejs 相依的 libraries
EXPOSE 300 #是指 container 對外的埠號，再與外界溝通時使用
CMD npm start #最後透過 npm start 會運行 Nodejs App
```
* 進資料夾，打包資料夾成docker image
```
docker build .
```
* 查看image是否已經build
```
docker image ls
```

* 本機運行 docker app
```
docker run -p 3000:3000 -it <image ID>
```

# Day4
* Docker Registry
    * 存放docker image的地方
* Docker hub
    * 官方提供的docker registry
    * 申請帳號
* 連結docker hub
    *  到terminal輸入，`docker login`
* docker image + tag
```
docker tag <Image ID> <account>/<tag name>:<version>

# example
docker tag 5ba49c38703a kimlin20011/docker_node_test:v1.0.0
docker image ls
```

* docker push
```
docker push <account>/<tag name>
```
*  Docker Remarks
    *  一個docker image只運行一個process
    *  Container 中的 data 不會保存下來 All data in the container is not preserved。當一個 container 停止運作時，存在該 container 的資料也會隨之消失。必須藉由第三方儲存服務，將內部的data保存下來，在日後學習筆記也會介紹到 Volumes，Kubernetes 會藉由 Volumes Component 將 data 保存下來。
*  下載某個image
```
docker pull <image name>
```

###### tags: `paper`
