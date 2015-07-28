---
layout: post
title: Ceph na szybko
---
Czasami trzeba bardzo szybko uruchomić jakąkolwiek działającą wersje cepha, taki skrypt robiący jedną działającą instancję mon i osd na jednym hoście może sie wtedy przydać.
~~~
#dysk na dane
export DATA_DEV=sdb
#dysk na journal
export JRNL_DEV=sdc
#hostname
export HOST=ceph1
#instalujemy repo
wget -q -O- 'https://ceph.com/git/?p=ceph.git;a=blob_plain;f=keys/release.asc' | sudo apt-key add -
echo deb http://ceph.com/debian-giant/ $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/ceph.list
#instalujemy cepha i ceph-deploy
apt-get update && apt-get install ceph ceph-deploy
#jedziemy z koksem
ceph-deploy install $HOST
ceph-deploy new $HOST
echo "osd crush chooseleaf type = 0" >> ceph.conf
echo "osd pool default size = 1" >> ceph.conf
ceph-deploy mon create-initial $HOST
ceph-deploy disk zap $HOST:$DATA_DEV
ceph-deploy disk zap $HOST:$JRNL_DEV
ceph-deploy osd create $HOST:$DATA_DEV:$JRNL_DEV
sudo chmod +r /etc/ceph/ceph.client.admin.keyring
~~~
