$mach_quant = 2

Vagrant.configure("2") do |config|
 
  config.vm.provider "virtualbox" do |v|
      v.gui = false
      v.memory=1024
      v.cpus=1
      v.check_guest_additions=false
  config.vm.box_check_update=false
  config.vm.box="ubuntu/trusty64"
 end

(1..$mach_quant).each do |machine_id|
    config.vm.define "test#{machine_id}" do |node|
        node.vm.network "public_network", ip: "192.168.1.#{10+machine_id}"
        node.vm.hostname = "test#{machine_id}"
        if machine_id == 1
          node.vm.provision :ansible do |ansible|
          ansible.playbook = "provisioning/deploy.yml"
          end
        end
    end
end


end 
