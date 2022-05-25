# How to build image command

```
docker login <END POINT>:<PORT>
docker build -t <IMAGE NAME> .
docker tag <OLD IMAGE NAME>:<TAG> <END POINT>:<PORT>/<NEW IMAGE NAME>:<TAG>
docker image push <END POINT>:<PORT>/<NEW IMAGE NAME>
```
