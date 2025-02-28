# Tutorial Menjalankan Apache Kafka dengan Docker Compose

Tutorial ini menjelaskan cara menjalankan Apache Kafka menggunakan Docker Compose secara sederhana.

## 1. Prasyarat
Sebelum memulai, pastikan Anda telah menginstal:
✅ **Docker Desktop** → [Download di sini](https://www.docker.com/products/docker-desktop/)

✅ **Docker Compose** (biasanya sudah termasuk dalam Docker Desktop)

✅ **PowerShell, Terminal, atau Command Prompt**

## 2. Membuat File `docker-compose.yml`
Buka terminal dan pindah ke direktori tempat Anda ingin menyimpan file. Misalnya:

```powershell
cd path/ke/direktori/proyek
```

Buat file baru bernama `docker-compose.yml`, bisa menggunakan editor teks seperti Notepad atau VS Code. Jika menggunakan Notepad di Windows:

```powershell
notepad docker-compose.yml
```

Tempelkan skrip berikut ke dalam file:

```yaml
version: '3'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    ports:
      - "2181:2181"
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181

  kafka:
    image: confluentinc/cp-kafka:latest
    ports:
      - "9092:9092"
    environment:
      KAFKA_BROKER_ID: 1
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
      KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
    depends_on:
      - zookeeper
```

Simpan file dan tutup editor.

## 3. Menjalankan Kafka dan Zookeeper
Jalankan perintah berikut untuk menjalankan Kafka dan Zookeeper:

```powershell
docker-compose up -d
```

💡 **Penjelasan:**
- `up -d` → Menjalankan container dalam mode background (daemon mode).
- Jika ada peringatan tentang `version` yang obsolete, Anda bisa menghapus baris `version: '3'`.

## 4. Memeriksa Apakah Kafka Berjalan
Gunakan perintah berikut untuk mengecek container yang sedang berjalan:

```powershell
docker ps
```

Jika Kafka dan Zookeeper berjalan dengan benar, Anda akan melihat output seperti ini:

```bash
CONTAINER ID   IMAGE                              STATUS         PORTS
xxxxx          confluentinc/cp-kafka:latest       Up 2 minutes   0.0.0.0:9092->9092/tcp
yyyyy          confluentinc/cp-zookeeper:latest   Up 2 minutes   0.0.0.0:2181->2181/tcp
```

Jika Kafka tidak berjalan atau ada error, cek log dengan:

```powershell
docker-compose logs
```

## 5. Membuat Kafka Topic
Untuk membuat topic baru bernama `test-topic`, jalankan perintah berikut:

```powershell
docker exec -it kafka kafka-topics --create --topic test-topic --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1
```

💡 **Penjelasan:**
- `docker exec -it kafka` → Menjalankan perintah dalam container Kafka.
- `kafka-topics --create` → Membuat topik baru.
- `--topic test-topic` → Nama topik yang dibuat.
- `--bootstrap-server localhost:9092` → Kafka berjalan di port 9092.
- `--replication-factor 1` → Set faktor replikasi ke 1 (karena hanya ada satu broker).
- `--partitions 1` → Membuat satu partisi untuk topik ini.

Jika berhasil, akan muncul pesan:

```nginx
Created topic test-topic.
```

## 6. Mengirim Pesan ke Kafka (Producer)
Untuk mengirim pesan ke `test-topic`, jalankan perintah berikut:

```powershell
docker exec -it kafka kafka-console-producer --topic test-topic --bootstrap-server localhost:9092
```

Lalu ketik pesan Anda, misalnya:

```shell
>Halo Kafka!
>Test pesan 1
>Test pesan 2
```

Tekan **Ctrl + C** untuk keluar.

## 7. Membaca Pesan dari Kafka (Consumer)
Untuk membaca pesan yang telah dikirim ke `test-topic`, jalankan perintah berikut di terminal baru:

```powershell
docker exec -it kafka kafka-console-consumer --topic test-topic --bootstrap-server localhost:9092 --from-beginning
```

Output yang diharapkan:

```nginx
Halo Kafka!
Test pesan 1
Test pesan 2
```

Tekan **Ctrl + C** untuk keluar.

## 8. Menghentikan Kafka dan Zookeeper
Jika ingin menghentikan Kafka dan Zookeeper, jalankan perintah berikut:

```powershell
docker-compose down
```

Jika hanya ingin menghentikan sementara tanpa menghapus container:

```powershell
docker stop kafka zookeeper
```

## 9. Menghapus Container dan Volume
Jika ingin menghapus container, jaringan, dan volume data secara permanen:

```powershell
docker-compose down -v
```

💡 **Perintah ini akan menghapus semua data Kafka, termasuk topic yang telah dibuat.**

📌 **Ringkasan Perintah Penting:**
- `docker-compose up -d` → Menjalankan Kafka dan Zookeeper.
- `docker ps` → Mengecek container yang berjalan.
- `docker-compose down` → Menghentikan Kafka.
- `docker exec` → Berinteraksi dengan Kafka.

Jika ada pertanyaan atau error, silakan tanyakan! 🚀🔥

## Kesimpulan
✅ Sekarang Anda telah berhasil menjalankan Kafka dengan Docker Compose! 🚀

Jika ada pertanyaan atau error, silakan ajukan di **Issues** GitHub repository ini.

---
**Penulis:** Bintang Hari Kahono
