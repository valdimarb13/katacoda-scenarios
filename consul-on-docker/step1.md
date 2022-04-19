#### Get docker image
The consul image is already available as a docker image created by Hashicorp themselves. It is advisable to use this directly as it is the official image that is secure and signed.

Note: this may take some time to download depending on the speed of the internet connection. If the connection is slow, it will keep retrying for some time but should eventually finish.

`docker pull consul`{{execute}}

#### Verify image
Once the image is pulled you should be able to verify its existence on the machine by running the `docker images` command shown below.

`docker images -f 'reference=consul'`{{execute}}

#### Run first Consul server on ports 8501/8601
It is perfectly fine to run only one Consul server for test purposes but we will try to run a multi-node cluster and show how they connect to each other while running in different containers.

We will call each of them different names like `server1`, `server2`, and `server3` and run them on different ports.

`docker run -d -p 8501:8500 -p 8601:8600/udp --name=server1 consul agent -server -ui -node=server-1 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
#### Run second Consul server on ports 8502/8602
`docker run -d -p 8502:8500 -p 8602:8600/udp --name=server2 consul agent -server -ui -node=server-2 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
#### Run third server Consul server on ports 8503/8603
`docker run -d -p 8503:8500 -p 8603:8600/udp --name=server3 consul agent -server -ui -node=server-3 -bootstrap-expect=3 -client=0.0.0.0`{{execute}}
#### Verify that each server is running

Right now the servers are not connected in one cluster. We have to check each one independently. We will connect them in the next steps.

`docker exec server1 consul members`{{execute}}

`docker exec server2 consul members`{{execute}}

`docker exec server3 consul members`{{execute}}