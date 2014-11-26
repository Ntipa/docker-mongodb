docker-mongodb
==============

### Learning Docker http://docker.io/ by creating a mongodb replica container.

### To build:

	Spostarsi nella directory e lanciare il comando
    sudo docker build -t tornabene/docker-mongodb .
### To run:

    sudo docker pull tornabene/docker-mongodb
    sudo mkdir -p /opt/ntipa/mongodb_data
    
    sudo docker run  -p 27017:27017 --name mongo.ntipa.it   -d    tornabene/docker-mongodb
    sudo docker run  -p 27017:27017 --name testmongo   -d  -v  /opt/ntipa/mongodb_data:/data/db   tornabene/docker-mongodb
    
    sudo docker run  -P --name rs1_srv1  -d tornabene/docker-mongodb --replSet rs1  --noprealloc --smallfiles
    sudo docker run  -P --name rs1_srv2  -d tornabene/docker-mongodb --replSet rs1  --noprealloc --smallfiles
    sudo docker run  -P --name rs1_srv3  -d tornabene/docker-mongodb --replSet rs1  --noprealloc --smallfiles
    
    
    sudo docker run  -P --name rs2_srv1  -d tornabene/docker-mongodb --replSet rs2  --noprealloc --smallfiles
    sudo docker run  -P --name rs2_srv2  -d tornabene/docker-mongodb --replSet rs2  --noprealloc --smallfiles
    sudo docker run  -P --name rs2_srv3  -d tornabene/docker-mongodb --replSet rs2  --noprealloc --smallfiles
    
    sudo docker inspect rs1_srv1
    sudo docker inspect rs1_srv2
    sudo docker inspect rs1_srv3

	on rs1_srv1
	 mongo --port
	 rs.initiate();
	 rs.add("<IP_of_rs1_srv2>:27017");
	 rs.add("<IP_of_rs1_srv3>:27017")
	 rs.status()
     cfg = rs.conf()
	 cfg.members[0].host = "<IP_of_rs1_srv1>:27017"
	 rs.reconfig(cfg)
	 rs.status()
    
    sudo docker inspect rs2_srv1
    sudo docker inspect rs2_srv2
    sudo docker inspect rs2_srv3
    
    on rs2_srv1
	 mongo --port
	 rs.initiate()
	 rs.add("<IP_of_rs2_srv2>:27017")
	 rs.add("<IP_of_rs2_srv3>:27m017")
	 rs.status()
	 cfg = rs.conf()
	 cfg.members[0].host = "<IP_of_rs2_srv1>:27017"
	 rs.reconfig(cfg)
	 rs.status()


