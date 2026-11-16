

Vagrant.configure("2") do |config|

  config.vm.box = "bento/ubuntu-22.04"

  # =====================================
  # PXE SERVER
  # =====================================
  config.vm.define "pxeserver" do |pxe|

    pxe.vm.hostname = "pxeserver"

    # Внутренняя сеть PXE: 10.0.0.0/24
    pxe.vm.network "private_network",
      ip: "10.0.0.20",
      virtualbox__intnet: "pxe"

    # Host-Only сеть управления: 192.168.50.0/24
    pxe.vm.network "private_network",
      ip: "192.168.50.10",
      virtualbox__hostonly: "VirtualBox Host-Only Ethernet Adapter #2"

    pxe.vm.provider "virtualbox" do |vb|
      vb.name = "pxeserver"
      vb.memory = 2048
      vb.cpus = 2
    end
  end

  # =====================================
  # PXE CLIENT
  # =====================================
  config.vm.define "pxeclient" do |client|

    client.vm.hostname = "pxeclient"

    # Внутренняя сеть PXE: DHCP от dnsmasq на сервере
    client.vm.network "private_network",
      type: "dhcp",
      virtualbox__intnet: "pxe"

    client.vm.provider "virtualbox" do |vb|
      vb.name = "pxeclient"
      vb.memory = 10240
      vb.cpus = 2

      # Сначала загрузка по сети
      vb.customize [
        "modifyvm", :id,
        "--boot1", "net",
        "--boot2", "disk",
        "--boot3", "none",
        "--boot4", "none"
      ]
    end
  end

end
