# https://portal.cloud.hashicorp.com/vagrant/discover/cloud-image?prev=CiBXeUp2Y0dWdWMzVnpaUzFzWldGd0xURTFMallpWFE9PQ%3D%3D
Vagrant.configure("2") do |config|
  config.vm.box = "cloud-image/ubuntu-24.04"

  config.vm.provider :libvirt do |libvirt|
    libvirt.driver = "kvm"
    libvirt.uri = "qemu:///system"
    libvirt.memory = 2048
    libvirt.cpus = 2
    libvirt.storage_pool_name = "pool"
    libvirt.default_prefix = "ubuntu"
  end

  config.vm.provision "shell", inline: <<-SHELL
    useradd -m -s /bin/bash setup
    echo "setup:12345678Aa!" | chpasswd
    usermod -aG sudo setup
  SHELL
end
