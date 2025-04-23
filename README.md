```markdown
# 🚀 Rilis Canggih dan Otomatis: Menggunakan GitHub Actions dan Semantic Versioning

Mempelajari cara menggunakan Semantic Release di GitHub untuk otomatisasi rilis versi proyek Anda.  

Semantic Release adalah alat otomatisasi untuk:
- Mengelola versi proyek menggunakan [Semantic Versioning](https://semver.org/).
- Membuat file changelog berdasarkan commit.
- Menerbitkan rilis ke GitHub dengan artefak terkait (opsional).

Keuntungan:
1. Menghilangkan proses manual pengelolaan versi.
2. Membantu tim memahami perubahan di setiap rilis melalui changelog otomatis.
3. Mendukung integrasi dengan berbagai bahasa pemrograman.
```
---

## 🛠️ Persyaratan
1. GitHub Repository yang sudah dikonfigurasi.
2. Node.js di sistem Anda (diperlukan untuk menjalankan Semantic Release).

---

## 🛠️ Step 1: Configure Workflow Permissions
Ikuti langkah-langkah berikut untuk mengatur **Workflow Permissions**:
1. Buka repository Anda di **GitHub**.
2. Pergi ke **Settings > Actions > General**.
3. Gulir ke bagian **Workflow permissions**.
4. Pilih opsi **Read and write permissions**.
5. Klik tombol **Save** untuk menyimpan pengaturan.

---

## 📁 Step 2: Clone Repository to Local
Jika project belum ada di komputer lokal, gunakan perintah berikut untuk menyalinnya:
```bash
git clone https://github.com/your-project-name.git
cd your-project-name
```
> **Note:** Pastikan Anda mengganti `your-project-name` dengan nama repository Anda.

---

### 3. (Opsional) Inisialisasi Proyek Node.js
> **Note:** Jika proyek Anda **tidak berbasis Node.js**, maka Anda dapat melewati langkah ini dan langsung melanjutkan ke Step 4.

Ikuti langkah-langkah berikut untuk menginisialisasi proyek:

1. Inisialisasi proyek Node.js dengan perintah:
   ```bash
   npm init -y
   ```
2. Tambahkan file `.gitignore` untuk mengabaikan folder `node_modules`:
   ```bash
   echo "node_modules/" > .gitignore
   ```
3. Commit perubahan:
   ```bash
   git add .
   git commit -m "chore: Initialize project with Node.js"
   git push origin main
   ```
---

## 📥 Step 4: Install Semantic Release
1. Instal Semantic Release dan plugin yang diperlukan:
   ```bash
   npm install --save-dev \
   semantic-release \
   @semantic-release/commit-analyzer \
   @semantic-release/release-notes-generator \
   @semantic-release/changelog \
   @semantic-release/github \
   conventional-changelog-conventionalcommits
   ```
2. Pastikan semua dependensi terinstal dengan menjalankan:
   ```bash
   npm install
   ```
3. Commit perubahan:
   ```bash
   git add package.json package-lock.json
   git commit -m "chore: Install Semantic Release and plugins"
   git push origin main
   ```

---

## ⚙️ Step 5: Configure Semantic Release
1. Buat file `.releaserc` di root folder proyek:
   ```bash
   nano .releaserc
   ```
2. Isi file `.releaserc` dengan konfigurasi berikut:
   ```json
   {
     "branches": ["main"],
     "plugins": [
       [
         "@semantic-release/commit-analyzer",
         {
           "preset": "conventionalcommits"
         }
       ],
       [
         "@semantic-release/release-notes-generator",
         {
           "preset": "conventionalcommits"
         }
       ],
       [
         "@semantic-release/changelog",
         {
           "changelogTitle": "# Changelog\n\nAll notable changes to this project will be documented in this file. See [Conventional Commits](https://conventionalcommits.org) for commit guidelines."
         }
       ],
       [
         "@semantic-release/github",
         {
           "assets": [
             {"path": "build.zip", "label": "Build"}
           ]
         }
       ]
     ]
   }
   ```
3. Commit perubahan:
   ```bash
   git add .releaserc
   git commit -m "chore: Add Semantic Release configuration"
   git push origin main
   ```

---

## 🔧 Step 6: Configure GitHub Actions
1. Buat folder untuk workflow:
   ```bash
   mkdir -p .github/workflows
   ```
2. Buat file `release.yml` di dalam folder tersebut:
   ```bash
   nano .github/workflows/release.yml
   ```
3. Tambahkan konfigurasi berikut ke file `release.yml`:
   ```yaml
   name: Release

   on:
    push:
      branches:
        - main
    pull_request:
      branches:
        - main

   jobs:
     release:
       name: Release
       runs-on: ubuntu-latest
       steps:
         - name: Checkout repository
           uses: actions/checkout@v3

         - name: Setup Node.js
           uses: actions/setup-node@v3
           with:
             node-version: 'lts/*'

         - name: Install dependencies
           run: npm ci

         - name: Run Semantic Release
           env:
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
           run: npx semantic-release
   ```
4. Commit perubahan:
   ```bash
   git add .github/workflows/release.yml
   git commit -m "chore: Add GitHub Actions workflow for release"
   git push origin main
   ```

---

## 📝 Step 7: Commit Message Format
Gunakan format berikut untuk commit agar kompatibel dengan Semantic Release:

1. **MAJOR** (Perubahan besar, tidak kompatibel dengan versi sebelumnya):
   ```bash
   git commit -m 'feat(subscription)!: Add subscription feature'
   ```
2. **MINOR** (Fitur baru yang kompatibel dengan versi sebelumnya):
   ```bash
   git commit -m 'feat(login): Add login feature'
   ```
3. **PATCH** (Bugfix atau peningkatan kecil):
   ```bash
   git commit -m 'fix(login): Fix login button'
   ```

---

🎉 **Proyek Anda sekarang telah berhasil diatur dengan Semantic Release dan GitHub Actions!**

**Selamat Mencoba! 🚀**

---

### © 2025 Ronald Sianipar