# Auto reconnect
Script untuk auto reconnect modem rakitan yang telah dicompile dengan bahasa go agar cross compiled dan bisa dipakai disemua OS dengan arsitektur yang sama.

## Persyaratan
- Nama interface: modem
- Interface device: wwan0

## Alur kerja 
- Script akan melakukan ping ke google selama 1 menit dengan jeda 5 detik menggunakan interface device `wwan0`
- Apabila script tidak mendapatkan koneksi ke google, maka script tersebut akan melakukan restart interface device
- Setelah interface direstart, script akan melakukan jeda pengecekan 5 menit untuk menghindari looping atau pengulangan jika koneksi sedang tidak stabil

## Pemasangan
### Direct Koneksi (tanpa clash)
```
wget -o /usr/sbin/reconnect https://github.com/Manssizz/auto-reconnect/raw/refs/heads/main/reconnect
chmod +x /usr/sbin/reconnect
sed -i '/exit 0/i /usr/sbin/reconnect &' /etc/rc.local
```

### Dengan clash
```
wget -o /usr/sbin/reconnect https://github.com/Manssizz/auto-reconnect/raw/refs/heads/main/reconnect
chmod +x /usr/sbin/reconnect
sed -i '/exit 0/i sleep 30' /etc/openclash/custom/openclash_custom_overwrite.sh
sed -i '/exit 0/i /usr/sbin/reconnect &' /etc/openclash/custom/openclash_custom_overwrite.sh
```
_Note: Restart clash setelah pemasangan script._

## Menghapus script.
Jika script tidak berfungsi sesuai yang diharapkan, bisa dihapus dengan mengikuti command berikut pada terminal.
### Direct Koneksi (tanpa clash)
```
sed -i '/sleep 30/d' /etc/openclash/custom/openclash_custom_overwrite.sh
sed -i '/\/usr\/sbin\/reconnect &/d' /etc/rc.local
```

### Dengan clash
```
sed -i '/sleep 30/d' /etc/openclash/custom/openclash_custom_overwrite.sh
sed -i '/\/usr\/sbin\/reconnect &/d' /etc/openclash/custom/openclash_custom_overwrite.sh
```
