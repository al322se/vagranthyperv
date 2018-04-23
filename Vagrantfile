# -*- mode: ruby -*-
# vi: set ft=ruby :

unless Vagrant.has_plugin?('vagrant-triggers')
  `vagrant plugin install vagrant-triggers`
end

# TODO: pull these from ENV with defaults
# HYPER_V_SWITCH = 'NATWotwSsh'
# HYPER_V_SWITCH_IP_FIRST_THREE = '10.47.1'

HYPER_V_SWITCH = 'Default Switch'

Vagrant.configure('2') do |config|
  # Be careful; triggers are run for each machine
  # if Vagrant::Util::Platform.windows?
  #   [:up, :reload].each do |command|
  #     config.trigger.before command do
  #       run "powershell.exe powershell/network.ps1 -Switch #{HYPER_V_SWITCH} -LeadingThree #{HYPER_V_SWITCH_IP_FIRST_THREE} -Create"
  #     end
  #   end
  #   config.trigger.after :destroy do
  #     run "powershell.exe powershell/network.ps1 -Switch #{HYPER_V_SWITCH} -Destroy"
  #   end
  # end

  config.vm.define 'host1' do |host1|
  host1.vm.hostname = 'host1'
  host1.vm.box = "hashicorp/precise64"
  host1.vm.network 'private_network', type: 'dhcp'

    host1.vm.provider 'hyperv' do |hyperv|
      hyperv.vmname = host1.vm.hostname
      config.vm.network 'public_network', bridge: HYPER_V_SWITCH
    end
  
      # Doesn't work; folders are mounted pre-provisioning
      # See https://seven.centos.org/2017/09/updated-centos-vagrant-images-available-v1708-01/
      # Run in this order to fix:
      # vagrant up
      # vagrant provision
      # vagrant reload --provision
      # These were vetted on RHEL and Debian; dunno about others
      mount_packages = ['nfs-utils', 'nfs-utils-lib']
      mount_type = 'nfs'
      if Vagrant::Util::Platform.windows?
        mount_packages = ['cifs-utils']
        mount_type = 'smb'
      end

      # You might have to replace rpm and yum with your distro's tools
      host1.vm.provision 'shell' do |sh|
        sh.inline = "rpm -qa | egrep -i \'#{mount_packages.join('|')}\' || yum install -y #{mount_packages.join(' ')}"
        sh.privileged = true
      end
      controller.vm.synced_folder './provisioning/', '/tmp/provisioning', type: mount_type
      # End doesn't work

    host1.vm.provision 'ansible_local' do |ansible|
      ansible.playbook = 'initial-setup.yml'
      # TODO read about
      ansible.install_mode = 'pip'
      ansible.limit = host1.vm.hostname
      ansible.provisioning_path = '/tmp/provisioning'
      ansible.inventory_path = '/tmp/provisioning/inventory'
    end
    
  end
end
