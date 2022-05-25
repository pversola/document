# How to copy files to/from a container

### To copy a file from the local file system to a container

```
docker cp <src-path> <container>:<dest-path>
kubectl cp <src-path> <your-pod-name>:<dest-path>
```

### To copy a file from the container to the local file system

```
docker cp <container>:<src-path> <local-dest-path>
kubectl cp <your-pod-name>:<src-path> <local-dest-path>
```
