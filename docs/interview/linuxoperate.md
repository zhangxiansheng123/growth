## Docker常用命令

1. 查看镜像

   ```
   docker images  # 查看镜像摘要
   docker images --no-trunc #显示镜像完整信息
   docker rmi 镜像名 # 删除指定镜像
   ```

2. 查看容器

   ```
   docker ps -a # 查看容器摘要
   docker ps --no-trunc  # 查看容器完整信息
   docker start 容器名
   docker stop 容器名
   docker rm 容器名
   ```

3. 查看日志

   ```
   docker logs -f -n 100 容器名  # 动态输出容器日志
   docker logs -f --tail 100 容器名 # 同上
   ```

4. 容器的交互

   ```
   docker exec -it 容器名 /bin/bash
   ```

5. 容器与主机间的数据拷贝

   ```
   docker cp nginx:/etc/nginx/conf.d/hello.conf /home/test  #从容器拷贝文件
   docker cp /home/test/hello.conf nginx:/etc/nginx/conf.d
   ```

6. 查看容器详细信息

   ```
   docker inspect 容器名
   ```

## Linux常用命令

1. 排查内存使用情况

   ```shell
   ps axo %mem,comm,pid --sort=-%mem | head -n 15  # 查出内存使用前十的进程号然后看整体内存使用多少
   ps -ef | grep pid  # 配合上个命令查看具体哪个应用
   ```

2. 查看文件所占磁盘大小、内存大小、磁盘大小

   ```
   du -sh *      # 查看该目录下文件大小各占多少
   du -sh 文件名  # 查看指定文件大小
   free -h       # 查看内存大小及使用情况
   df -h         # 查看磁盘大小及使用情况
   ```

   

3. 文件在主机间的拷贝

   ```shell
   scp -r 文件或文件夹  root@hello:/home/test
   ```

4. 目录切换cd

   ```shell
   cd /   # 切换到根目录
   cd ..  # 切换到上一级目录 或者 cd ..
   cd ~   # 切换到home目录
   cd -   # 切换到上次访问的目录
   ```

5. 目录查看

   ```shell
   ls -a         # 查看当前目录下的所有目录和文件（包括隐藏的文件）
   ls -l 或者 ll  # 列表查看当前目录下所有的目录和文件 
   ls -l | grep 'hello'
   ```

6. 目录创建mkdir

   ```shell
   mkdir quan       # 创建一个名字quan的目录
   mkdir /usr/quan  # 在指定目录下创建一个名为quan的目录
   ```

7. 删除目录与文件

   ```shell
   rm -rf  # 最喜欢用的删除命令
   ```

8. 目录修改与拷贝mv和cp

   ```shell
   mv 当前目录 新目录  #
   mv 目录绝对路径 到新目录位置
   cp -r 目录名称  目标拷贝目录   # -r递归，可以拷贝文件夹
   cp 文件名 目标指定目录 
   ```

9. 搜索目录和文件

   ```shell
   find 目录  参数  文件名称
   find  /    -name  'quan'
   ```

10. 文件创建

    ```shell
    touch   # 新建文件
    vim 文件名  #或者vi文件名创建
    ```

11. cat/more/less/tail 查看文件 偏爱cat与tail

    ```powershell
    cat nginx.conf  # 只能显示最后一屏内容
    more nginx.conf  # 可以显示百分比，回车可以向下一行，空格可以向下一页，q退出查看
    less nginx.conf  # 翻页查看可以使用PgUp和PgDn向上和向下翻页，q结束查看
    tail -f zoo.cfg  # 实时查看
    tail -f -n 100 zoo.cfg   # 实时查看文件后100
    ```

12. 权限修改chmod

13. 解压与压缩

    ```shell
    tar -zcvf 打包压缩后的文件名  要打包的文件  # z调用gzip压缩命令进行压缩  c:打包文件 v:显示运行过程 f:指定文件名
    
    tar -zxvf 压缩包  # 解压文件
    tar -zxvf 压缩包 -C /home/test  # 解压文件到指定位置
    ```

14. grep文本搜索

    ```shell
    ps -ef | grep 服务名或者进程号     # 查找指定服务进程
    ps -ef | grep 服务名 -c  # 查找指定进程个数
    ```

15. whereis

    ```powershell
    whereis nginx  # whereis命令是定位可执行文件、源代码文件、帮助文件在文件系统中的位置。
    ```

16. which which命令的作用是在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。

    ```
    which pwd  查找pwd命令所在路径 
    which java  查找path中java的路径 
    ```

17. su、sudo

    ```
    su # su用于用户之间的切换。但是切换前的用户依然保持登录状态。如果是root 向普通或虚拟用户切换不需要密码，反之普通用户切换到其它任何用户都需要密码验证。
    sudo # 需要配置 sudo是为所有想使用root权限的普通用户设计的。可以让普通用户具有临时使用root权限的权利。只需输入自己账户的密码即可。
    ```

18. 网络通信

    ```
    netstat -ntlp  # 查看启动端口
    ping ip # 查看某台机器的连接情况
    telnet ip port  # 查看端口是否能访问
    ```

    

