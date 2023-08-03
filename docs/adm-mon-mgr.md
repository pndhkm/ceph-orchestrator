## Admin
Membuat node ceph-admin
```
cephadm bootstrap --mon-ip 192.168.1.4 --initial-dashboard-user admin --initial-dashboard-password password --dashboard-password-noupdate --allow-fqdn-hostname --docker --cluster-network 10.10.10.0/24
```

Mendapatkan pub key
```
ceph cephadm get-pub-key > ~/ceph.pub
```

Menyalin pub key ke node lain
```
ssh-copy-id -f -i ~/ceph.pub root@192.168.1.5
```

Menambahkan host ke dalam cluster
```
ceph orch host add node2 192.168.1.5
```

Menambahkan daemon monitor ke host node2
```
ceph orch daemon add mon node2:192.168.1.5
```

Menambahkan daemon manager ke host node2
```
ceph orch daemon add mgr node2:192.168.1.5
```

## remove or change mgr 
```
ceph mgr fail <mgr.id>
ceph mgr fail mgr.ceph-mon-mgr.qnqpgi
```
