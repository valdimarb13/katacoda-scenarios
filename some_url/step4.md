
# Register the app by adding the service in /consul/config directory
`docker exec agent /bin/sh -c "echo '{\"service\": {\"name\": \"counting\", \"tags\": [\"go\"], \"port\": 9001}}' >> /consul/config/counting.json"`{{execute}}

# Reload config
`docker exec agent consul reload`{{execute}}

# Use Consul DNS to see service is now registered
`dig @127.0.0.1 -p 8601 counting.service.consul`{{execute}}