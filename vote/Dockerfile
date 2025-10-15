# Menggunakan image Python untuk menginstall dependensi
FROM python:3.9-slim as builder

# Menetapkan direktori kerja di dalam container
WORKDIR /app

# Menyalin file daftar dependensi
COPY requirements.txt .

# Mengunduh dependensi sebagai 'wheels' (paket terkompilasi)
# lebih efisien untuk tahap selanjutnya
RUN pip wheel --no-cache-dir --wheel-dir /wheels -r requirements.txt

# Menggunakan image Python yang jauh lebih kecil untuk produksi
FROM python:3.9-slim

# Menetapkan direktori kerja
WORKDIR /app

# Menyalin 'wheels' yang sudah jadi dari stage 'builder'
COPY --from=builder /wheels /wheels

# Menyalin kode aplikasi
COPY . .

# Menginstall dependensi dari 'wheels' lokal tanpa perlu download lagi
RUN pip install --no-cache /wheels/*

# Menentukan port yang akan diekspos oleh container
EXPOSE 80

# Perintah untuk menjalankan aplikasi saat container dimulai
CMD ["python", "app.py"]