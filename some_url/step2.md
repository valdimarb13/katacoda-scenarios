# Get the IPs of all the servers in a list
``IPLIST="`docker exec server1 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server2 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server3 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'`"``{{execute}}

# Make sure they are connected to each other
`docker exec server1 consul join $IPLIST`{{execute}}

# Get IP of Consul leader
``LEADER=`docker exec server1 consul info | grep leader_addr | awk -F= '{print $2}' | awk -F: '{print $1}' | xargs echo` ``{{execute}}

# Start consul agent on ports 8504/8604
`docker run -d -v $(pwd):/consul/config -p 8504:8500 -p 8604:8600/udp --name=agent consul agent -ui -node=agent -join=$LEADER`{{execute}}

# Check agent has joined
`docker exec server1 consul members`{{execute}}