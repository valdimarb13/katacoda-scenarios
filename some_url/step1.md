Configure a multi-node Consul on Docker containers for service discovery and KV storage

# Get docker image
`docker pull consul`{{execute}}

# Verify image
`docker images -f 'reference=consul'`{{execute}}

# Run server
`docker run -d -p 8501:8500 -p 8601:8600/udp --name=server1 consul agent -server -ui -node=server-1 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
# Run another server
`docker run -d -p 8502:8500 -p 8602:8600/udp --name=server2 consul agent -server -ui -node=server-2 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
# Run third server
`docker run -d -p 8503:8500 -p 8603:8600/udp --name=server3 consul agent -server -ui -node=server-3 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}

# Get their IPs
``IPLIST="`docker exec server1 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server2 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server3 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'`"``{{execute}}



