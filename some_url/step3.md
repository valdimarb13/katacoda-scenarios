# Pull a small sample app
`docker pull hashicorp/counting-service:0.0.2`{{execute}}

# Run the app
`docker run -p 9001:9001 -d --name=app hashicorp/counting-service:0.0.2`{{execute}}

# Create counting.json for app to register with
This file automatically show up inside the container since we have mounted the volume
`touch counting.json`{{execute}}