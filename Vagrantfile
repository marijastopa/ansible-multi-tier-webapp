Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  
  # Load Balancer
  config.vm.define "lb-dev-01" do |lb|
    lb.vm.hostname = "lb-dev-01"
    lb.vm.network "private_network", ip: "192.168.56.10"
    lb.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
    lb.vm.network "forwarded_port", guest: 8080, host: 8081, host_ip: "127.0.0.1"
    lb.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
  end

  # Web Server 1
  config.vm.define "web-dev-01" do |web|
    web.vm.hostname = "web-dev-01"
    web.vm.network "private_network", ip: "192.168.56.20"
    web.vm.network "forwarded_port", guest: 80, host: 8082, host_ip: "127.0.0.1"
    web.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
  end

  # Web Server 2
  config.vm.define "web-dev-02" do |web|
    web.vm.hostname = "web-dev-02"
    web.vm.network "private_network", ip: "192.168.56.21"
    web.vm.network "forwarded_port", guest: 80, host: 8083, host_ip: "127.0.0.1"
    web.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
  end

  # Provision SSH keys for Ansible
  config.vm.provision "shell", inline: <<-SHELL
    sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    systemctl restart sshd
  SHELL
end