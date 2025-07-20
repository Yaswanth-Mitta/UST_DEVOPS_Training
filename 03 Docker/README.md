
This document lists Docker commands extracted from the shell history, along with comments describing what each command does.


```bash
docker images                  # List all available Docker images
docker image ls                # Alternative syntax to list Docker images
docker search ubuntu           # Search Docker Hub for Ubuntu images
docker rmi hello-world         # Remove the 'hello-world' image
docker rmi 74cc54e27dc4        # Remove the image with the specified image ID
docker history                 # Show history of the last-used image (missing image name)
docker -v                      # Display the installed Docker version

