# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :otuslinux => {
        :box_name => "centos/7",
        :ip_addr => '192.168.10.30',
    :disks => {
	:sata1 => {
	    :dfile => '~/VirtualBox VMs/sata1.vdi',
	    :size => 250,
	    :port => 1
	},
	:sata2 => {
                        :dfile => '~/VirtualBox VMs/sata2.vdi',
                        :size => 250, # Megabytes
	    :port => 2
	},
        :sata3 => {
                        :dfile => '~/VirtualBox VMs/sata3.vdi',
                        :size => 250,
                        :port => 3
        },
        :sata4 => {
                        :dfile => '~/VirtualBox VMs/sata4.vdi',
                        :size => 250, # Megabytes
                        :port => 4
        },
        :sata5 => {
                        :dfile => '~/VirtualBox VMs/sata5.vdi',
                        :size => 250, # Megabytes
                        :port => 5
        }    
    }
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s

          #box.vm.network "forwarded_port", guest: 3260, host: 3260+offset

          box.vm.network "private_network", ip: boxconfig[:ip_addr]

          box.vm.provider :virtualbox do |vb|
        	  vb.customize ["modifyvm", :id, "--memory", "2048"]
                  needsController = false
	  boxconfig[:disks].each do |dname, dconf|
	      unless File.exist?(dconf[:dfile])
		vb.customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
                                needsController =  true
                          end

	  end
                  if needsController == true
                     vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata" ]
                     boxconfig[:disks].each do |dname, dconf|
                         vb.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
                     end
                  end
          end
      box.vm.provision "shell", inline: <<-SHELL
          mkdir -p ~root/.ssh
          cp ~vagrant/.ssh/auth* ~root/.ssh
          yum install -y mdadm gdisk

          mdadm --create --verbose /dev/md0 -l 5 -n 5 /dev/sd{b,c,d,e,f}
          mkdir /etc/mdadm
          echo "DEVICE partitions" > /etc/mdadm/mdadm.conf
          mdadm --detail --scan --verbose | awk '/ARRAY/ {print}' >> /etc/mdadm/mdadm.conf
      SHELL

      end
  end
end