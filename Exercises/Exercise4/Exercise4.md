## Use bind mounts

In the previous exercise, we talked about and used a named volume to persist the data in our database. Named volumes are great if we simply want to store data, as we don’t have to worry about where the data is stored.

With bind mounts, we control the exact mountpoint on the host. We can use this to persist data, but it’s often used to provide additional data into containers. When working on an application, we can use a bind mount to mount our source code into the container to let it see code changes, respond, and let us see the changes right away.

For Node-based applications, nodemon (look at package.json) is a tool to watch for file changes and then restart the application. There are equivalent tools in most other languages and frameworks.

|                                             | Named Volumes              | Bind Mounts                    |
|---------------------------------------------|----------------------------|--------------------------------|
|Host Location                                | Docker chooses             | Your control                   |
|Mount Example (using -v)                     | my-volume:/usr/local/data  | /path/to/data:/usr/local/data  |
|Populates new volume with container contents | Yes                        | No                             |
|Supports Volume Drivers                      | Yes                        | No                             |

## Start a dev-mode container
To run our container to support a development workflow, we will do the following:

- Mount our source code into the container
- Install all dependencies, including the “dev” dependencies
- Start nodemon to watch for filesystem changes

1. Run the following command. We’ll explain what’s going on afterwards:

Bash version:
```sh
 docker run -dp 3000:3000 \
     -w /app -v "$(pwd):/app" \
     node:12-alpine \
     sh -c "yarn install && yarn run dev"
If you are using PowerShell then use this command:
```
PowerShell version:
```sh
 docker run -dp 3000:3000 `
     -w /app -v "$(pwd):/app" `
     node:12-alpine `
     sh -c "yarn install && yarn run dev"
```

2. Look and find in the code, for the text of the button "Add Item" and change it (i.e. only "Add")

3. Go to the browser and refresh.