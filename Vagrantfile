box_os  = "generic/centos7"
network_br = "en0: Ethernet 1"

vmlist = [
  { name: "master",
    cpu: 2,
    memory: 2048,
    private_ip: "192.168.56.11",
    public_ip: "192.168.11.91",
    disk_size: "20GB" },

  { name: "worker1",
    cpu: 2,
    memory: 2048,
    private_ip: "192.168.56.12",
    public_ip: "192.168.11.92",
    disk_size: "20GB" },

  { name: "worker2",
    cpu: 2,
    memory: "2048",
    private_ip: "192.168.56.13",
    public_ip: "192.168.11.93",
    disk_size: "20GB" },
]

Vagrant.configure("2") do |config|
  vmlist.each do |spec|
    config.vm.define spec[:name] do |v|
      v.vm.box = box_os
      v.vm.box_check_update = true
      v.vm.boot_timeout = 1000
      v.vm.hostname = spec[:name]
      v.vm.disk :disk, size: spec[:disk_size], primary: true
      v.vm.network :private_network,ip: spec[:private_ip]
      v.vm.network :public_network,ip:  spec[:public_ip], bridge: network_br
      v.vm.provider "virtualbox" do |vbox|
        vbox.gui = false
        vbox.cpus = spec[:cpu]
        vbox.memory = spec[:memory]
        vbox.customize ["modifyvm", :id, "--cableconnected1", "on"]
      end
    end
  end
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
    ansible.inventory_path = "ansible/hosts"
    # ansible.verbose = "-vvv"
  end 
end

