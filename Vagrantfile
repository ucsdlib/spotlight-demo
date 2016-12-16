# -*- mode: ruby -*-
# vi: set ft=ruby :

# Reasonable Defaults - can be overwridden with environmental variables
IP_NETWORK=ENV.fetch('IP_NETWORK','172.16.1')
DEFAULT_BOX=ENV.fetch('DEFAULT_BOX', 'centos/7')

# List guests here, one per line.
# :name, name of the box
# :box (optional), box to build from (default DEFAULT_BOX)
# :ip (optional), IP address for local networking. Can be full dotted quad
#     or the last octet, which will be appended to the IP_NETWORK environmental
#     variable. Default none.
# If there are multiple systems, the first one will be marked "primary"
guests = [
  { :name => 'spotlight', :box => 'ubuntu/trusty64', :ip => '172.16.21.2', :memory=>'1024', },
]

Vagrant.configure(2) do |config|
  guests.each_with_index do |guest, i|
    config.vm.define "#{guest[:name]}", primary: i==0 do |box|
      box.vm.box = ( guest.key?(:box) ? guest[:box] : DEFAULT_BOX )
      box.vm.synced_folder '.', '/vagrant', disabled: true
      box.vm.box_check_update = false
      if guest.has_key?(:ip)
        box.vm.network 'private_network',
          ip: guest[:ip].match('\.') ? guest[:ip] : "#{IP_NETWORK}.#{guest[:ip]}"
      end
      box.vm.provider "virtualbox" do |v|
         v.memory = guest[:memory] if guest.has_key?(:memory)
      end
    end
  end
end
