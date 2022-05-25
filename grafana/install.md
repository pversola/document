```
$ curl -fsSL https://packages.grafana.com/gpg.key | sudo apt-key add -

$ cat <<EOF | sudo tee /etc/apt/sources.list.d/grafana.list
deb https://packages.grafana.com/oss/deb stable main
EOF

$ sudo apt install grafana
$ sudo systemctl enable grafana-server.service --now

$ sudo apt install prometheus prometheus-node-exporter
```
