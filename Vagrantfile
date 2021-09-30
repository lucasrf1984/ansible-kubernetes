Vagrant.configure("2") do |config|
	N = 2
    (1..N).each do |machine_id|
      config.vm.define "node" do |node|
		node.vm.box = "centos/7"
		node.vm.hostname = "#{machine_id}.zlkube.com"
		node.vm.network "public_network", ip: "192.168.5.#{11+machine_id}"
		node.vm.synced_folder ".", "/vagrant"
		node.vm.provider "virtualbox" do |vb|
		  vb.memory = 4096
	      vb.cpus = 2
		end
	  end
    end
	if #{machine_id} == N
      config.vm.provision "ansible_local" do |ansible|
         ansible.playbook = "kube_node.yml"
	  end
	end
	
	config.vm.define "master" do |master|
      master.vm.box = "centos/7"
      master.vm.hostname = "master.zlkube.com"
      master.vm.network "public_network", ip: "192.168.5.110"
      master.vm.synced_folder ".", "/vagrant"
      master.vm.provider "virtualbox" do |vb|
       vb.memory = 4096
	   vb.cpus = 2
	  end
    end
    config.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "kube_master.yml"
    end
    config.vm.provision "shell", inline: <<-SHELL
	 cat hosts_ansible > /tmp/node-deploy.yml
	ansible-playbook -i /etc/ansible/hosts /tmp/node-deploy.yml
  SHELL
end