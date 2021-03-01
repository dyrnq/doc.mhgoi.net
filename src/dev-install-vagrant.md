

# Vagrant

Vagrant可以理解为针对虚拟机的`docker`，可以方便拉取别人制作好的虚机镜像后启动一个虚拟机实例。

通常我们可以不用自己安装虚机，可以像`docker pull`一样从Vagrant的app仓库拉取操作系统官方制作好的box（镜像），ubuntu和centos都有官方自己制作的镜像。

Vagrant不仅可以操控virtualbox，还可以操控vmware等其他虚拟化方案。

## install

文档 <https://www.vagrantup.com/downloads>


mac下安装Vagrant可以通过brew安装


```bash
brew cask install vagrant
```

## 下载vbox镜像


为本地仓库添加box方法有2种，box是vagrant的镜像。

* 一种是先使用curl或者wget等工具下载镜像，再添加。


```bash
## .box文件比较大，也可以下载下来，再调用vagrant box add
wget --tries 100 --continue --output-document debian10.box  https://app.vagrantup.com/debian/boxes/buster64/versions/10.4.0/providers/virtualbox.box

vagrant box add debian10 ./debian10.box
```

* 另外可以通过`vagrant box add` 命令后边+box的http地址。

```bash
# debian buster64
vagrant box add debian10 https://app.vagrantup.com/debian/boxes/buster64/versions/10.4.0/providers/virtualbox.box

## centos7
vagrant box add centos7 https://mirrors.ustc.edu.cn/centos-cloud/centos/7/vagrant/x86_64/images/CentOS-7-x86_64-Vagrant-2004_01.VirtualBox.box

## centos8
vagrant box add centos8 https://mirrors.ustc.edu.cn/centos-cloud/centos/8/vagrant/x86_64/images/CentOS-8-Vagrant-8.3.2011-20201204.2.x86_64.vagrant-virtualbox.box

## ubuntu1804
vagrant box add ubuntu1804 https://mirrors.tuna.tsinghua.edu.cn/ubuntu-cloud-images/bionic/current/bionic-server-cloudimg-amd64-vagrant.box

## ubuntu2004
vagrant box add ubuntu2004 https://mirrors.tuna.tsinghua.edu.cn/ubuntu-cloud-images/server/focal/current/focal-server-cloudimg-amd64-vagrant.box
```

* 更多os镜像 [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search)

## 创建Vagrantfile

Vagrantfile是一个虚机实例的配置，比如cpu多少核、内存多大啊

```bash
mkdir vmworkspace
cd vmworkspace
vi Vagrantfile
```

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|
  config.vm.box_check_update = false

  [11,12].each do |i|
    config.vm.define "n#{i}" do |node|
      node.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--ioapic", "on"]
        vb.memory = "2048"
        vb.cpus = 2        
      end

      node.vm.box = "ubuntu2004"
      #node.vm.network :forwarded_port, guest: 22, host: "2#{i}22", host_ip: "0.0.0.0", auto_correct:true

      node.vm.provision "shell", inline: <<-SHELL
        #########################################################################
        echo "root:vagrant" | sudo chpasswd
        #########################################################################
        swapsize=4000

        # does the swap file already exist?
        grep -q "swapfile" /etc/fstab

        # if not then create it
        if [ $? -ne 0 ]; then
          echo 'swapfile not found. Adding swapfile.'
          fallocate -l ${swapsize}M /swapfile
          chmod 600 /swapfile
          mkswap /swapfile
          swapon /swapfile
          echo '/swapfile none swap defaults 0 0' >> /etc/fstab
        else
          echo 'swapfile found. No changes made.'
        fi
        #######################################################################
                
        mkdir -p /data

        #######################################################################
        sed -i "s@http://.*archive.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list && \
        sed -i "s@http://.*security.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list && \
        DEBIAN_FRONTEND=noninteractive apt-get update && \
        DEBIAN_FRONTEND=noninteractive apt-get -y upgrade && \
        DEBIAN_FRONTEND=noninteractive apt install -yqq curl
        #######################################################################

        curl -fsSL https://ghproxy.com/https://raw.githubusercontent.com/dyrnq/install-docker/main/install-docker.sh | bash -s docker --mirror tencent --version 20.10.3 --with-compose --compose-version 1.28.2 --compose-mirror daocloud
        #######################################################################
        sudo usermod -aG docker vagrant
      SHELL


    end
  end

end

```

## 启动
```bash
vagrant up n11
vagrant ssh n11
```

<font color=red size=3>使用Vagrant的好处是可以快速的重新获取一个全新的既定的vm</font>，比如我用下边这条语句

```bash
vagrant destroy n11 -f && vagrant up n11 && vagrant ssh n11 
```

表示强制销毁n11---->重新建立n11---->ssh登录到n11

## ref

* [Vagrant搭建开发测试环境](https://codebays.com/server/171.html)
* [Vagrant使用国内镜像安装插件和box镜像](https://blog.dteam.top/posts/2020-04/vagrant-use-mirror.html)
* [Updated CentOS Vagrant Images Available (v2004.01)](https://blog.centos.org/2020/05/updated-centos-vagrant-images-available-v2004-01/)