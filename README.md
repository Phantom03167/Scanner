# Scanner使用说明

本程序利用`scapy`包编写了一个简易扫描工具，支持ARP、ICMP、TCP、UDP发现扫描，支持TCP SYN、UDP端口扫描，程序说明如下：

```bash
usage: scanner <-p ping扫描类型> <-s 端口发现类型> <-t TARGET> [--port <PORT>]

简单扫描工具，可以进行存活扫描及端口扫描. 存活扫描包括：ARP扫描、ICMP扫描、TCP扫描、UDP扫描. 端口扫描包括：TCP SYN扫描、TCP ACK扫描、TCP FIN扫描.

optional arguments:
  -h, --help            show this help message and exit
  -v, --version         show program's version number and exit

target group:
  用于设置IP、PORT参数

  -t TARGET, --target TARGET
                        target为IP或IP段，如192.168.1.1，192.168.1.x，或192.168.1.1-254
  --port PORT           port为待扫描的端口，如21,80,...或21-80
  -p-                   扫描1-65535的所有端口

ping group:
  用于开启存活扫描相关选项

  -s                    开启存活扫描
  --ARP                 启动ARP扫描
  --ICMP                启动ICMP扫描
  --TCP                 启动TCP扫描
  --UDP                 启动UDP扫描

port scan group:
  用于开启端口扫描相关选项

  -p                    开启端口扫描
  --SYN                 开启SYN扫描
  --ACK                 开启ACK扫描
  --FIN                 开启FIN扫描
  --UPORT               开启UDP端口扫描

utils group:
  用于开启扫描过程中的一些实用选项

  --timeout TIMEOUT     设置发包超时时间,默认1.5秒
  --retry RETRY         设置发包重试次数，默认不重试

以上做为说明，祝好运！
```

---

# 主机扫描

使用`-s`参数进行主机扫描。

## § ARP扫描

使用`--ARP`参数进行ARP扫描，如下：

```bash
scanner -s -t 192.168.1.1-254 --ARP
```

使用`--retry`参数增加发包尝试次数，如下：

```bash
scanner -s -t 192.168.1.1-254 --ARP --retry 3
```



## § ICMP扫描

若没有指定任何主机扫描方式参数，默认会启用ICMP扫描，或使用`--ICMP`参数进行ICMP扫描，如下：

```bash
scanner -s -t 192.168.1.1-254
或
scanner -s -t 192.168.1.1-254 --ICMP
```

使用`--timeout`参数，设置较长的超时，可以防止因网络状况不好造成丢包而遗失主机，如下：

```bash
scanner -s -t 192.168.1.1-254 --ICMP --timeout 2
```



## § TCP扫描

使用参数`--TCP`进行TCP扫描，如下：

```bash
scanner -s -t 192.168.1.100-115 --TCP
```



## § UDP扫描

使用参数`--UDP`进行UDP扫描，如下：

```bash
scanner -s -t 192.168.1.100-115 --UDP
```

---

# 端口扫描

使用`-p`参数进行端口扫描。

## § SYN扫描

若没有指定任何端口扫描方式参数，默认会启用SYN扫描，或使用`--SYN`参数进行SYN扫描；若不设置端口参数，则默认扫描1-1024端口，如下：

```bash
scanner -p -t 192.168.1.110
或
scanner -p -t 192.168.1.110 --SYN
```

使用`--port`扫描指定端口，如下：

```bash
scanner -p -t 192.168.1.110 --SYN --port 10,22-30,80,443,500-1000
```



## § ACK扫描

使用`--ACK`参数进行ACK扫描，如下：

```bash
scanner -p -t 192.168.1.110 --ACK
```

使用`-p-`参数扫描1-65535的所有端口（`-p-`和`--port <PORT>`参数只能二选一使用），如下：

```bash
scanner -p -t 192.168.1.110 --ACK -p-
```



## § FIN扫描

使用`--FIN`参数进行FIN扫描，如下：

```bash
scanner -p -t 192.168.1.110 --FIN --port 1-100
```



## § UDP 扫描

使用`--UPORT`参数进行UDP扫描，如下：

```bash
scanner -p -t 192.168.1.110 --UPORT
```

---

# 综合扫描

使用`-s`和`-p`参数同时进行主机和端口扫描，如下：

```bash
scanner -t 192.168.1.100-110 -s --ARP -p --SYN --port 1-10000,31545 --timeout 2
```

