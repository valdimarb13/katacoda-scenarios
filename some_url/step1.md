Configure a multi-node Consul on Docker containers for service discovery and KV storage

# Get docker image
`docker pull consul`{{execute}}

# Verify image
`docker images -f 'reference=consul'`{{execute}}

# Run first Consul server on ports 8501/8601
`docker run -d -p 8501:8500 -p 8601:8600/udp --name=server1 consul agent -server -ui -node=server-1 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
# Run second Consul server on ports 8502/8602
`docker run -d -p 8502:8500 -p 8602:8600/udp --name=server2 consul agent -server -ui -node=server-2 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
# Run third server Consul server on ports 8503/8603
`docker run -d -p 8503:8500 -p 8603:8600/udp --name=server3 consul agent -server -ui -node=server-3 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
# Verify the cluster is running
`docker exec server1 consul members# Verify the cluster is running`{{execute}}