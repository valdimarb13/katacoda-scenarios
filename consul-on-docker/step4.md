#### Use Consul KV storage
Consul comes with a built-in KV storage that we can now utilize. We do this simply by using the `consul kv get` and `consul kv put` commands to get and put data in, respectively. 

Let's add the first version of our app as a key-value data in our Consul KV storage. This is the version we will deploy and register on Consul.

`docker exec agent consul kv put app/counting/version 1`{{execute}}

Similarly, we can `query` this data as well to get the value we entered back to us as a result.

`docker exec agent consul kv get app/counting/version`{{execute}}