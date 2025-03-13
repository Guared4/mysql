Vagrant.configure("2") do |config|
  config.vm.define "master" do |master|
    master.vm.box = "bento/ubuntu-22.04"
    master.vm.hostname = "master"
    master.vm.network "private_network", ip: "192.168.57.10"
    master.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 2
    end
  end

  config.vm.define "slave" do |slave|
    slave.vm.box = "bento/ubuntu-22.04"
    slave.vm.hostname = "slave"
    slave.vm.network "private_network", ip: "192.168.57.20"
    slave.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 2
    end
  end

  ssh_pub_key = File.readlines("./id_rsa/id_rsa.pub").first.strip
  config.vm.provision "shell", inline: <<-SHELL
    echo #{ssh_pub_key} >> ~vagrant/.ssh/authorized_keys
    echo #{ssh_pub_key} >> ~root/.ssh/authorized_keys
    sudo sed -i 's/#PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
    systemctl restart sshd
  SHELL
end