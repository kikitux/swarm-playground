# openswarm-playground
a vagrant project to get docker swarm on your laptop


dependencies:
- virtualbox
- vagrant
- docker binaries

## TL;DR

```
vagrant up
vagrant status
```

## Number of nodes

By default the cluster is made of 2 nodes, however you can [update this on Vagrantfile](https://github.com/kikitux/swarm-playground/blob/master/Vagrantfile#L2) to have multiples nodes

```
## config
numnodes = 5
## end config
```

## Long description of usage

Once all the dependencies are installed, the following will happen:

vagrant will create a two (2) node multi-machine setup, and install docker

then will create a swarm cluster, make the 2nd node join

`vagrant up` creates the vms
`vagrant status` will display the current status of the vms and some information

```
0  (master) $ vagrant status

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

go to the webpage and play: http://localhost:8080

Current machine states:

swarm2                     running (virtualbox)
swarm1                     running (virtualbox)

This environment represents multiple VMs. The VMs are all listed
above with their current state. For more information about a specific
VM, run `vagrant status NAME`.
0  (master) $ 
```

You can configure `DOCKER_HOST` variable so docker client on your computer can talk to the daemon on the vm
```
export DOCKER_HOST=tcp://localhost:2375
0  (master) $ docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
iilumy3xqznbp5hmuvimpmlg5 *   swarm1               Ready               Active              Leader
4haixnr636ymahj8nnu5z1mof     swarm2               Ready               Active              
0  (master) $ 
```

## desctiption of whats here

Vagrant is a tool to create repeatable environments so the developer can focus on the code instead of the infrastructure.

This project will do on start:
- download vm template
- configure ssh password less between the nodes
- deploy guest, 2 by default, you can scale to more
- install and configure docker
- create docker swarm cluster

on node1 swarm1:
- install portainer

## extra stuff

### portainer

portainer is a ui to visualise and manage docker hosts and integrate with swarm.

![cluster](https://github.com/kikitux/openswarm-playground/raw/master/screenshots/portainer4.png)

head to [http://localhost:9000/](http://localhost:9000/) and setup the initial password



