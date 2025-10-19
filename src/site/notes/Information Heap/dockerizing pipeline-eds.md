---
{"dg-publish":true,"permalink":"/information-heap/dockerizing-pipeline-eds/","noteIcon":"","created":"2025-09-19T05:51:00.149-05:00"}
---

Date: [[Information Heap/2025 09-September 19\|2025 09-September 19]]

To see a Plotly plot running on a container's localhost, you need to **map the container's port to a port on your host machine**. To investigate the container's file tree, you can either **use `docker exec`** to get a shell inside it or **build a temporary container** to inspect it.


## **1. Viewing a Plotly Plot from a Docker Container**

The Plotly web server runs on a port inside the container, but it's not accessible from your host machine unless you explicitly map it. Your container's localhost is a separate network from your computer's localhost.

Use the `-p` (publish) flag with `docker run` to map the container's port to your host's port. Assuming Plotly runs on port 8000 inside the container, you would use:

Bash

```
docker run -p 8000:8000 pipeline-eds trend M100FI --start Sept2 --end Sept16
```

- **`-p 8000:8000`**: The first `8000` is the port on your host machine, and the second `8000` is the port inside the container. This command tells Docker to forward traffic from your computer's port 8000 to the container's port 8000.
    

After running the command, you can open your web browser and navigate to `http://localhost:8000` to view the plot.


<hr>

## **2. Investigating the Container's File Tree**

You can't directly `cat` files on a container's filesystem from your host machine. You need to either execute a command inside a running container or copy files to your host.

### **Method A: Use `docker exec` (Best for a running container)**

If you have a container running, you can use `docker exec` to execute a command inside it, such as `cat`.

1. **Start the container**:
    
    Bash
    
    ```
    docker run -itd --name my-eds pipeline-eds sleep 3600
    ```
    
    This command starts a container named `my-eds` in the background and keeps it running for an hour.
    
2. **Execute the `cat` command inside the container**:
    
    Bash
    
    ```
    docker exec my-eds cat /app/src/pipeline/data/sensors.db
    ```
    
    This command executes `cat` inside the `my-eds` container and prints the contents of the database file to your terminal.
    

### **Method B: Use `docker cp`**

You can copy a file from the container to your host machine, and then open it locally.

Bash

```
docker cp my-eds:/app/src/pipeline/data/sensors.db ./sensors.db
```

This command copies the file `sensors.db` from the container's `/app/src/pipeline/data/` directory to your current local directory. You can then open and inspect it with your preferred tools.

### **Method C: Use `docker run` on a one-off container**

For simple file inspection, you can start a new container that simply executes `ls -R` to list the files and then exits.

Bash

```
docker run --rm pipeline-eds ls -R /app/src/pipeline/data
```

This command starts a new container, lists the contents of the specified directory, and then removes the container. This is a quick and clean way to investigate the file system.

