# Tutorial Menjalankan Apache Kafka dengan Docker Compose

Tutorial ini menjelaskan cara menjalankan Apache Kafka menggunakan Docker Compose secara sederhana.

## Prasyarat
Sebelum memulai, pastikan Anda telah menginstal:
- **Docker Desktop**: [Download di sini](https://www.docker.com/products/docker-desktop/)
- **Docker Compose** (biasanya sudah termasuk dalam Docker Desktop)
- **Terminal atau PowerShell**

## Cara Menggunakan

### 1. Clone Repository ini
```sh
git clone https://github.com/harikahono/kafka-docker-tutorial.git
cd kafka-docker-tutorial
```

### 2. Jalankan Kafka dan Zookeeper
```sh
docker-compose up -d
```
> **Penjelasan:** `-d` menjalankan container di background.

### 3. Periksa Status Container
```sh
docker ps
```

### 4. Membuat Kafka Topic
```sh
docker exec -it kafka kafka-topics --create --topic test-topic --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1
```

### 5. Mengirim Pesan ke Kafka (Producer)
```sh
docker exec -it kafka kafka-console-producer --topic test-topic --bootstrap-server localhost:9092
```
Kemudian ketik pesan dan tekan **Enter**.

### 6. Membaca Pesan dari Kafka (Consumer)
```sh
docker exec -it kafka kafka-console-consumer --topic test-topic --bootstrap-server localhost:9092 --from-beginning
```

### 7. Menghentikan Kafka dan Zookeeper
```sh
docker-compose down
```

## Struktur Proyek
```
📂 kafka-docker-tutorial
 ┣ 📜 docker-compose.yml
 ┣ 📜 README.md
```

## Kesimpulan
Sekarang Anda telah berhasil menjalankan Kafka dengan Docker Compose! 🚀

Jika ada pertanyaan atau error, silakan ajukan di **Issues** GitHub repository ini.

---
**Penulis:** Bintang Hari Kahono
