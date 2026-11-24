# Cek URL-phising-detection

Anggota: Jovanic Manuelo, Elvin Federic Aurelio, Agnes Aurelia

Aplikasi Flask yang menilai apakah sebuah URL mirip tautan phishing, memakai model
machine learning yang dilatih dari "Phishing URL dataset" (Mendeley Data).

## Isi folder

| File / folder | Isi |
|---|---|
| `UAS_KeamananData_Kelompok4_Phishing_URL.ipynb` | Notebook: EDA, preprocessing, fitur, split, training, evaluasi, simpan model |
| `features.py` | Normalisasi URL + 36 fitur leksikal. Dipakai notebook DAN aplikasi |
| `app.py` | Aplikasi Flask |
| `templates/`, `static/` | Tampilan aplikasi (HTML, CSS, grafik dari notebook) |
| `model/phishing_url_pipeline.joblib` | Pipeline terlatih (ekstraksi fitur + Random Forest) |
| `model/metadata.json` | Threshold, metrik, dan info data untuk halaman Info Model |
| `uji_fungsi.py`, `hasil_uji_fungsi.csv` | Uji fungsi aplikasi dan hasilnya |
| `data/Phishing URL dataset/` | Dataset asli (2 file CSV) |
| `screenshots/` | Tangkapan layar aplikasi |

## Penjelasan 36 fitur URL

Fitur berikut dihitung dari teks URL oleh `features.py`. Semua nilai diekstrak
tanpa membuka atau mengirim permintaan ke URL tersebut.

| Fitur | Penjelasan |
|---|---|
| `url_length` | Jumlah karakter pada URL setelah normalisasi. |
| `host_length` | Jumlah karakter pada bagian nama host/domain. |
| `path_length` | Jumlah karakter pada bagian path setelah domain. |
| `query_length` | Jumlah karakter pada bagian query atau parameter URL. |
| `n_dots_host` | Banyaknya titik pada nama host. |
| `n_subdomains` | Banyaknya tingkat subdomain pada host. |
| `n_hyphens_host` | Banyaknya tanda hubung pada nama host. |
| `n_digits_host` | Banyaknya angka pada nama host. |
| `digit_ratio_host` | Perbandingan jumlah angka terhadap panjang nama host. |
| `longest_host_token` | Panjang token alfanumerik terpanjang pada nama host. |
| `host_entropy` | Keacakan atau keragaman karakter pada nama host. |
| `path_depth` | Banyaknya segmen tidak kosong pada path URL. |
| `n_digits_path` | Banyaknya angka pada bagian path. |
| `n_hyphens_path` | Banyaknya tanda hubung pada bagian path. |
| `n_underscores` | Banyaknya karakter garis bawah pada seluruh URL ternormalisasi. |
| `n_percent` | Banyaknya tanda `%` yang menandai encoding karakter pada URL. |
| `n_equals` | Banyaknya tanda `=` pada seluruh URL ternormalisasi. |
| `n_ampersand` | Banyaknya tanda `&`, yang biasa memisahkan parameter query. |
| `n_question` | Banyaknya tanda `?` pada seluruh URL ternormalisasi. |
| `n_at` | Banyaknya tanda `@` pada seluruh URL ternormalisasi. |
| `n_tilde` | Banyaknya tanda `~` pada seluruh URL ternormalisasi. |
| `n_special_total` | Jumlah gabungan karakter khusus seperti `_`, `%`, `=`, `&`, `?`, `@`, `~`, `!`, `$`, `*`, `,`, `;`, dan `+`. |
| `path_entropy` | Keacakan atau keragaman karakter pada bagian path. |
| `longest_path_token` | Panjang token alfanumerik terpanjang pada bagian path. |
| `has_ip` | Bernilai 1 jika host merupakan alamat IP IPv4/IPv6, dan 0 jika berupa nama domain. |
| `has_port` | Bernilai 1 jika URL menyertakan nomor port khusus, dan 0 jika tidak. |
| `has_double_slash` | Bernilai 1 jika path mengandung `//`, dan 0 jika tidak. |
| `has_php` | Bernilai 1 jika path mengarah ke file `.php`, dan 0 jika tidak. |
| `has_html` | Bernilai 1 jika path mengarah ke file HTML, seperti `.html`, `.htm`, atau `.xhtml`, dan 0 jika tidak. |
| `has_exe` | Bernilai 1 jika path mengarah ke format dapat diunduh/dijalankan seperti `.exe`, `.apk`, `.zip`, atau `.rar`, dan 0 jika tidak. |
| `is_shortener` | Bernilai 1 jika domain merupakan layanan pemendek URL yang dikenali, dan 0 jika bukan. |
| `is_common_tld` | Bernilai 1 jika akhiran domain termasuk daftar TLD yang umum pada data, dan 0 jika tidak. |
| `tld_length` | Jumlah karakter pada akhiran domain (TLD), misalnya `com` memiliki panjang 3. |
| `n_suspicious_words` | Banyaknya kemunculan kata mencurigakan seperti `login`, `verify`, `account`, `password`, atau `secure`. |
| `brand_outside_domain` | Bernilai 1 jika nama merek muncul pada host, path, atau query tetapi domain terdaftarnya bukan domain resmi merek tersebut. |
| `n_query_params` | Banyaknya parameter pada bagian query URL. |

## Menjalankan aplikasi

```bash
pip install -r requirements.txt
python app.py
```

Buka http://127.0.0.1:5000

Halaman: **Cek URL** (satu URL), **Cek Banyak** (teks per baris atau unggah CSV/TXT, maks. 200),
**Riwayat** (100 pengecekan terakhir), **Info Model** (metrik, confusion matrix, keterbatasan).

## Melatih ulang model

Buka notebook dari folder ini (supaya `features.py` dan folder `data/` terbaca), lalu
*Run All*. Lamanya sekitar 2-3 menit. Notebook menulis ulang `model/` dan grafik di `static/`.

Model `.joblib` terikat pada versi scikit-learn. File ini dibuat dengan scikit-learn 1.9.1.
Kalau versi yang terpasang berbeda dan muncul peringatan atau error saat memuat model,
jalankan ulang notebook untuk membuat model dengan versi kalian.

## Menjalankan uji fungsi

```bash
python uji_fungsi.py
```

## Hasil singkat (data uji, domain tidak pernah dilihat saat training)

| Accuracy | Precision (phishing) | Recall (phishing) | F1 (phishing) |
|---|---|---|---|
| 0,918 | 0,780 | 0,834 | 0,806 |

## Sumber

- Dataset: Phishing URL dataset, Mendeley Data, DOI 10.17632/vfszbj9b36.1, lisensi CC BY 4.0.
  https://data.mendeley.com/datasets/vfszbj9b36/1
- Pustaka: Flask, scikit-learn, pandas, NumPy, joblib, tldextract, matplotlib, seaborn.
- Bantuan AI: penyusunan kode notebook dan aplikasi dibantu asisten AI (Claude, Anthropic).
  Hasilnya diperiksa dan dipahami oleh anggota kelompok.

## Catatan keamanan

Aplikasi hanya membaca teks URL dan tidak pernah membuka tautannya. URL ancaman pada uji fungsi
adalah URL simulasi.
