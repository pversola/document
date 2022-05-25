- Open the docker service file `/lib/systemd/system/docker.service`.  
  Search for `ExecStart` and replace that line with the following:
  ```
  ExecStart=/usr/bin/dockerd -H tcp://0.0.0.0:4243 -H unix:///var/run/docker.sock
  ```
- Reload and restart Docker service
  ```
  $ sudo systemctl daemon-reload
  $ sudo service docker restart
  ```
- Test
  ```
  curl http://localhost:4243/version
  ```
