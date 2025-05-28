
1. **`docker network ls`**: 列出所有网络
2. **`docker network inspect`**: 查看网络详细信息
3. **`docker network create`**: 创建一个新网络
4. **`docker network rm`**: 删除一个或多个网络
5. **`docker network connect`**: 将一个容器连接到一个网络
6. **`docker network disconnect`**: 将一个容器从一个网络断开

### `docker network ls` 命令

`docker network inspect [name]`


### `docker network create`

```

[root@host01 ~]# docker network create --help

Usage:  docker network create [OPTIONS] NETWORK

Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by Network driver (default map[])
      --config-from string   The network from which to copy the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a network segment
 ```

**常用参数**

- **`--driver`**: 指定网络驱动程序（如 `bridge`、`host`、`overlay`）。
- **`--subnet`**: 指定子网。
- **`--gateway`**: 指定网关。
- **`--ip-range`**: 指定可用 IP 地址范围。
- **`--ipv6`**: 启用 IPv6。
- **`--label`**: 为网络添加标签。

`docker network create --driver bridge --subnet 192.168.1.0/24 my_network`

### `docker network connect` 命令

将一个容器连接到一个网络。

`docker network connect my_network my_container`

### `docker network disconnect` 命令

将一个容器从一个网络断开。

`docker network disconnect [my_network] [my_container]`

