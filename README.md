docker-mongodb
==============

### Learning Docker http://docker.io/ by creating a mongodb replica container.

### To build:

	Spostarsi nella directory e lanciare il comando
    sudo docker build -t tornabene/docker-mongodb .
	sudo docker build -t tornabene/docker-mongodb-sharding .
### To run localy:

    sudo docker pull tornabene/docker-mongodb
    sudo docker run  -P --name rs1_srv1  -d tornabene/docker-mongodb --replSet rs1  --noprealloc --smallfiles
    sudo docker run  -P --name rs1_srv2  -d tornabene/docker-mongodb --replSet rs1  --noprealloc --smallfiles
    sudo docker run  -P --name rs1_srv3  -d tornabene/docker-mongodb --replSet rs1  --noprealloc --smallfiles
    
    
    sudo docker run  -P --name rs2_srv1  -d tornabene/docker-mongodb --replSet rs2  --noprealloc --smallfiles
    sudo docker run  -P --name rs2_srv2  -d tornabene/docker-mongodb --replSet rs2  --noprealloc --smallfiles
    sudo docker run  -P --name rs2_srv3  -d tornabene/docker-mongodb --replSet rs2  --noprealloc --smallfiles
    
    sudo docker inspect rs1_srv1
    sudo docker inspect rs1_srv2

	on rs1_srv1
	 mongo --port
	 rs.initiate()
	 rs.add("<IP_of_rs1_srv2>:27017")
	 rs.add("<IP_of_rs1_srv3>:27017")
	 rs.status()
     cfg = rs.conf()
	 cfg.members[0].host = "<IP_of_rs1_srv1>:27017"
	 rs.reconfig(cfg)
	 rs.status()
    
    on rs2_srv1
	 mongo --port
	 rs.initiate()
	 rs.add("<IP_of_rs2_srv2>:27017")
	 rs.add("<IP_of_rs2_srv3>:27017")
	 rs.status()
	 cfg = rs.conf()
	 cfg.members[0].host = "<IP_of_rs2_srv1>:27017"
	 rs.reconfig(cfg)
	 rs.status()



	sudo docker run  -P --name cfg1 -d tornabene/docker-mongodb  --noprealloc --smallfiles --configsvr --dbpath /data/db --port 27017
	sudo docker run  -P --name cfg2 -d tornabene/docker-mongodb  --noprealloc --smallfiles --configsvr --dbpath /data/db --port 27017
	sudo docker run  -P --name cfg3 -d tornabene/docker-mongodb  --noprealloc --smallfiles --configsvr --dbpath /data/db --port 27017
	
	sudo docker inspect cfg1
    sudo docker inspect cfg2
    sudo docker inspect cfg3
    
	sudo docker run -P --name mongos1  -d tornabene/docker-mongos --port 27017 --configdb  <IP_of_container_cfg1>:27017, <IP_of_container_cfg2>:27017, <IP_of_container_cfg3>:27017
    
  
    mongo --port 
    sh.addShard("rs1/<IP_of_rs1_srv1>:27017")
    sh.addShard("rs2/<IP_of_rs2_srv1>:27017")
    sh.status()

