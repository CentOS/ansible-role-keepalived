# vi: set ft=ruby :

boxes = [
    {
        :name => "keepalived1",
        :eth0 => "192.168.33.11",
        :image => "centos/7",
    },
    {
        :name => "keepalived2",
        :eth0 => "192.168.33.12",
        :image => "centos/7",
    },
    {
        :name => "client",
        :eth0 => "192.168.33.13",
        :image => "centos/7",
    }
]

Vagrant.configure(2) do |config|

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--memory", 1024]
    v.customize ["modifyvm", :id, "--cpus", 1]
  end

  boxes.each do |boxopts|
    config.vm.define boxopts[:name] do |config|
        config.vm.box = boxopts[:image]
      config.vm.provider :libvirt do |domain|
        domain.cpu_mode = 'host-passthrough'
      end
      config.vm.hostname = boxopts[:name]
      config.vm.network :private_network, ip: boxopts[:eth0]
      # Vagrant works serially and provision machines
      # serially. Each of them is unaware of the others.
      # Therefore, we should start provisioning only on last machine
      if boxopts[:name] == "keepalived2"
        config.vm.provision :ansible do |ansible|
          ansible.playbook = "tests/provision.yml"
          ansible.limit = 'ci-gateway-nodes'
          ansible.verbose = "-vv"
          ansible.groups = {
              "ci-gateway-nodes" => ["keepalived1", "keepalived2"]
          }
        end
      end
    end
  end
end
