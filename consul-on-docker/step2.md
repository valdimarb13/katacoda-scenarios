#### Get the IPs of all the servers in a list
For connecting the servers to each other we need to know their IP addresses. When using the consul CLI you can simply run the `consul members` command. However, in our case since our Consul servers are inside docker containers we need to `exec` into the container first and run the command inside each container.

`docker exec server1 consul members`{{execute}}

`docker exec server2 consul members`{{execute}}

`docker exec server3 consul members`{{execute}}

We can see the IP for each server is listed in the `Address` column. We can run a simple command that adds all these IPs to a variable called IPLIST which we will use later to join the cluster together.

``IPLIST="`docker exec server1 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server2 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server3 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'`"``{{execute}}

#### Check that IPLIST has correctly obtained three IP addresses
`echo $IPLIST`{{execute}}

#### Connect the cluster
The cluster can be connected by running the `consul join` command and passing all the server IPs that need to be connected into one cluster. Again, we use `docker exec` to step into one container and from there run the join command.

Note: The `consul join` command needs to only be run once and you can run it from any container.

`docker exec server1 consul join $IPLIST`{{execute}}

#### Verify cluster connection
Recall how in the previous step we were check each container to see if the server was running. Now that our cluster is connected we can see the health of all our members from any container. We use `server3` as an example here. Additionally, you can see all three consul installations are of type `server`. This is because when we started the containers we passed the `-server` flag which tells Consul to run these containers as `server` instances.

`docker exec server3 consul members`{{execute}}