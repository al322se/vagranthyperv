
Vagrant.configure("2") do |config|

  config.vm.define "host1" do |host1|
    host1.vm.box = "hashicorp/precise64"
    host1.vm.network "private_network", type: "dhcp"
    host1.vm.synced_folder ".", "/host1",type: "smb", smb_username: "Semen", smb_password: "password"
    host1.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.limit = "all"
    end
  end

  config.vm.define "host2" do |host2|
    host2.vm.box = "hashicorp/precise64"
    host2.vm.network "private_network", type: "dhcp"
    host2.vm.synced_folder ".", "/host2", type: "smb", smb_username: "Semen", smb_password: "password"

    host2.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.limit = "all"
    end
  end
  config.vm.define "host3" do |host3|
    host3.vm.box = "hashicorp/precise64"
    host3.vm.network "private_network", type: "dhcp"
    host3.vm.synced_folder ".", "/host3", type: "smb", smb_username: "Semen", smb_password: "password"
    host3.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.limit = "all"
    end
  end

end
