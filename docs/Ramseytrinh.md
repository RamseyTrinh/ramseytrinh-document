An example for codeblock for docker console
``` yml 
# Pull the latest version of the image
docker pull my-python-app:latest

# Run the container in detached mode
docker run -d -p 4000:80 my-python-app

# Check the logs of the running container
docker logs <container_id>

# Stop the running container
docker stop <container_id>

# Remove the container
docker rm <container_id>
```

``` mermaid
graph LR
  A[Start] --> B{Error?};
  B -->|Yes| C[Hmm...];
  C --> D[Debug];
  D --> B;
  B ---->|No| E[Yay!];
```