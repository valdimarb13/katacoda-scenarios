#### Get IP of Consul leader
We will need this to tell our agent to join our cluster available at the $LEADER IP address.

``LEADER=`docker exec server1 consul info | grep leader_addr | awk -F= '{print $2}' | awk -F: '{print $1}' | xargs echo` ``{{execute}}

#### Check the IP has been correctly obtained
`echo $LEADER`{{execute}}
#### Start Consul agent on ports 8504/8604 and with a volume mount
We run our Consul agent docker container with a volume mount using the `-v` flag for `docker run`. We basically say to use our current working directory (`pwd`) and to mount it into the docker container at the path `/consul/config`. As a result, all files we create in our current path will also get mounted into the docker container at `/consul/config/`

`docker run -d -v $(pwd):/consul/config -p 8504:8500 -p 8604:8600/udp --name=agent consul agent -ui -node=agent -join=$LEADER`{{execute}}

#### Check agent has joined
Once the agent is running, you can see the agent is also part of the cluster. Note here that the `Type` of the agent is mentioned as `client` and not as `server`. The agent is what we will use for our applications to interact with Consul.

`docker exec server1 consul members`{{execute}}