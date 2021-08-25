## Dockerfile with another example (more complex)

Learn with an example. We create a node app, with a TODO list management.

1. First of all, get noticed that we have an app folder

2. Open the terminal and go to the inside app

3. Now, we can create a Dockerfile:
```
 # syntax=docker/dockerfile:1
 FROM node:12-alpine
 RUN apk add --no-cache python g++ make
 WORKDIR /app
 COPY . .
 RUN yarn install --production
 CMD ["node", "src/index.js"]
```

4. Go to the console and build it
```sh
 docker build -t manavarro/excersise:2.0 .
```

5. Finally, Run it
```sh
docker run -dp 2000:3000 manavarro/excersise:2.0
```
Remember the -d and -p flags. We’re running the new container in “detached” mode (in the background) and creating a mapping between the host’s port 2000 to the container’s port 3000. Without the port mapping, we wouldn’t be able to access the application.

6. Update the souce code, and then build and run the app into Docker, by replacing the following text:
```sh
<p className="text-center">No items yet! Add one above!</p>
```


You probably will se an error like this (the IDs will be different):

```sh
docker: Error response from daemon: driver failed programming external connectivity on endpoint laughing_burnell 
(bb242b2ca4d67eba76e79474fb36bb5125708ebdabd7f45c8eaf16caaabde9dd): Bind for 0.0.0.0:3000 failed: port is already allocated.
 ```
 
So, what happened? We aren’t able to start the new container because our old container is still running. The reason this is a problem is because that container is using the host’s port 3000 and only one process on the machine (containers included) can listen to a specific port. To fix this, we need to remove the old container.
