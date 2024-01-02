## Docker

### 基本概念

- **镜像**
- **容器**
- **仓库**

### 使用镜像

#### 获取镜像

```shell
$docker pull 仓库地址/用户名/镜像名
```

#### 运行

获取镜像以后，就能以镜像为基础启动并运行一个容器，随即在容器中进行操作。

```shell
$docker run -it --rm 镜像名
#镜像名如ubuntu:18.04，18.04为标签
```

`-it`为交互式操作，`--rm`为容器推出后，将其删除。

#### 列出镜像

列出已经下载的镜像：

```shell
$docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    9c7a54a9a43c   7 months ago   13.3kB
```

便捷查看镜像、容器、数据卷所占用空间：

```shell
$docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          1         0         13.26kB   13.26kB (100%)
Containers      0         0         0B        0B
Local Volumes   1         0         0B        0B
Build Cache     0         0         0B        0B
```

#### 虚悬镜像

由于新旧镜像同名，旧镜像名称被取消，因而出现相关性均为`<none>`的镜像，即虚悬镜像。

使用如下命令显示这类镜像：

```shell
$docker image ls -f dangling=true
```

删除虚悬镜像使用：

```shell
$docker image prune
```

#### 删除本地镜像

如果要删除本地镜像使用：

```shell
$docker image rm 镜像名
Untagged:
Deleted:
```

删除行为分为两类，一类是`Untagged`与`Deleted`。当使用`rm`命令时，实际是在删除某个标签的镜像，，当该镜像的所有标签都被取消了，就会触发删除行为。而当有其他镜像或容器正依赖与当前镜像时，依旧不会触发删除行为，直到没有任何层依赖当前层时，才会真实的删除当前层。

### 容器

#### 启动

启动容器分为两种方式，一种是基于镜像新建一个容器并启动，另外一个是将在终止状态的容器重新启动。

**新建并启动使用的是`docker run`**

当利用 `docker run` 来创建容器时，Docker 在后台运行的标准操作包括：

- 检查本地是否存在指定的镜像，不存在就从 [registry]() 下载
- 利用镜像创建并启动一个容器
- 分配一个文件系统，并在只读的镜像层外面挂载一层可读写层
- 从宿主主机配置的网桥接口中桥接一个虚拟接口到容器中去
- 从地址池配置一个 ip 地址给容器
- 执行用户指定的应用程序
- 执行完毕后容器被终止

**启动已终止的容器**

使用`docker container start + 容器ID/容器名`直接将一个已经终止的容器启动运行。

对`docker run`使用`-d`参数将容器置于后台运行，输出结果可以使用`docker logs`和`docker container logs`查看。

#### 终止

使用`docker container stop + 容器ID/容器名`来终止一个运行中的容器；

使用`docker container restart + 容器ID/容器名`来重新启动一个运行态容器；

 #### 进入容器

当使用`-d`参数进入后台后，可以使用`docker attach`和`docker exac -it`重新连接容器，将输入输出重定向到当前终端。

#### 导入与导出

使用`docker export`导出容器快照到本地文件：

```shell
$docker export ID号 > 文件名
```

使用`docker import`从容器快照文件再导入为镜像：

```shell
$cat ubuntu.tar | docker import - test/ubuntu:v1.0
```

#### 删除容器

删除一个处于终止状态的容器：

```shell
$docker container rm 容器名/ID
```

还可以添加`-f`参数。

清除所有处于终止状态的容器：

```shell
$docker container prune
```

### 仓库

仓库是集中存放镜像的地方

### 数据管理

Docker内部及容器之间管理数据的方式分为两种：

- 数据卷
- 挂载主机目录

#### 数据卷

数据卷是一个可供多个容器使用的特殊目录，具有以下特性：

- 数据卷可以在容器之间共享和重用
- 对数据卷的修改会立刻生效
- 对数据卷的更新不会影响镜像
- 数据卷默认会一直存在，即使容器被删除

创建一个数据卷

```shell
$docker volume create vol-name
```

查看所有的数据卷

```shell
$docker volume ls
```

查看指定数据卷的信息：

```shell
$docker volume inspect vol-name
```

使用`--mount`来启动一个挂载数据卷的容器：

```shell
$ docker run -d -P \
    --name web \
    # -v my-vol:/usr/share/nginx/html \
    --mount source=my-vol,target=/usr/share/nginx/html \
    nginx:alpine
```

删除数据卷：

```shell
$docker volume rm vol-name
```

数据卷是被设计用来持久化数据的，它的生命周期独立于容器，Docker 不会在容器被删除后自动删除数据卷，并且也不存在垃圾回收这样的机制来处理没有任何容器引用的数据卷。

无主的数据卷可能会占据很多空间，可以用以下命令清理：

```shell
$ docker volume prune
```

#### 挂载主机目录

使用`--mount`可以指定挂载本地主机的目录到容器中去：

```shell
$ docker run -d -P \
    --name web \
    # -v /src/webapp:/usr/share/nginx/html \
    --mount type=bind,source=/src/webapp,target=/usr/share/nginx/html \
    nginx:alpine
```

上面的命令加载主机的`/src/webapp`目录到容器的`/usr/share/nginx/html`目录。

在主机里使用以下命令查看`web`容器的信息

```shell
$ docker inspect web
```

**`--mount` 标记也可以从主机挂载单个文件到容器中**



