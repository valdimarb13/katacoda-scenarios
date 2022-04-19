### Get the IPs of all the servers in a list
For connecting the servers to each other we need to know their IP addresses. In a pure consul installation on a VM or through the consul CLI you can simply run the `consul members` command. However, in our case we need to exec into a docker container first and run the command inside each container. 

`docker exec server1 consul members`{{execute}}

`docker exec server2 consul members`{{execute}}

`docker exec server3 consul members`{{execute}}

Now we can run a simple command that adds all these IPs to a variable called IPLIST.

``IPLIST="`docker exec server1 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server2 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server3 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'`"``{{execute}}

### Make sure they are connected to each other
`docker exec server1 consul join $IPLIST`{{execute}}

### Now they are all connected so you can see all three from any one container
`docker exec server3 consul members`{{execute}}