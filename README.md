# tiny-web-server

此文代码来源于《深入理解计算机系统·第二版》11.6节的综合：TINY Web服务器。
书上只给了源代码但是没给搭建的步骤，想要理解道理需要先有感性的认识，所以先把服务器搭起来再说。

网络这块学完，Linux下基本的操作就完了，I/O、并发、网络。

https://zhuanlan.zhihu.com/p/353692786   --- 网络 IO 演变过程

写完Tiny Web，应该达到的状态是，对网络底层已经有了基本的了解，掌握了数据传输的方式的接口函数使用，
明白了FTP、HTTP等所谓的协议都是什么样的概念。

网络的目的是数据传输。要重点理解，数据传输接口的使用。

 11 /************************************************************************************************************************
 12 1、int socket(int family,int type,int protocol)
 13 family:
 14     指定使用的协议簇:AF_INET（IPv4） AF_INET6（IPv6） AF_LOCAL（UNIX协议） AF_ROUTE（路由套接字） AF_KEY（秘钥套接字）
 15 type:
 16     指定使用的套接字的类型:SOCK_STREAM（字节流套接字） SOCK_DGRAM
 17 protocol:
 18     如果套接字类型不是原始套接字，那么这个参数就为0
 19 2、int bind(int sockfd, struct sockaddr *myaddr, int addrlen)
 20 sockfd:
 21     socket函数返回的套接字描述符
 22 myaddr:
 23     是指向本地IP地址的结构体指针
 24 myaddrlen:
 25     结构长度
 26 struct sockaddr{
 27     unsigned short sa_family; //通信协议类型族AF_xx
 28     char sa_data[14];  //14字节协议地址，包含该socket的IP地址和端口号
 29 };
 30 struct sockaddr_in{
 31     short int sin_family; //通信协议类型族
 32     unsigned short int sin_port; //端口号
 33     struct in_addr sin_addr; //IP地址
 34     unsigned char si_zero[8];  //填充0以保持与sockaddr结构的长度相同
 35 };
 36 3、int connect(int sockfd,const struct sockaddr *serv_addr,socklen_t addrlen)
 37 sockfd:
 38     socket函数返回套接字描述符
 39 serv_addr:
 40     服务器IP地址结构指针
 41 addrlen:
 42     结构体指针的长度
 43 4、int listen(int sockfd, int backlog)
 44 sockfd:
 45     socket函数绑定bind后套接字描述符
 46 backlog:
 47     设置可连接客户端的最大连接个数，当有多个客户端向服务器请求时，收到此值的影响。默认值20
 48 5、int accept(int sockfd,struct sockaddr *cliaddr,socklen_t *addrlen)
 49 sockfd:
 50     socket函数经过listen后套接字描述符
 51 cliaddr:
 52     客户端套接字接口地址结构
 53 addrlen:
 54     客户端地址结构长度
 55 6、int send(int sockfd, const void *msg,int len,int flags)
 56 7、int recv(int sockfd, void *buf,int len,unsigned int flags)
 57 sockfd:
 58     socket函数的套接字描述符
 59 msg:
 60     发送数据的指针
 61 buf:
 62     存放接收数据的缓冲区
 63 len:
 64     数据的长度，把flags设置为0
 65 *************************************************************************************************************************/
