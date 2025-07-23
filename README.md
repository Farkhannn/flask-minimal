#  flask-minimal

Starter project **Flask** minimalis untuk memulai pengembangan aplikasi web sederhana dengan cepat.  
Cocok untuk belajar Flask, membuat prototipe, dan proyek kecil dengan overhead minimal.

---

##  Deskripsi

`flask-minimal` adalah kerangka kerja awal untuk aplikasi berbasis Flask yang dirancang agar ringan dan mudah dikembangkan. Proyek ini mengemas semua logika utama dalam satu file `app.py`, sekaligus menyediakan struktur dasar untuk *template* dan *static assets*.

---

##  Fitur

-  Struktur satu file (`app.py`) untuk kemudahan pengembangan
-  Template HTML dasar (`templates/index.html`)
-  Folder `static/` untuk file CSS dan JavaScript
-  Siap dikembangkan tanpa konfigurasi tambahan
-  Dapat dijalankan otomatis saat boot di server EC2 (systemd)

---

##  Persiapan Sistem

Jalankan perintah berikut di Ubuntu server Anda (misalnya EC2):

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip -y
```

---

##  Instalasi Proyek

1. **Clone Repository**
   ```bash
   git clone https://github.com/yourusername/flask-minimal.git
   cd flask-minimal
   ```

2. **Buat dan Aktifkan Virtual Environment**
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install Dependensi**
   ```bash
   pip install -r requirements.txt
   ```

4. **Jalankan Aplikasi**
   ```bash
   python app.py
   ```

Aplikasi akan aktif di `http://localhost:5000`.  
Jika berjalan di EC2, pastikan port 5000 di-*allow* melalui Security Group agar bisa diakses publik.

---

##  Setup Otomatis systemd (Autostart di EC2)

Agar aplikasi Flask berjalan otomatis setelah reboot EC2, ikuti langkah berikut:

1. **Buat file systemd service**
   ```bash
   sudo nano /etc/systemd/system/flask-app.service
   ```

2. **Isi dengan konfigurasi berikut (ubah path jika perlu):**
   ```ini
   [Unit]
   Description=Flask Minimal App
   After=network.target

   [Service]
   Type=simple
   User=ubuntu
   WorkingDirectory=/home/ubuntu/flask-minimal
   Environment=PATH=/home/ubuntu/flask-minimal/venv/bin
   ExecStart=/home/ubuntu/flask-minimal/venv/bin/python app.py
   Restart=always
   RestartSec=10

   [Install]
   WantedBy=multi-user.target
   ```

3. **Aktifkan servicenya**
   ```bash
   sudo systemctl daemon-reload
   sudo systemctl enable flask-app
   sudo systemctl start flask-app
   ```

4. **Cek status**
   ```bash
   sudo systemctl status flask-app
