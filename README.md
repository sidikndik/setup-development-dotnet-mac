# 💻 .NET Development Setup for macOS
Panduan ini dikhususkan untuk developer yang beralih ke Mac atau baru melakukan clean install.

## 1. Persiapan Awal (Essential Tools)
Sebelum masuk ke .NET, kita butuh "fondasi" agar instalasi tool lainnya lebih mudah.

- Xcode Command Line Tools: Wajib untuk tool development.
```bash
xcode-select --install
```

- Homebrew: Package manager terbaik untuk macOS.
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

## 2. Instalasi asdf & .NET Plugin
Alih-alih menginstal .NET secara global, kita gunakan asdf untuk manajemen versi yang lebih fleksibel.

### A. Install asdf
```bash
brew install asdf
```

Pastikan kamu menambahkan asdf ke shell kamu (tambahkan ke ~/.zshrc):
```bash
echo ". $(brew --prefix asdf)/libexec/asdf.sh" >> ~/.zshrc
source ~/.zshrc
```

### B. Install Plugin .NET
```bash
asdf plugin add dotnet
```

### C. Install .NET SDK Spesifik
Sekarang kamu bisa memilih versi yang diinginkan (misalnya .NET 10):
```bash
# Cek versi yang tersedia
asdf list-all dotnet

# Install versi terbaru (contoh: 10.0.103)
asdf install dotnet 10.0.103

# Set versi tersebut sebagai default global
asdf set -u dotnet 10.0.103
```

Tips: Keuntungan asdf adalah kamu bisa set `asdf set dotnet <version>` di dalam folder project tertentu agar project lama tetap pakai SDK lama.

## 3. Setup VS Code (The Lean Machine)
Karena kita akan menggunakan VS Code secara eksklusif, kita perlu memastikan integrasi C# berjalan sempurna.

### A. Install VS Code
```bash
brew install --cask visual-studio-code
```

### B. Ekstensi Wajib
Buka VS Code (Command + Shift + X) dan instal ekstensi berikut:
1. **C# Dev Kit**: Ekstensi resmi Microsoft yang memberikan fitur mirip Visual Studio (Solution Explorer, Test Explorer).
2. **C#**: Language support (Omnisharp/Roslyn).
3. **IntelliCode for C# Dev Kit**: AI untuk auto-complete kode.
4. **.NET Install Tool**: Membantu ekstensi VS Code menemukan jalur SDK dari asdf.

## 4. Database & Containerization
Karena SQL Server tidak berjalan native di Mac, kita gunakan Docker.
- Docker Desktop: `brew install --cask docker`
- Azure Data Studio: (Paling pas dipasangkan dengan VS Code) `brew install --cask azure-data-studio`

Run SQL Server (Azure SQL Edge) di Docker:
```bash
docker run --cap-add SYS_PTRACE -e "ACCEPT_EULA=1" -e "MSSQL_SA_PASSWORD=YourStrongPassword123!" \
-p 1433:1433 --name sql_server -d mcr.microsoft.com/azure-sql-edge
```

## 5. Verifikasi Akhir
Buka terminal dan pastikan semuanya terhubung:
```bash
# Cek versi .NET
dotnet --version

# Cek lokasi binary (harus mengarah ke folder .asdf)
which dotnet
```

## 🚀 Membuat Project Pertama (Step-by-Step)
Setelah asdf dan VS Code siap, ikuti langkah ini untuk membuat Web API pertama kamu:

### Langkah 1: Buat Folder Project
Buka terminal dan navigasi ke tempat kamu menyimpan project:
```bash
mkdir MyDotNetProjects
cd MyDotNetProjects
```

### Langkah 2: Generate Project via Terminal
Gunakan template Web API (yang paling umum digunakan):
```bash
# Membuat project baru dengan folder 'MyAwesomeApi'
dotnet new webapi -n MyAwesomeApi

# Masuk ke folder project
cd MyAwesomeApi
```

### Langkah 3: Buka di VS Code
Ketik perintah ini di terminal:
```bash
code .
```

Catatan: Jika muncul pesan "Do you trust the authors of the files in this folder?", pilih Yes.

### Langkah 4: Jalankan Project
1. Buka terminal terintegrasi di VS Code (Ctrl + ~).
2. Ketik:
    ```bash
    dotnet run
    ```
3. Lihat output di terminal, biasanya akan muncul URL seperti `http://localhost:5xxx`.
4. Buka browser dan tambahkan /swagger atau /scalar di akhir URL (tergantung versi .NET) untuk melihat dokumentasi API kamu.