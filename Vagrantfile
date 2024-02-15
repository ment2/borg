Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.vm.box_version = "2004.01"
  
  config.vm.provider "virtualbox" do |v|
    v.memory = 256
    v.cpus = 1
  end

  config.vm.define "backup" do |backup|
    backup.vm.network "private_network", ip: "192.168.56.100", virtualbox__intnet: "net1"
    backup.vm.hostname = "backup"

    backup.vm.provider "virtualbox" do |v|
      v.customize ['createhd', '--filename', '6backup_disks.vdi', '--size', 2048]
      v.customize ['storageattach', :id, '--storagectl', 'IDE', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', '6backup_disks.vdi']
    end
  end

  config.vm.define "client" do |client|
    client.vm.network "private_network", ip: "192.168.56.150", virtualbox__intnet: "net1"
    client.vm.hostname = "client"
  end
end
