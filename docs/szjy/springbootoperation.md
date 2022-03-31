1. 创建Dockerfile文件

   ```shell
   FROM java:8
   VOLUME /tmp
   ADD helloworld.jar hello.jar
   ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/hello.jar"]
   ```

2. 编写启动脚本

   ```shell
   #源jar路径
   SOURCE_PATH=/home/test
   #docker 镜像名字
   IMAGE_NAME=hello
   #docker 容器名字可以和镜像名一样
   SERVER_NAME=hello-test
   TAG=latest
   SERVER_PORT=9898
   #容器id
   CID=$(docker ps | grep "$SERVER_NAME" | awk '{print $1}')
   #镜像id
   IID=$(docker images | grep "$IMAGE_NAME" | awk '{print $3}')
   if [ -n "$CID" ]; then
     echo "存在容器$SERVER_NAME, CID-$CID"
     docker stop $SERVER_NAME
     docker rm $SERVER_NAME
   fi
   # 构建docker镜像
   if [ -n "$IID" ]; then
     echo "存在$IMAGE_NAME:$TAG镜像，IID=$IID"
     docker rmi $IMAGE_NAME:$TAG
   else
     echo "不存在$IMAGE_NAME:$TAG镜像，开始构建镜像"
   fi
   cd $SOURCE_PATH
   docker build -t "$IMAGE_NAME:$TAG" .
   # 运行docker容器并把存放图片的目录挂载出来
   docker run --name $SERVER_NAME -v /home/test/images:/home/test/images -d -p $SERVER_PORT:$SERVER_PORT $IMAGE_NAME:$TAG
   echo "$SERVER_NAME容器创建完成"
   
   ```

   

