# Troubleshooting

<!-- vim-markdown-toc GFM -->

* [Linux](#linux)
    * [ping: icmp open socket: Operation not permitted](#ping-icmp-open-socket-operation-not-permitted)
* [Consul](#consul)
    * [[ERR] memberlist: Failed to send gossip to x.x.x.x:8301: write udp [::]:8301->x.x.x.x:8301: sendto: invalid argument](#err-memberlist-failed-to-send-gossip-to-xxxx8301-write-udp-8301-xxxx8301-sendto-invalid-argument)
* [K8S](#k8s)
    * [Error: failed to start container "delivery-center-t2crl1": Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:319: getting the final child's pid from pipe caused \"EOF\"": unknown](#error-failed-to-start-container-delivery-center-t2crl1-error-response-from-daemon-oci-runtime-create-failed-container_linuxgo349-starting-container-process-caused-process_linuxgo319-getting-the-final-childs-pid-from-pipe-caused-eof-unknown)

<!-- vim-markdown-toc -->



### Linux

#### ping: icmp open socket: Operation not permitted

- symptom

    普通用户执行 ping 命令报错

- cause

    ping 缺少 `suid` 属性

- solution

    ```
    # sudo chmod u+s /bin/ping
    ```


- reference
    - [PING icmp open socket: Operation not permitted in vserver](https://serverfault.com/questions/696281/ping-icmp-open-socket-operation-not-permitted-in-vserver)


### Consul

####  [ERR] memberlist: Failed to send gossip to x.x.x.x:8301: write udp [::]:8301->x.x.x.x:8301: sendto: invalid argument

- symptom

    QA 环境迁移到 K8S 后，压测发现有许多服务节点状态变得非常不稳定，时而健康时而不健康，影响测试流程。

    通过查看 consul agent 日志发现有非常多的 `Failedto send gossip to ... ` 报错，猜测被其他节点"投死" ([Gossip 协议](https://en.wikipedia.org/wiki/Gossip)) 导致不能正常提供服务

    consul agent 日志：

    ```
    [WARN] agent: Check "service:5dcf789c-w5pnq:http" HTTP request failed: Get http://127.0.0.1:8080/health: net/http: request canceled while waiting for connection (Client.Timeout exceeded while awaiting headers)
    [INFO] agent: Synced check "service:5dcf789c-w5pnq:http"
    [INFO] agent: Synced check "service:5dcf789c-w5pnq:http"
    [ERR] memberlist: Failed to send gossip to x.x.x.x:8301: write udp [::]:8301->x.x.x.x:8301: sendto: invalid argument
    [ERR] memberlist: Failed to send ack: write udp [::]:8301->x.x.x.x:8301: sendto: invalid argument from=x.x.x.x:8301
    [ERR] memberlist: Failed to send ping: write udp [::]:8301->x.x.x.x:8301: sendto: invalid argument
    ```


- cause

    通过官方 issue [263](https://github.com/hashicorp/serf/issues/263) 发现是 ARP/neighbor table overflow 的问题，同时在 `/var/log/message` 日志中也发现了大量的 neighbor table overflow 的报错

    /var/log/message:

    ```
    kernel: neighbour: arp_cache: neighbor table overflow!
    kernel: neighbour: arp_cache: neighbor table overflow!
    kernel: neighbour: arp_cache: neighbor table overflow!
    ```


- solution

    设置以下的配置解决（当前 running pod 数量为200）

    ```
    vi /etc/sysctl.conf
    net.ipv4.neigh.default.gc_thresh1 = 2048
    net.ipv4.neigh.default.gc_thresh2 = 4096
    net.ipv4.neigh.default.gc_thresh3 = 8192
    sysctl -p
    ```

    推荐内核参数

    ```
    cat /etc/sysctl.d/k8s.conf 
    net.ipv4.ip_forward=1
    net.ipv4.conf.eth0.rp_filter=2
    net.bridge.bridge-nf-call-ip6tables = 1
    net.bridge.bridge-nf-call-iptables = 1
    net.ipv4.neigh.default.gc_thresh1 = 2048
    net.ipv4.neigh.default.gc_thresh2 = 4096
    net.ipv4.neigh.default.gc_thresh3 = 8192
    net.ipv4.neigh.default.gc_interval = 120
    net.ipv4.neigh.default.gc_stale_time = 240
    ```

- reference
    - [How to increase the ARP cache garbage collection threshold](https://success.docker.com/article/how-to-increase-the-arp-cache-collection-threshold)
    - [ip-sysctl](https://www.kernel.org/doc/Documentation/networking/ip-sysctl.txt)


### K8S

#### Error: failed to start container "delivery-center-t2crl1": Error response from daemon: OCI runtime create failed: container_linux.go:349: starting container process caused "process_linux.go:319: getting the final child's pid from pipe caused \"EOF\"": unknown

- symptom

    无法启动容器

- cause

    查看相关内核参数
    ```
    user.max_user_namespaces = 0
    kernel.pid_max = 196608
    ```

- solution

    ```
    # sudo sysctl -w kernel.pid_max=100000
    ```

- reference
    - [Cannot start container: Getting the final child's pid from pipe caused "EOF"](https://github.com/moby/moby/issues/40835)
