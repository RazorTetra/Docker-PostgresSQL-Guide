# Membuat dan Mengakses Database PostgreSQL di Docker di Windows

## 1. Persiapan

Pastikan Docker Desktop sudah terinstall dan berjalan di mesin Windows kamu. Kamu juga memerlukan Docker Compose jika menggunakan file `docker-compose.yml`.

## 2. Menyiapkan File `docker-compose.yml`

1. Buat direktori baru di sistem kamu untuk proyek ini.

2. Di dalam direktori tersebut, buat file bernama `docker-compose.yml` dengan konten berikut:

    ```yaml
    version: '3.8'
    
    services:
      postgres:
        image: sha256:74cc00b2e28f8b5cad42680cc425b261544eee3dfe70fbdf903015dac9b0fe4a
        container_name: postgres-okupasi
        environment:
          - POSTGRES_USER=postgres
          - POSTGRES_PASSWORD=yourpassword
          - POSTGRES_DB=okupasi
        ports:
          - "5433:5432"
        volumes:
          - postgres-okupasi-volume:/var/lib/postgresql/data

    volumes:
      postgres-okupasi-volume:
    ```

    **Catatan:**
    - Ganti `yourpassword` dengan password yang kamu inginkan untuk pengguna PostgreSQL.
    - File ini mendefinisikan service `postgres` dengan image tertentu, port mapping dari port container 5432 ke port host 5433, dan volume untuk data persisten.

## 3. Menjalankan Docker Compose

1. Buka **Command Prompt** atau **PowerShell** di Windows.

2. Pindah ke direktori di mana file `docker-compose.yml` berada menggunakan perintah `cd`:

    ```bash
    cd path\to\your\directory
    ```

3. Jalankan perintah berikut untuk memulai container:

    ```bash
    docker-compose up -d
    ```

    Perintah ini akan mengunduh image yang diperlukan (jika belum ada), membuat container, dan menjalankannya di background.

## 4. Memverifikasi Container Berjalan

Untuk memastikan container berjalan dengan baik, jalankan:

    ```bash
    docker ps
    ```

Ini akan menampilkan daftar container yang sedang berjalan. Kamu harus melihat container dengan nama `postgres-okupasi`.

## 5. Mengakses Database PostgreSQL dari Terminal Windows

1. **Install PostgreSQL Client**

   Jika kamu belum memiliki PostgreSQL client di mesin Windows, kamu perlu menginstalnya. Kamu bisa mengunduhnya dari [situs PostgreSQL](https://www.postgresql.org/download/windows/) dan mengikuti panduan instalasi.

2. **Mengakses Database PostgreSQL**

   Buka **Command Prompt** atau **PowerShell** di Windows, lalu jalankan perintah berikut untuk mengakses database PostgreSQL yang berjalan di container:

    ```bash
    psql -h localhost -p 5433 -U postgres -d okupasi
    ```

    **Penjelasan:**
    - `-h localhost`: Menghubungkan ke host `localhost`.
    - `-p 5433`: Menggunakan port 5433 (port yang kamu set di `docker-compose.yml`).
    - `-U postgres`: Username untuk PostgreSQL.
    - `-d okupasi`: Nama database yang ingin diakses.

3. **Masukkan Password**

   Setelah menjalankan perintah di atas, kamu akan diminta untuk memasukkan password yang sudah kamu set di file `docker-compose.yml` (`yourpassword`).

## 6. Menggunakan PostgreSQL CLI

Setelah berhasil login, kamu dapat menjalankan perintah SQL untuk berinteraksi dengan database, seperti:

- Melihat daftar tabel:

    ```sql
    \dt
    ```

- Melihat detail tabel tertentu:

    ```sql
    \d table_name
    ```

- Menjalankan query SQL:

    ```sql
    SELECT * FROM your_table;
    ```

## 7. Menghentikan dan Menghapus Container

Jika kamu ingin menghentikan dan menghapus container, jalankan:

    ```bash
    docker-compose down
    ```

Ini akan menghentikan dan menghapus semua container yang didefinisikan di file `docker-compose.yml`, tetapi volume akan tetap ada sehingga data database tidak hilang.

---

Dokumentasi ini seharusnya memandu kamu dari awal hingga akhir dalam membuat dan mengakses database PostgreSQL di Docker di Windows. Jika ada pertanyaan lebih lanjut atau masalah lain, jangan ragu untuk bertanya!