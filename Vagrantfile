# -*- mode: ruby -*-
# vi: set ft=ruby :
#

require 'getoptlong'

opts = GetoptLong.new(
      [ '--managers', GetoptLong::OPTIONAL_ARGUMENT ],
      [ '--workers', GetoptLong::OPTIONAL_ARGUMENT ]
)

numOfManagers=1
numOfWorkers=3

opts.each do |opt, arg|
  case opt
    when '--managers'
      numOfManagers=arg.to_i
    when '--workers'
      numOfWorkers=arg.to_i
  end
end

puts <<-EOD
Starting Docker Swarm mode.
Managers: #{numOfManagers}
Workers : #{numOfWorkers} 

EOD

@initManager = <<EOD
echo Arguments: $*
if [ ! -d "/vagrant/swarm-token" ]; then
    mkdir /vagrant/swarm-token 
    chmod 777 /vagrant/swarm-token
    
    docker swarm init --advertise-addr eth1:2377
    docker swarm join-token -q manager > /vagrant/swarm-token/manager
    docker swarm join-token -q worker > /vagrant/swarm-token/worker
else
    docker swarm join \
      --token `cat  /vagrant/swarm-token/manager` \
      192.168.50.100:2377
fi
EOD

@initWorker = <<EOD
docker swarm join \
  --token `cat  /vagrant/swarm-token/worker` \
  192.168.50.100:2377
EOD

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"

  (1..numOfManagers).each do |mgrNumber|
      config.vm.define "swarm-manager-#{mgrNumber}" do |node|
          node.vm.hostname = "swarm-manager-#{mgrNumber}"
          node.vm.network "private_network", ip: "192.168.50.#{99+mgrNumber}"
          node.vm.provision "shell", inline: "sudo apt-get update"
          node.vm.provision "docker"
          node.vm.provision "shell", inline: @initManager, args: [ "#{numOfManagers}" , "192.168.50.#{99+mgrNumber}" ]
      end
  end

  (1..numOfWorkers).each do |workerNumber|
    config.vm.define "swarm-worker-#{workerNumber}" do |node|
      node.vm.hostname = "swarm-worker-#{workerNumber}"
      node.vm.network "private_network", ip: "192.168.50.#{149+workerNumber}"
      node.vm.provision "shell", inline: "sudo apt-get update"
      node.vm.provision "docker"
      node.vm.provision "shell", inline: @initWorker
    end
  end
end
