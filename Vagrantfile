## config
numnodes = 2
## end config

info = <<-'EOF'

                                                                                                                  
    sssssssssswwwwwww           wwwww           wwwwwwwaaaaaaaaaaaaa  rrrrr   rrrrrrrrr      mmmmmmm    mmmmmmm   
  ss::::::::::sw:::::w         w:::::w         w:::::w a::::::::::::a r::::rrr:::::::::r   mm:::::::m  m:::::::mm 
ss:::::::::::::sw:::::w       w:::::::w       w:::::w  aaaaaaaaa:::::ar:::::::::::::::::r m::::::::::mm::::::::::m
s::::::ssss:::::sw:::::w     w:::::::::w     w:::::w            a::::arr::::::rrrrr::::::rm::::::::::::::::::::::m
 s:::::s  ssssss  w:::::w   w:::::w:::::w   w:::::w      aaaaaaa:::::a r:::::r     r:::::rm:::::mmm::::::mmm:::::m
   s::::::s        w:::::w w:::::w w:::::w w:::::w     aa::::::::::::a r:::::r     rrrrrrrm::::m   m::::m   m::::m
      s::::::s      w:::::w:::::w   w:::::w:::::w     a::::aaaa::::::a r:::::r            m::::m   m::::m   m::::m
ssssss   s:::::s     w:::::::::w     w:::::::::w     a::::a    a:::::a r:::::r            m::::m   m::::m   m::::m
s:::::ssss::::::s     w:::::::w       w:::::::w      a::::a    a:::::a r:::::r            m::::m   m::::m   m::::m
s::::::::::::::s       w:::::w         w:::::w       a:::::aaaa::::::a r:::::r            m::::m   m::::m   m::::m
 s:::::::::::ss         w:::w           w:::w         a::::::::::aa:::ar:::::r            m::::m   m::::m   m::::m
  sssssssssss            www             www           aaaaaaaaaa  aaaarrrrrrr            mmmmmm   mmmmmm   mmmmmm
                                                                                                                  
                                                                                                                  

to connect to docker daemon in the vm swarm1 use:
export DOCKER_HOST=tcp://localhost:2375

then run:
docker node ls

go to the webpage and play: http://localhost:9000

EOF

## don't modify
lan = "192.168.99"
Vagrant.configure("2") do |config|
  config.vm.box = "alvaro/xenial64"
  #to avoid Inappropriate ioctl for device in messages
  #config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 2
  end
  numnodes.downto(1).each do |i|
    config.vm.define vm_name = "swarm%01d" % i do |swarm|
      #hostname
      swarm.vm.hostname = vm_name
      #network
      swarm.vm.network :private_network, ip: "#{lan}.#{100+i}"
      #setup ssh passwordless
      swarm.vm.provision "shell", privileged: true, path: "ops/sshkeys.sh", args: numnodes
      #setup packaes and docker
      swarm.vm.provision "shell", privileged: true, path: "ops/provision.sh", args: numnodes

      # only of first node
      if i==1 
        swarm.vm.provision "shell", path: "ops/swarm1.sh", args: numnodes
        # port forward to easy access for docker
        swarm.vm.network "forwarded_port", guest: 2375, host: 2375 #docker
        swarm.vm.network "forwarded_port", guest: 9000, host: 9000 #portainer
        puts info if ARGV[0] == "status"
      end
    end
  end
end
