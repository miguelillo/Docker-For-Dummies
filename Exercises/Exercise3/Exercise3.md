## The container's file system

When a container runs, it uses the various layers from an image for its filesystem. Each container also gets its own “scratch space” to create/update/remove files. Any changes won’t be seen in another container, even if they are using the same image.

## See this in practice

To see this in action, we’re going to start two containers and create a file in each. What you’ll see is that the files created in one container aren’t available in another.

1. Start an ubuntu container that will create a file named /data.txt with a random number between 1 and 10000.

```sh
 docker run -d ubuntu bash -c "shuf -i 1-10000 -n 1 -o /data.txt && tail -f /dev/null"
```
In case you’re curious about the command, we’re starting a bash shell and invoking two commands (why we have the &&). The first portion picks a single random number and writes it to /data.txt. The second command is simply watching a file to keep the container running.

2. Open the terminal and go to the inside app

```sh
 docker exec <container-id> cat /data.txt
```

3. Now, let’s start another ubuntu container (the same image) and we’ll see we don’t have the same file.

```sh
 docker run -it ubuntu ls /
```
And look! There’s no data.txt file there! That’s because it was written to the scratch space for only the first container.
With the previous exercise, we saw that each container starts from the image definition each time it starts. 

Containers can create, update, and delete files, those changes are lost when the container is removed and all changes are isolated to that container. With volumes, we can change all of this.

Volumes provide the ability to connect specific filesystem paths of the container back to the host machine. If a directory in the container is mounted, changes in that directory are also seen on the host machine. If we mount that same directory across container restarts, we’d see the same files.

## Volumes - Named Volumes

1. Go to the second Exercise directory (todo app)

By default, the todo app stores its data in a SQLite Database at /etc/todos/todo.db

With the database being a single file, if we can persist that file on the host and make it available to the next container, it should be able to pick up where the last one left off. By creating a volume and attaching (often called “mounting”) it to the directory the data is stored in, we can persist the data. As our container writes to the todo.db file, it will be persisted to the host in the volume.

2. Create a volume by using the docker volume create command.

```sh
 docker volume create todo-db
```
3. Start the todo app container in detached mode and mapping 3000 port (or whatever), but add the -v flag to specify a volume mount. We will use the named volume and mount it to /etc/todos, which will capture all files created at the path.

4. Once the container starts up, open the app and add a few items to your todo list.

5. Stop and remove the container for the todo app. Use the Dashboard or docker ps to get the ID and then docker rm -f  with the id to remove it.

6. Start a new container using the same command from above.

7. Open the app. You should see your items still in your list :)

## Volumes - Dive Into Volume

Yo could ask “Where is Docker actually storing my data when I use a named volume?” If you want to know, you can use the docker volume inspect command:

```sh
 docker volume inspect todo-db
```

