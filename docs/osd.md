## Create OSD
Menambahkan OSD baru ke node ceph-osd2 dengan perangkat /dev/sdb
```
ceph orch daemon add osd ceph-osd2:/dev/sdb
```

## Remove OSD
Untuk memperlihatkan osd yang ada pada cluster ceph gunakan perintah:
```
ceph osd tree
```
Output:
```
ID  CLASS  WEIGHT   TYPE NAME           STATUS  REWEIGHT  PRI-AFF
-1         0.10413  root default                                 
-3         0.04376      host ceph-osd1                           
 0    hdd  0.01459          osd.0           up   1.00000  1.00000
 1    hdd  0.01459          osd.1           up   1.00000  1.00000
 2    hdd  0.01459          osd.2           up   1.00000  1.00000
-5         0.06036      host ceph-osd2                           
 3    hdd  0.01459          osd.3           up   1.00000  1.00000
 4    hdd  0.01459          osd.4           up   1.00000  1.00000
 5    hdd  0.03119          osd.5         down   1.00000  1.00000
```

output diatas pada osd.5 terdapat masalah, berikut cara menyelesaikan masalah tersebut:

- Menonaktifkan osd.5, Agar OSD yang dieluarkan tidak lagi menerima pemrosesan atau penyimpanan data, gunakan perintah:
    ```
    ceph osd out osd.5
    ```

- Hapus referensi osd.5 dari konfigurasi pohon crush (CRUSH map). CRUSH map digunakan oleh Ceph untuk menentukan penempatan data secara terdistribusi di OSD, perintahnya:
    ```
    ceph osd crush remove osd.5
    ```

- Hapus entri otorisasi osd.5, fungsinya untuk menghapus kunci dan informasi otorisasi terkait dengan OSD tersebut, perintahnya adalah:
    ```
    ceph auth del osd.5 
    ```

- Hapus osd.5:
    ```
    ceph osd rm osd.5
    ```

- Berikan flag noout pada cluster Ceph OSD. Flag ini menunjukkan bahwa tidak ada OSD yang sedang di-"out", artinya semua OSD aktif dan tersedia:
    ```
    ceph osd set noout
    ```
- Memaksa penghapusan daemon OSD bermasalah:
    ```
    ceph orch daemon rm osd.5 --force
    ```
- Berikan flag unset noout apabila sudah mengganti/menyelesaikan masalah osd.5:
    ```
    ceph osd unset noout
    ```