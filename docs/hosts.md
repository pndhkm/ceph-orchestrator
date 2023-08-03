## Add host
Menyalin pub key ke node lain
```
ssh-copy-id -f -i ~/ceph.pub root@192.168.1.6
```

Menambahkan host baru ke dalam cluster
```
ceph orch host add new-host 192.168.1.6
```

Menambahkan daemon monitor ke host baru
```
ceph orch daemon add mon new-host:192.168.1.6
```

add label
```
ceph orch host label add newhosts mon
```

## Remove host
Membuang semua layanan dari host sebelum menghapus
```
ceph orch host drain new-host
```

Menghapus host dari cluster (setelah memastikan tidak ada pekerjaan yang berjalan)
```
ceph orch host rm new-host
```

Menghapus host dari cluster secara paksa dan offline
```
ceph orch host rm new-host --offline --force
```
