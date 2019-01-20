
Vagrant.configure("2") do |vagrant|
  vagrant.vm.define "dns" do |config|
    config.vm.hostname = "dns"

    config.vm.box = "bento/ubuntu-18.10"
    #config.vm.box = "bryanhuntesl/alpine64"
    #config.vm.box_version = "3.8.2"

    config.vm.provider "vmware_desktop" do |v|
      v.vmx["memsize"] = "512"
      v.vmx["numvcpus"] = "1"

      v.gui = false
    end

    config.vm.network "public_network", ip: "192.168.0.2"

    #config.ssh.shell = "sh"
    #config.vm.provision "shell", inline: <<-SHELL
    #  apk update
    #  apk --no-cache add dnsmasq
    #SHELL

    config.vm.provision "shell", inline: <<-SHELL
      systemctl disable systemd-resolved
      systemctl stop systemd-resolved

      rm -rf /etc/resolv.conf

      echo "nameserver 8.8.8.8" > /etc/resolv.conf
      
      apt-get update -qq
      apt-get install -qq -y dnsmasq
    SHELL
  end

  vagrant.vm.define "esxi1" do |config|
    config.vm.hostname = "esxi1"

    config.vm.box = "vmware/esxi"
    config.vm.box_version = "6.7.0-8169922"

    config.vm.provider "vmware_desktop" do |v|
      v.vmx["memsize"] = "4096"
      v.vmx["numvcpus"] = "4"

      v.gui = true
    end

    config.vm.network "public_network", ip: "192.168.0.80"
  end
end
