# Information 
Dirboosters adalah sebuah tool populer untuk menemukan direktori dan file tersembunyi pada server web. Dirboosters ditulis dalam bahasa Python dan menggunakan beberapa thread untuk mempercepat proses scanning. Berikut adalah cara membuat code sederhana dari Dirboosters dalam bahasa Python:

**Pertama, kita perlu mengimpor library yang diperlukan:**
```python3
import argparse
import requests
from concurrent.futures import ThreadPoolExecutor, as_completed
```
Kita akan menggunakan `argparse` untuk mengurai argumen baris perintah, `requests` untuk mengirim permintaan HTTP, dan `ThreadPoolExecutor` untuk menjalankan beberapa thread secara bersamaan.
Selanjutnya, kita perlu mendefinisikan argumen baris perintah. Kita akan menggunakan `argparse` untuk mendefinisikan dua argumen: URL target dan file wordlist. Berikut cara mendefinisikan argumen:
```python3
parser = argparse.ArgumentParser()
parser.add_argument('url', help='Target URL')
parser.add_argument('-w', '--wordlist', help='Wordlist file', default='wordlist.txt')
args = parser.parse_args()
```
Argumen `url` diperlukan dan harus berisi URL target. Argumen `wordlist` adalah opsional dan menentukan file wordlist yang digunakan. Jika tidak disediakan, argumen ini default ke `wordlist.txt`.
Sekarang, kita perlu membaca file wordlist dan membuat daftar direktori untuk diuji. Kita bisa menggunakan fungsi bawaan `open()` untuk membaca file dan metode `splitlines()` untuk membagi file menjadi daftar baris. Berikut cara membaca file wordlist:
```python3
with open(args.wordlist) as f:
    directories = [line.strip() for line in f.readlines()]
```
Metode `strip()` digunakan untuk menghapus karakter baris baru dari akhir setiap baris.
Sekarang, kita dapat mendefinisikan fungsi yang akan menguji setiap direktori. Kita akan memanggil fungsi ini `test_directory()`. Fungsi ini akan mengirimkan permintaan HTTP GET ke URL target dengan direktori ditambahkan ke akhir URL. Jika kode status respons adalah 200, ini berarti direktori ada dan kita akan mencetak URL. Berikut kode untuk fungsi `test_directory()`:
```python3
def test_directory(url, directory):
    full_url = url + '/' + directory
    response = requests.get(full_url)
    if response.status_code == 200:
        print(full_url)
```
Akhirnya, kita dapat membuat pool thread dan menjalankan fungsi `test_directory()` untuk setiap direktori dalam daftar. Kita akan menggunakan `ThreadPoolExecutor` untuk menjalankan fungsi untuk setiap direktori secara paralel. Berikut kode untuk membuat pool thread:
```python3
with ThreadPoolExecutor(max_workers=20) as executor:
    futures = [executor.submit(test_directory, args.url, directory) for directory in directories]

    for future in as_completed(futures):
        pass
```
Argumen `max_workers` menentukan jumlah maksimum thread yang digunakan. Kita akan menggunakan 20 thread dalam contoh ini. Metode `executor.submit()` digunakan untuk menyerahkan tugas baru ke pool thread untuk setiap direktori. Fungsi `as_completed()` digunakan untuk menunggu semua tugas selesai.


