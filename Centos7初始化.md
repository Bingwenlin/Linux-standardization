## 执行下面内容
```
#####修改时区#######
setenforce 0
sed -i 's#SELINUX=enforcing#SELINUX=disabled#g' /etc/selinux/config
yum install -y ntp
systemctl enable ntpd
systemctl start ntpd
timedatectl set-timezone Asia/Shanghai
timedatectl set-ntp yes
ntpq -p
####修改系统参数####1
sed -i -e '5d' /etc/security/limits.d/20-nproc.conf
sed -i -e '5d' /etc/security/limits.d/20-nproc.conf
cat  << EOF >>  /etc/security/limits.conf
* hard    nofile           1024000
* soft    nofile           1024000
* hard    nproc            102400
* soft    nproc            102400
EOF
############2
cat << EOF >> /etc/sysctl.conf
# Disable ipv6 on interfaces.
net.ipv6.conf.all.disable_ipv6 = 1
# Controls IP packet forwarding
net.ipv4.ip_forward = 1
#kernel parameter optimization
net.core.rmem_default = 126976
net.core.wmem_default = 126976
net.core.wmem_max = 16777216
net.core.rmem_max = 16777216
net.ipv4.tcp_mem = 8192 87380 16777216
net.ipv4.tcp_wmem = 8192 65536 16777216
net.ipv4.tcp_rmem = 8192 87380 16777216
net.core.netdev_max_backlog = 2500
net.core.somaxconn = 262144
net.ipv4.tcp_no_metrics_save = 0
net.ipv4.tcp_moderate_rcvbuf = 1
net.ipv4.tcp_fin_timeout = 5
net.ipv4.tcp_keepalive_time = 300
net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_sack = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.ip_local_port_range = 10250 65000
net.ipv4.tcp_max_syn_backlog = 81920
net.ipv4.tcp_max_tw_buckets = 1600000
net.ipv4.tcp_synack_retries = 2
net.ipv4.tcp_syn_retries = 2
net.ipv4.tcp_retries2 = 2
net.ipv4.tcp_window_scaling = 1
net.ipv4.tcp_timestamps = 0
fs.file-max = 1024000
net.nf_conntrack_max = 6553500
vm.swappiness = 0
EOF
sysctl -p
ulimit -HSn 1024000
#####防火墙#######
yum -y install net-tools vim iptables-services wget telnet
systemctl disable NetworkManager
systemctl mask firewalld
systemctl stop firewalld
#systemctl enable iptables
#systemctl start iptables
```
