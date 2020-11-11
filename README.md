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
![soal1resolv](https://user-images.githubusercontent.com/49650266/98757869-f9d43a00-23ff-11eb-96c8-53bfb83a2c8b.png) <br>
Lalu coba ping semeruC06.pw. <br>
![pingsoal1](https://user-images.githubusercontent.com/49650266/98758108-8da60600-2400-11eb-8258-b43f22392401.png)

### Soal 2. Buat Alias www.semeruC06.pw
Konfigurasi pada UML MALANG di file /etc/bind/jarkom/semeruC06.pw sesuai gambar di bawah. <br>
![soal2semeruc06](https://user-images.githubusercontent.com/49650266/98758356-2a68a380-2401-11eb-9007-55678723f426.png) <br>
Coba ping pada UML Gresik. <br>
![soal2ping](https://user-images.githubusercontent.com/49650266/98758419-54ba6100-2401-11eb-8a9f-fc99fb38fae1.png) <br>

### Soal 3. Membuat subdomain http://penanjakan.semeruyyy.pw yang diatur DNS-nya pada MALANG dan mengarah ke IP Server PROBOLINGGO
Konfigurasi pada UML MALANG di file /etc/bind/jarkom/semeruC06.pw seperti gambar dibawah. <br>
![soal3semeruc06](https://user-images.githubusercontent.com/49650266/98758605-c1cdf680-2401-11eb-9543-d95cee120ba9.png) <br>
Coba ping www.penanjakan.semeruC06.pw pada UML GRESIK. <br>
![soal3ping](https://user-images.githubusercontent.com/49650266/98758705-00fc4780-2402-11eb-9700-1c08ac9c99fb.png) <br>

### Soal 4. Membuat reverse domain untuk domain utama.
Konfigurasi pada UML MALANG di file /etc/bind/named.conf.local. <br>
![soal4conflocal](https://user-images.githubusercontent.com/49650266/98761348-cd242080-2407-11eb-86ba-8875f011f3a6.png) <br>
Salin file /etc/bind/db.local ke /etc/bind/jarkom/77.151.10.in-addr.arpa dan konfigurasi file tersebut sesuai gambar di bawah. <br>
![soal4arpa](https://user-images.githubusercontent.com/49650266/98761471-12e0e900-2408-11eb-9a82-79bada8e0f6e.png) <br>
Lalu pindah ke UML GRESIK dan konfigurasi file /etc/resolv.conf. Pada file tersebut uncomment nameserver dua atas dan comment yg IP Malang. <br>
Setelah melakukan konfigurasi update dan install dnsutils. <br>
```diff
apt-get update 
apt-get install dnsutils
```
Lalu konfigurasi file /etc/resolv.conf, comment nameserver dua atas, uncomment yg IP Malang. <br>
Setelah itu jalankan host -t PTR 10.151.77.58. <br>
![soal4ptr](https://user-images.githubusercontent.com/49650266/98761797-ccd85500-2408-11eb-8b47-4bd862804ef2.png)

### Soal 5. Membuat DNS Server Slave pada MOJOKERTO.
Konfigurasi pada UML MALANG di file /etc/bind/named.conf.local seperti gambar dibawah. <br>
![soal5conflocal](https://user-images.githubusercontent.com/49650266/98762480-4b81c200-240a-11eb-9e48-1de362d06079.png) <br>
Lalu pindah ke UML MOJOKERTO, update UML dan install aplikasi bind9. Konfigurasi file /etc/bind/named.conf.local seperti gambar dibawah. <br>
![soal5conflocalmojo](https://user-images.githubusercontent.com/49650266/98762632-ad422c00-240a-11eb-8a77-d5d9cd3f25d0.png) <br>
Sebelum melakukan ping pada UML GRESIK, hentikan dulu bind9 di UML MALANG. <br>
![soal5ping](https://user-images.githubusercontent.com/49650266/98762731-e4184200-240a-11eb-9f0f-87e0fe1da766.png) <br>

### Soal 6. Membuat subdomain gunung.semeruC06.pw delegasi pada MOJOKERTO dan mengarah ke IP PROBOLINGGO.
Konfigurasi pada UML MALANG di file /etc/bind/jarkom/semeruC06.pw seperti gambar dibawah. <br>
```diff
@	IN	SOA	semeruC06.pw. root.semeruC06.pw. (
			2020110901		; Serial
			604800		; Refresh
			86400			; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@		IN	NS		semeruC06.pw.
@		IN	A		10.151.77.58	; IP MALANG
www		IN	CNAME	semeruC06.pw.
penanjakan	IN	A		10.151.77.60	; IP PROBO
ns1		IN	A		10.151.77.59	; IP MOJO
gunung	IN	NS		ns1
```
Konfigurasi file /etc/bind/named.conf.options dengan melakukan comment dnssec-validation auto menambahkan allow-query{any;}; <br>
Lalu tambahkan konfigurasi file /etc/bind/named.conf.local seperti dibawah ini. <br>
```diff
zone "semeruC06.pw" {
    type master;
    file "/etc/bind/jarkom/semeruC06.pw";
    allow-transfer { 10.151.77.59; }; // IP MOJOKERTO
};
```
Pindah ke UML MOJOKERTO. <br>
Konfigurasi file /etc/bind/named.conf.options dengan melakukan comment dnssec-validation auto dan menambahkan allow-query{any;}; <br>
Setelah itu menambahkan konfigurasi file /etc/bind/named.conf.local <br>
```diff
zone "semeruC06.pw" {
    type master;
    file "/etc/bind/delegasi/semeruC06.pw";
    allow-transfer { any; }; // IP MOJOKERTO
};
```
Lalu buat folder delegasimkdir. <br>
```diff
mkdir /etc/bind/delegasi
```
Setelah itu salin file db.local ke /delegasi/gunung.semeruC06.pw. <br>
```diff
cp /etc/bind/db.local /etc/bind/delegasi/gunung.semeruC06.pw
```
Konfigurasi file gunung.semeruC06.pw seperti dibawah ini. <br>
```diff
@	IN	SOA	gunung.semeruC06.pw. root.gunung.semeruC06.pw. (
			2020110903		; Serial
			604800		; Refresh
			86400			; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@		IN	NS		gunung.semeruC06.pw.
@		IN	A		10.151.77.60	; IP PROBO
```
Lalu melakukan ping di UML GRESIK <br>
![soal6ping](https://user-images.githubusercontent.com/49650266/98763686-23e02900-240d-11eb-8910-d5c3376e15a2.png)

### Soal 7. Membuat subdomain naik.gunung.semeruC06.pw diarahkan ke Probo.
Konfigurasi pada UML MOJOKERTO di file /etc/bind/delegasi/gunung.semeruC06.pw .<br>
```diff
@	IN	SOA	gunung.semeruC06.pw. root.gunung.semeruC06.pw. (
			2020110903		; Serial
			604800		; Refresh
			86400			; Retry
			2419200		; Expire
			604800	)	; Negative Cache TTL
;
@		IN	NS		gunung.semeruC06.pw.
@		IN	A		10.151.77.60	; IP PROBO
naik		IN	A		10.151.77.60	; IP PROBO
```
Lalu coba ping dari UML GRESIK. <br>
![soal7ping](https://user-images.githubusercontent.com/49650266/98764476-70783400-240e-11eb-9c3f-990808b9eea1.png) <br>

### Soal 8. Membuat domain semeruC06.pw DocumentRoot /var/www/semeruC06.pw dapat diakses pada semeruC06.pw/index.php/home.
Konfigurasi pada UML PROBOLINGGO :
* Membuat file semeruC06.pw.conf di sites-available, salin dari default
* Tambahkan ServerName semeruC06.pw pada file tersebut
* Ubah DocumentRoot seperti yg diminta
* Download file semru.xip, unzip, copy ke /var/ww/semeruC06.pw <br>
Berikut ini konfigurasinya : <br>
![soal8conf](https://user-images.githubusercontent.com/49650266/98770482-ef706b00-2414-11eb-9ce0-df48440e3e4b.png) <br>
Dan ini merupakan hasil yang diminta : <br>
![soal8hasil](https://user-images.githubusercontent.com/49650266/98770485-f0a19800-2414-11eb-8101-efab536959fc.png) <br>

### Soal 9. Mengaktifkan mod rewrite semeruC06.pw/index.php/home -> semeruC06.pw/home

Langkah menyelesaikan soal 9 :
* Nyalakan mod rewrite dengan a2enmod mod_rewrite
* Konfigurasi semeruC06 agar bisa pake mod_rewrite kyk di modul
* Buat .htaccess di /var/www/semeruC06.pw dengan rule berikut : <br>
![soal9conf](https://user-images.githubusercontent.com/49650266/98770859-bf759780-2415-11eb-8849-4b27874807c2.png) <br>
Dibawah ini merupakan hasilnya : <br>
![soal9hasil](https://user-images.githubusercontent.com/49650266/98770849-b97fb680-2415-11eb-9457-f6180e88d12c.png) <br>

### Soal 10. Membuat penanjakan.semeruC06.pw untuk nyimpen file asset, root pada /var/www/penanjakan.semeruC06.pw

Langkah menyelesaikan soal 10 :
* Membuat config apache penanjakan.semeruC06.pw.conf dengan cp
* Mengganti root, servername, dan alias <br>
![soal10conf](https://user-images.githubusercontent.com/49650266/98771296-afaa8300-2416-11eb-8489-51d2acfceb4a.png) <br>
* Download dan unzip penanjakan.semeru.zip ke /var/ww/penanjakan/semeruC06.pw <br>
![soal10source](https://user-images.githubusercontent.com/49650266/98771370-e1234e80-2416-11eb-9fc4-3d6ea5c38e01.png) <br>
* Jalankan a2ensite dan restart apache <br>

### Soal 11. Pada folder /public dibolehkan directory listing namun untuk folder yang berada di dalamnya tidak dibolehkan.
* Tambah Options +Indexes sm -Indexes seperti di bawah. <br>
![soal11_1](https://user-images.githubusercontent.com/49650266/98771606-6d357600-2417-11eb-94c7-b6201c24364c.png) <br>
* Restart apache. <br>
* Coba buka. <br>
![public](https://user-images.githubusercontent.com/49650266/98771779-c8676880-2417-11eb-8581-d07e710d62a4.png) <br>
javascripts <br>
![public1](https://user-images.githubusercontent.com/49650266/98771782-c9989580-2417-11eb-9a9d-c563a5f4a43f.png) <br>
images <br>
![public2](https://user-images.githubusercontent.com/49650266/98771783-ca312c00-2417-11eb-825e-591873498e5e.png) <br>
css <br>
![public3](https://user-images.githubusercontent.com/49650266/98771786-cac9c280-2417-11eb-989b-d1cfab79a13c.png) <br>
public <br>

### Soal 12. Mengatasi http error code 404, pake 404.html pada /error buat ganti defaultnya apache.

Berikut ini merupakan langkah penyelesaian :
* Setting di .htaccess seperti dibawah <br>
![soal12awal](https://user-images.githubusercontent.com/49650266/98772090-84289800-2418-11eb-88b6-519de14d4053.png) <br>
* Untuk mengetes, buka alamat asal, misal penanjakan.semeruc06.pw/jek-ganteng <br>
![soal12hasil](https://user-images.githubusercontent.com/49650266/98772092-8559c500-2418-11eb-8f42-bdade7374385.png) <br>

### Soal 13. Alias penanjakan.semeruc06.pw/public/javascripts jd penanjakan.semeruc06.pw/js
Berikut ini merupakan langkah penyelesaian :
* Tambahkan alias seperti ini di config penanjakan <br>
![soal13awal](https://user-images.githubusercontent.com/49650266/98772493-5c85ff80-2419-11eb-82f1-88bbcb512bd5.png) <br>
* Tes, buka penanjakan.semeruc06.pw/js <br>
![soal13akhir](https://user-images.githubusercontent.com/49650266/98772486-5b54d280-2419-11eb-93de-be03164f6129.png)

### Soal 14. Membuat web naik.gunung.semeruc06.pw bisa diakses lewat port 8888
Berikut ini merupakan langkah penyelesaian : 
* Salin config seperti tadi, tapi portnya ganti 8888 <br>
![soal14_1](https://user-images.githubusercontent.com/49650266/98772732-ff3e7e00-2419-11eb-8770-97bdf5a4bd11.png) <br>
* Setting port agar listen ke 8888 <br>
![soal14_2](https://user-images.githubusercontent.com/49650266/98772733-006fab00-241a-11eb-9683-314296c30926.png) <br>
* Download, unzip ke /var/www <br>
* A2ensite, restart apache, buka <br>
![soal14_3](https://user-images.githubusercontent.com/49650266/98772734-01084180-241a-11eb-998e-48a231ab3e21.png)

### Soal 15. Membuat autentikasi naik gunung user “semeru” pass “kuynaikgunung”
Berikut ini merupakan langkah penyelesaian : 
* Membuat user sama pass seperti ini. Kami disini memakai pass “kuy” karena malas hehe <br>
![soal15_1](https://user-images.githubusercontent.com/49650266/98773133-d7034f00-241a-11eb-93ef-7ae06cc1bdb3.png) <br>
* Edit config seperti ini <br>
![soal15_2](https://user-images.githubusercontent.com/49650266/98773119-d4085e80-241a-11eb-9a4c-0db130a764cc.png) <br>
* Hasilnya akan seperti ini <br>
![soal15_3](https://user-images.githubusercontent.com/49650266/98773130-d66ab880-241a-11eb-852c-80e75b9cb307.png) <br>

### Soal 16. Arahkan document root ip default ke semeruc06.pw
![soal16_1](https://user-images.githubusercontent.com/49650266/98773431-7de7eb00-241b-11eb-807a-ae57d9a2b4e6.png) <br>
Hasilnya akan seperti ini : <br>
![soal16_2](https://user-images.githubusercontent.com/49650266/98773424-7c1e2780-241b-11eb-9156-afc8afdd29a7.png) <br>

### Soal 17. Membuat request substring semeru redirect ke semeru.jpg
Edit .htaccess lalu tambhkan seperti ini : <br>
![soal17_1](https://user-images.githubusercontent.com/49650266/98773631-e59e3600-241b-11eb-849e-b408855922c1.png) <br>
Penjelasan: 
* RewriteCond %{REQUEST_URI} !/public/images/semeru.jpg memastikan bahwa url request sekarang bukan semeru.jpg, untuk menghindari infinite redirect
* RewriteRule ^(.*)semeru(.*)$ -> mengecek substring semeru pada url
* /public/images/semeru.jpg url redirect nya <br> <br>
![soal17_2](https://user-images.githubusercontent.com/49650266/98773623-e33bdc00-241b-11eb-9b13-b5b7f964fc8a.png) <br> <br>
![soal17_3](https://user-images.githubusercontent.com/49650266/98773628-e46d0900-241b-11eb-8a29-790ffa306082.png)




















































