#### Pull a small sample app
For our purposes we use a small counting application that counts the number of times it is invoked. We will use this to demo how to register an application with Consul.

`docker pull hashicorp/counting-service:0.0.2`{{execute}}

#### Run the app
`docker run -p 9001:9001 -d --name=app hashicorp/counting-service:0.0.2`{{execute}}

#### Create counting.json for app to register with
When registering a service with Consul we have to declare its configuration in a service file. We will call this file counting.json as we will name our service `counting`. We create this file locally but because we have mounted our local path to the consul agent docker container as a volume, this file will automatically show up inside our container.

`touch counting.json`{{execute}}
#### Add the configuration for the service

This is the basic service definition for registration in Consul

```
{
    service: {
        "name": "counting",
        "tags": ["go"],
        "port": 9001
    }
}
```

We can add this configuration to the `counting.json` file we just created.

`echo '{"service": {"name": "counting", "tags": ["go"], "port": 9001}}' > counting.json`{{execute}}