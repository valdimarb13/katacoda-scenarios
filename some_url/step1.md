Configure a multi-node Consul on Docker containers for service discovery and KV storage

# Get docker image
`docker pull consul`{{execute}}

# Verify image
`docker images -f 'reference=consul'`{{execute}}


# Config file

# Run server
`docker run -d -p 8501:8500 -p 8601:8600/udp --name=server1 consul agent -server -ui -node=server-1 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
# Run another server
`docker run -d -p 8502:8500 -p 8602:8600/udp --name=server2 consul agent -server -ui -node=server-2 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
# Run third server
`docker run -d -p 8503:8500 -p 8603:8600/udp --name=server3 consul agent -server -ui -node=server-3 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}

# Get their IPs
``IPLIST="`docker exec server1 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server2 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'` `docker exec server3 consul members | awk '{print $2}' | awk 'FNR==2 {print}' | awk -F: '{print $1}'`"``{{execute}}

# Make sure they are connected to each other
`docker exec server1 consul join $IPLIST`{{execute}}

# Get Leader IP
LEADER=`docker exec server1 consul info | grep leader_addr | awk -F= '{print $2}' | awk -F: '{print $1}' | xargs echo`

# Start consul agent
docker run -d -v $(pwd):/consul/config -p 8504:8500 -p 8604:8600/udp --name=agent consul agent -ui -node=agent -join=$LEADER

# Check agent has joined
docker exec server1 consul members

# Pull a small sample app
docker pull hashicorp/counting-service:0.0.2

# Run the app
docker run -p 9001:9001 -d --name=app hashicorp/counting-service:0.0.2

# Create counting.json for app to register with
# This file automatically show up inside the container since we have mounted the volume
touch counting.json

# Register the app by adding the service in /consul/config directory
docker exec agent /bin/sh -c "echo '{\"service\": {\"name\": \"counting\", \"tags\": [\"go\"], \"port\": 9001}}' >> /consul/config/counting.json"

# Reload config
docker exec agent consul reload

# Use Consul DNS to see service is now registered
dig @127.0.0.1 -p 8601 counting.service.consul


