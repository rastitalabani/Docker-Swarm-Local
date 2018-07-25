#################################
# prepare hosts list 
#################################
require 'yaml'

$hosts = {}
$vagrant_image =  "ubuntu"
$image_version = "/xenial64"
file = "./ansible/inventories/hosts"
prev_line = ""


File.readlines(file).each do |line|
    
  if  line.length != 1 && line.match(/^(?:[0-9]{1,3}\.){3}[0-9]{1,3}$/) then
    if line =~ /\n/ then
      line = line.sub!(/\n/,'')
    end

    prev_line = prev_line.sub!(/\n/,'')
    $hosts["#{prev_line.sub('[','').sub(']','')}"] = "#{line}"
  end

  prev_line = line
end

puts $hosts

#################################
# Vagrant  
#################################
Vagrant.configure("2") do |config|


  # #configure proxy on VM machines
  # if Vagrant.has_plugin?("vagrant-proxyconf")
  #   config.proxy.http     = ""
  #   config.proxy.https    = ""
  #   config.proxy.no_proxy = ""
  # end

  $hosts.each do |key, value|
    config.vm.define key do |m|
      m.vm.box = $vagrant_image + $image_version
      m.vm.network "private_network", :ip => value, :name => 'vboxnet0', :adapter => 2
      m.vm.hostname = key

      m.vm.provider "virtualbox" do |vb|
        vb.name = key
      end

      #configure ssh keys on guests for SSH connection, to be used by ansible
      m.vm.provision "file", source: "./keys/id_rsa.pub", destination: "~/.ssh/ansible.pub"
      m.vm.provision "shell", inline: <<-SHELL
      count=$(less /home/vagrant/.ssh/authorized_keys | grep -wc ssh-rsa)
        if [ $count = 1 ]
        then
          cat /home/vagrant/.ssh/ansible.pub >> /home/vagrant/.ssh/authorized_keys
          rm /home/vagrant/.ssh/ansible.pub
        fi
      SHELL
      
    end
  end
end 
