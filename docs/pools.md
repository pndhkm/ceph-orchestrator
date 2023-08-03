## Remove pool 
Bagian ini berfokus pada penghapusan pool (kelompok penyimpanan) dari cluster Ceph. Ini melibatkan beberapa langkah, seperti mengizinkan kemampuan penghapusan pool, menghapus pool, dan kemudian menonaktifkan kembali kemampuan tersebut.

Mengizinkan penghapusan pool
```
ceph tell mon.\* injectargs '--mon-allow-pool-delete=true'
```
Menghapus pool (ganti <pool-name> dengan nama pool yang ingin dihapus)
```
ceph osd pool delete <pool-name> <pool-name> --yes-i-really-really-mean-it
```
Menonaktifkan kembali kemampuan penghapusan pool
```
ceph tell mon.\* injectargs '--mon-allow-pool-delete=false'
```

## Create Pool & RBD 
Proses pembuatan pool (kelompok penyimpanan) dan RBD (Ceph Block Device) dalam cluster Ceph. Ini melibatkan tiga langkah utama: membuat pool baru, mengatur properti pool, dan menginisialisasi pool untuk RBD.

Membuat pool baru dengan ukuran 32 dan 32 placement groups
```
ceph osd pool create <name pool> 32 32
```
Mengatur ukuran replikasi pool menjadi 2
```
ceph osd pool set <name pool> size 2
```
Menginisialisasi pool untuk RBD
```
rbd pool init <name pool>
```

## Create health metrics
Membuat metrik kesehatan (health metrics) untuk perangkat (device) dalam cluster Ceph. Metrik kesehatan ini dapat memberikan informasi penting tentang kondisi perangkat dalam cluster.

Membuat pool khusus untuk menyimpan metrik kesehatan perangkat
```
ceph osd pool create device_health_metrics
```
Mengumpulkan metrik kesehatan dari perangkat
```
ceph device scrape-health-metrics
```
Mengatur jumlah minimum placement groups (PGs) pada pool metrik kesehatan menjadi 1
```
ceph osd pool set device_health_metrics pg_num_min 1
```

## Clone Pool 
Mengklon pool dalam cluster Ceph. Kloning pool melibatkan beberapa tahap, termasuk pembuatan pool baru dengan jumlah placement groups (PGs) yang benar, menyalin konten dari pool lama ke pool baru, menghapus pool lama, dan mengubah nama pool baru.

Membuat pool baru dengan jumlah PGs yang benar
```
ceph osd pool create $new_pool 32
```

Menyalin konten dari pool lama ke pool baru
```
rados cppool $old_pool $new_pool
```

Menghapus pool lama (ganti $old_pool dengan nama pool lama)
```
ceph osd pool delete $old_pool $old_pool --yes-i-really-really-mean-it
```
Mengubah nama pool baru menjadi 'default.rgw.buckets.data'
```
ceph osd pool rename $new_pool $old_pool
```
