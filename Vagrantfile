# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=2 tw=0 et :

if ENV.has_key?("PLAYBOOK")
   playbook = ENV['PLAYBOOK']
else
  playbook = ""
  print "If you want to provision you have to set PLAYBOOK when run vagrant \n"
end

boxes = [
  {
    :name => "docker",
    :box => "ubuntu/trusty64",
    :ip => '192.168.33.11',
    :cpu => "1",
    :ram => "512"
  }
]

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  boxes.each do |box|
    config.vm.define box[:name] do |vms|
      vms.vm.box = box[:box]
      vms.vm.box_url = box[:url]
      vms.vm.hostname = "#{box[:name]}"

      vms.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--cpus", box[:cpu]]
        v.customize ["modifyvm", :id, "--memory", box[:ram]]
      end

      vms.vm.network :private_network, ip: box[:ip]
      config.vm.network "public_network"

      if playbook != ""
        vms.vm.provision :ansible do |ansible|
          ansible.playbook = "#{playbook}"
          ansible.verbose = "vv"
        end
      end
    end
  end
end
