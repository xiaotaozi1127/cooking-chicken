Vagrant.configure("2") do |config| #2代表配置文件的版本
  #对于每台虚拟机的统一配置
  config.vm.box = "ubuntu/trusty64"
  config.vm.provider :virtualbox do |v|
    v.customize ["modifyvm", :id, "--memory", 1024]
    v.customize ["modifyvm", :id, "--cpus", 1]
  end

  #对于每台虚拟机依赖的Ansible配置
  config.vm.provision :ansible do |ansible|
    ansible.verbose = "vv"
    ansible.playbook = "playbooks/deploy.yml"
    ansible.inventory_path = "playbooks/hosts"
  end

  #定义名为Nginx的主机，在宿主机上可直接访问192.168.56.101来访问Nginx
  config.vm.define "nginx" do |config|
    config.vm.hostname = 'nginx'
    config.vm.network :private_network, ip: "192.168.56.101"

    config.vm.provision :host_shell do |host_shell|
      host_shell.inline = 'vagrant/bootstrap.sh' #在Jenkins虚拟机执行Ansible之前将私钥放到Jenkins的file文件夹中
    end
  end

  #定义名为Jenkins的主机，在宿主机上可直接访问192.168.56.102来访问Jenkins
  config.vm.define "jenkins" do |config|
    config.vm.hostname = 'jenkins'
    config.vm.network :private_network, ip: "192.168.56.102"
  end
end
