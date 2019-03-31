# -*- mode: ruby -*-
# vim: set ft=ruby :
# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:master => {
            :box_name => "centos/7",
                :net => [
                         {ip: '192.168.100.11', adapter: 2, netmask: "255.255.255.0"},
                         ]
                },
	
	
:slave => {
           :box_name => "centos/7",
               :net => [
                        {ip: '192.168.100.12', adapter: 2, netmask: "255.255.255.0"},
                        ]
              },
	
:proxy => {
           :box_name => "centos/7",
               :net => [
                        {ip: '192.168.100.13', adapter: 2, netmask: "255.255.255.0"},
                        ]
              },


}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
		box.vm.host_name = boxname.to_s

		boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end


		#config.vm.provision "ansible" do |ansible|
		#	ansible.verbose = "vvv"
		#	ansible.playbook = "provisioning/playbook.yml"
		#	ansible.become = "true"
		#end
		
		
		config.vm.provider "virtualbox" do |v|
			v.memory = 256
		end


      end

  end

end

