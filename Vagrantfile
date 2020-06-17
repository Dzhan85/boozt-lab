Vagrant.configure("2") do |config|
    # CentOS 8 x64
    # Box supports most of the providers, including libvirt and virtualbox
    config.vm.box = "generic/centos8"
    config.vm.provider :libvirt do |v|
      v.qemu_use_session = false # BZ1697773
                                 # "bug" with private networks not being created
    end
#    config.vm.provider "virtualbox"
#    config.vm.provider "libvirt"

    # main node
    config.vm.define "n0" do |n0|
      n0.vm.hostname = "n0"
      config.vm.provider "libvirt" do |v, override|      # rsync for libvirt
        override.vm.synced_folder './', '/vagrant', type: 'rsync'
      end
      n0.vm.network :private_network, :ip => "192.168.100.101"
      n0.vm.provision :shell, path: "bootstrap.sh" # ansible installation
      n0.vm.provision :shell, inline: "cd /vagrant/ansible/ && ansible-playbook -i inventory.txt deploy-app.yml"
    end

end
