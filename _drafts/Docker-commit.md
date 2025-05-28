```
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

OPTIONS说明：
 -a :提交的镜像作者。

 -c :使用 Dockerfile 指令来创建镜像。
    
 -m :提交时的说明文字。
    
 -p :提交镜像前暂停容器（默认为 true）。 
```
### 将容器保存为新镜像:

`docker commit [containerID] [my_new_image]`

### 将名为 my_container 的容器保存为一个名为 my_new_image 的新镜像。

**指定标签**:

`docker commit my_container my_new_image:latest`

将容器保存为带有 `latest` 标签的镜像。

**添加作者信息和提交信息:**

`docker commit -a "John Doe" -m "Added new features" my_container my_new_image`

将容器保存为新镜像，并添加作者信息和提交信息。

**在不暂停容器的情况下提交镜像:**

`docker commit --pause=false my_container my_new_image`

**在不暂停容器的情况下，将其保存为新镜像。**

### 实例

**启动一个容器:**

`docker run -d -it --name [container_name] ubuntu bash`

**进行一些更改:**

`docker exec [container] apt-get update`
`docker exec [container] apt-get install -y nginx`

**提交容器为新镜像:**

`docker commit -a "Your Name" -m "Installed nginx" my_container my_new_image`

**查看新镜像:**

`docker images`