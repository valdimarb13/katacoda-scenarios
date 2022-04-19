#### Verify the counting.json in the container
We can check that our changes to counting.json have actually showed up in the container. After running the docker exec command below we should see the same contents of the file in the container.

`docker exec agent cat /consul/config/counting.json`{{execute}}

#### Reload config
This command tells the consul agent to reload its configurations. This way the consul agent is able to pull the latest configurations and apply them.

`docker exec agent consul reload`{{execute}}

#### Verify that our service is registered 
We can see our service is registered and showing up in Consul.

`docker exec agent consul catalog services`{{execute}}

#### Use Consul DNS to see service is now registered
When a service is registered with Consul it becomes reachable on Consul's default domain (assuming you haven't changed it). This follows the syntax of `name.service.consul` where `name` is what we have registered as the name for the service in its configuration file. In our case, since our counting app is registered with the name `counting` it should be available via Consul on `counting.service.consul`. We can do a small DNS query to see if our service is available via Consul. 

`dig +short @127.0.0.1 -p 8601 counting.service.consul`{{execute}}

We can see some more domain data with the nslookup command to confirm our service is working.

`nslookup counting.service.consul`{{execute}}

If successful, it should return the IP of one of the Consul servers. You've deployed Consul using Docker containers, used KV storage, and registered a service successfully!