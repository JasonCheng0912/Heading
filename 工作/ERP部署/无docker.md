 
如果在无docker环境下执行：

```shell
//生成ssh 证书，做本机到本机的免密
ssh-keygen -t rsa -f /root/.ssh/id_rsa -N ''
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

// 测试免密是否生效
ssh 本机IP


yum -y install git telnet epel-release lsof vim
yum -y install ansible

git clone http://github.app.hd123.cn:10080/qianfanops/iac.git -b develop
cd iac

修改 inventory/centos_init

# 修改 inventory/centos_init 文件，配置主机名以及IP 密码(如果本机到本机或到目标机器做了免密，密码可忽略)
ansible-playbook -i inventory/centos_init hdiac/centos_init/init.yml --tags centos_int
```


 