# Install and configure the Kubernetes API Server

## node0

```
gcloud compute ssh node0
```

### Download etcd release

```
wget https://github.com/coreos/etcd/releases/download/v2.3.2/etcd-v2.3.2-linux-amd64.tar.gz
tar -xvf etcd-v2.3.2-linux-amd64.tar.gz
sudo cp etcd-v2.3.2-linux-amd64/etcdctl /usr/local/bin/
sudo cp etcd-v2.3.2-linux-amd64/etcd /usr/local/bin/
```

### Create the etcd systemd unit file:

```
[Unit]
Description=etcd
Documentation=https://github.com/coreos

[Service]
ExecStart=/usr/local/bin/etcd \
  --data-dir=/var/lib/etcd
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Review the unit file:

```
cat etcd.service
```

Start the etcd service:

```
sudo mv etcd.service /etc/systemd/system/
```

```
sudo systemctl daemon-reload
sudo systemctl enable etcd
sudo systemctl start etcd
```

### Verify

```
sudo systemctl status etcd
```

```
etcdctl cluster-health
```