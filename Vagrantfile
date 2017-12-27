# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "fedora/25-cloud-base"
  config.vm.box_url = "https://mirrors.tuna.tsinghua.edu.cn/fedora/releases/25/CloudImages/x86_64/images/Fedora-Cloud-Base-Vagrant-25-1.3.x86_64.vagrant-virtualbox.box"
  config.vm.box_download_checksum = "390addeab171fadec4d5901e9eb2054bd955efeebc6a8b2231c7881803245d38"
  config.vm.box_download_checksum_type = :sha256

  config.vm.network "private_network", ip: "192.168.55.33"
  config.vm.synced_folder ".", "/vagrant", create: true, nfs: true
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL
    dnf install -y git redhat-rpm-config
	
	
    curl --silent --location https://rpm.nodesource.com/setup_7.x | bash -
    yum -y install nodejs
    yum groupinstall -y 'Development Tools'
	
	
    type -fp python3.6 &>/dev/null || (
      dnf install -y python36
      python3.6 -mensurepip
    )

    type -fp mysql &>/dev/null || (
      dnf install -y mariadb-server mariadb-devel
      systemctl enable mariadb
      systemctl start mariadb
      mysqladmin -u root password vagrant
    )

    [[ -f /etc/pip.conf ]] || cat > /etc/pip.conf <<'EOF'
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
EOF

    pip3.6 install ipython bpython
  SHELL
end
