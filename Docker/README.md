# Docker

## 容器化技术之隔离

Docker 容器其实就是一个特殊的进程而已，通过linux的Namespace进行进程隔离，并且利用Cgroups进行资源限制

### Namespace 隔离

```c
int pid = clone(main_function, stack_size, ClONE_NEWPID | SIGCHLD, NULL)
```

ClONE_NEWPID 就是用来开辟新的pid障眼法，是的容器认为自己就是pid=1

### 使用 Cgroups

```bash
mount -t cgroup
ls /sys/fs/cgroup/cpu
```

创建自己的Cgroups

```bash
cd /sys/fs/cgroup/cpu
mkdir container
ls
```

运行进程, 并且吃光所有CPU

```bash
while : ; do : ; done &
```

查看top

```bash
top
```

添加限制

```bash
echo 20000 > /sys/fs/cgroup/cpu/container/cpu.cfs_quota_us
echo {PID} > /sys/fs/cgroup/cpu/container/tasks
```

#### 验证

启动docker

```bash
docker run -it --cpu-period=100000 --cpu-quota=20000 ubuntu /bin/bash
```

检查cgroup配置
```bash
$ cat cpu.cfs_period_us
100000
```

### 容器技术之镜像

[https://zhuanlan.zhihu.com/p/41958018](https://zhuanlan.zhihu.com/p/41958018)

Docker镜像其实就是文件的镜像 (rootfs) 在`/var/lib/docker/overlay2`

rootfs 是分层的： 

![容器分层](../assets/docker_img_layer.png)

每层具体的文件存放在层标识符下的diff目录下

link文件描述了该层标识符的精简版，而lower文件描述了层序的组织关系

+ 只读层为操作系统读部分

+ Init层用来存放/etc/hosts, /etc/resolv.conf 等信息

+ 可读写层用来存放修改的信息

最后docker会使用联合挂载（union mount）来挂载（上层覆盖下层），并且使用chroot操作来修改容器的更目录为宿主机的一个目录

查看联合挂载情况
```bash
mount | grep overlay
```

总结一下，镜像大体上，可以认为是多个只读层通过某些特定的方式组织起来，而容器则是在其之上的一个可读写层，我们可以保存一个可读写层的更改，将它转化为一个只读层。