#指定基本镜像(根镜像)
FROM centos
#提供作者的信息
MAINTAINER from jcloud.com by fanlu
#安装sshd服务以及对应的工具
RUN    yum -y install openssh openssh-server openssh-clients net-tools wget vim
RUN    mkdir -p /var/run/sshd
#生产公钥(私钥)对
RUN    ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key
RUN    ssh-keygen -t dsa -f /etc/ssh/ssh_host_dsa_key
RUN    ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key
RUN    ssh-keygen -t ed25519 -f /etc/ssh/ssh_host_ed25519_key
#修改root密码和创建访问容器的账户opuser
RUN    /bin/echo 'root:123456'|chpasswd
RUN    /usr/sbin/useradd admin
RUN    /bin/echo 'admin:123456'|chpasswd
#取消pam模块对登录的限制
RUN    /bin/sed -i 's/.*session.*required.*pam_loginuid.so.*/session optional pam_loginuid.so/g' /etc/pam.d/sshd
#设置默认字符集
RUN    /bin/echo -e "LANG=\"en_US.UTF-8\"" > /etc/default/local
#开发端口
EXPOSE 22
#设置自启动命令
CMD    /usr/sbin/sshd -D
