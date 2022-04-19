# Configure a multi-node Consul on Docker containers for service discovery and KV storage

Hashicorp Consul is a tool that does many things from service discovery to DNS resolution to providing a distributed KV store. This is especially useful for environments where there are many microservices.

In this tutorial we first go through how to deploy Consul on docker containers with some custom configuration and get the cluster up and running. Then we register a micro-service on it to demonstrate service discovery. Because we are setting up a multi-node Consul cluster and Consul itself is fairly lightweight we will use docker containers to deploy the whole setup and use container volumes to mount the config files.