demo-project
============

node.js express project for GCE start script demo use.


! Start Script (demo-git-pull.sh)

```
#!/bin/bash
cd /opt
wget http://nodejs.org/dist/v0.10.30/node-v0.10.30-linux-x64.tar.gz
tar -xzf node-v0.10.30-linux-x64.tar.gz
ln -s node-v0.10.30-linux-x64 node

export PATH=/opt/node/bin:$PATH

#yum update -y
yum install git -y
git clone https://github.com/peihsinsu/demo-project.git web
cd /opt/web && /opt/node/bin/npm install

/opt/node/bin/npm install forever -g
/opt/node/bin/forever start /opt/web/app.js
```

! Start a GCE instance using the script (start.sh)

```
#!/bin/bash

## Usage: bash ./start.sh test2

export PROJECT=your-gcp-project
export IMG='projects/centos-cloud/global/images/centos-6-v20140718'
export SCRIPT=$2

if [[ -z "$1" ]]; then
  echo "Please insert machine name..."
elif [[ -z "$2" ]]; then
  echo "Please insert startup script..."
else
  gcutil --service_version="v1" --project="$PROJECT" addinstance "$1" \
    --zone="asia-east1-a" --machine_type="g1-small" --network="default" \
    --external_ip_address="ephemeral" \
    --service_account_scopes="https://www.googleapis.com/auth/devstorage.read_only" \
    --can_ip_forward="true" \
    --image="https://www.googleapis.com/compute/v1/$IMG" \
    --persistent_boot_disk="true" \
    --auto_delete_boot_disk="true" \
    --tags="demo" \
    --metadata_from_file=startup-script:$SCRIPT
fi
```

! Execute

```
$ ./start.sh instance-1 demo-git-pull.sh
```

