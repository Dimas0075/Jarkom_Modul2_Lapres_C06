# Jarkom_Modul2_Lapres_C06

### Soal 1. Membuat sebuah website utama dengan (1) alamat http://semeruyyy.pw <br>
Sebelumnya lakukan konfigurasi seperti pada modul pengenalan uml. <br>
Langkah pertama adalah konfigurasi pada file /etc/bind/named.conf.local sesuai dengan gambar dibawah. 
![soal1conflocal](https://user-images.githubusercontent.com/49650266/98755261-ce028580-23fa-11eb-8e9c-2f8e2ac4fe6c.png) <br>
Lalu buat sebuah direktori dengan nama jarkom. <br>
```diff
nano /etc/bind/jarkom
```
Setelahnya salin db.local ke semeruC06.pw .
```diff
cp /etc/bind/db.local /etc/bind/jarkom/semeruC06.pw
```
Lalu konfigurasi pada file /etc/bind/jarkom/semeruC06.pw .<br>
![soal1semeruC06](https://user-images.githubusercontent.com/49650266/98757570-66026e00-23ff-11eb-9139-bb9d1bf40fb8.png) <br>
Lalu kita konfigurasi beberapa file di uml GRESIK. <br>
Comment (tambahkan # di depan) 2 nameserver di atas. <br>
![soal1resolv](https://user-images.githubusercontent.com/49650266/98757869-f9d43a00-23ff-11eb-96c8-53bfb83a2c8b.png)
Lalu coba ping semeruC06.pw. <br>
![pingsoal1](https://user-images.githubusercontent.com/49650266/98758108-8da60600-2400-11eb-8258-b43f22392401.png)

### Soal 2. Buat Alias www.semeruC06.pw






