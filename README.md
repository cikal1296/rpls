# Web AI dengan Gemini API - Free Tier

Web AI chat sederhana menggunakan Gemini API (free tier) yang di-deploy ke Vercel.

## 📁 Struktur File (FIXED)

```
my-ai-web/
├── api/
│   └── chat.js          # Serverless function (ES6 module)
├── index.html           # Frontend chat (di root!)
├── package.json         # Dependencies
├── vercel.json          # Konfigurasi Vercel
└── .gitignore
```

**PENTING**: `index.html` harus di root, bukan di folder `public/`!

## 🚀 Cara Deploy ke Vercel

### 1. Dapetin API Key Gemini (GRATIS)

1. Buka https://makersuite.google.com/app/apikey atau https://aistudio.google.com/app/apikey
2. Klik "Create API Key" atau "Get API Key"
3. Copy API key yang dikasih

### 2. Deploy ke Vercel

**Opsi A: Via Vercel CLI (Recommended)**

```bash
# Masuk ke folder project
cd my-ai-web

# Install Vercel CLI (kalo belum punya)
npm install -g vercel

# Login ke Vercel
vercel login

# Deploy
vercel

# Ikuti instruksi:
# - Set up and deploy? Y
# - Which scope? (pilih account lo)
# - Link to existing project? N
# - Project name? (enter aja, atau kasih nama)
# - In which directory? ./ (enter)
# - Override settings? N

# Setelah deploy, lanjut ke step 3!
```

**Opsi B: Via GitHub + Vercel Dashboard**

1. Upload folder ini ke GitHub repository
2. Buka https://vercel.com
3. Klik "Add New" → "Project"
4. Import repository GitHub lo
5. Klik "Deploy"

**Opsi C: Drag & Drop**

1. Buka https://vercel.com/new
2. Drag & drop folder `my-ai-web` ke browser
3. Klik "Deploy"

### 3. Setup Environment Variable (WAJIB!)

**Ini step paling penting! Tanpa ini API ga bakal jalan.**

1. Setelah deploy, buka Vercel Dashboard
2. Pilih project lo
3. Masuk ke tab **"Settings"**
4. Klik **"Environment Variables"** di sidebar kiri
5. Tambah variable baru:
   - **Key/Name**: `GEMINI_API_KEY`
   - **Value**: [paste API key dari step 1]
   - **Environment**: Centang **Production**, **Preview**, **Development** (semua)
6. Klik **"Save"**
7. Balik ke tab **"Deployments"**
8. Klik titik tiga (...) di deployment paling atas
9. Klik **"Redeploy"** → pilih "Use existing Build Cache" → **"Redeploy"**

### 4. Tes Website Lo!

Buka URL yang dikasih Vercel (contoh: `https://your-project.vercel.app`)

Chat sekarang harusnya udah jalan!

## 🔧 Troubleshooting

### Error 404 di `/api/chat`

**Penyebab**: File struktur salah atau Vercel belum detect API folder

**Solusi**:
- Pastikan `api/chat.js` ada di folder `api/`
- Redeploy: `vercel --prod`
- Cek di Vercel Dashboard → Functions, harusnya ada `chat`

### Error "API Key belum di-setup"

**Penyebab**: Environment variable belum di-set

**Solusi**:
- Ikuti step 3 di atas dengan teliti
- WAJIB redeploy setelah tambah env variable
- Cek di Settings → Environment Variables, harusnya ada `GEMINI_API_KEY`

### Error "Failed to load resource"

**Penyebab**: CORS atau API endpoint ga ketemu

**Solusi**:
- Buka browser console (F12) → Network tab
- Cek request ke `/api/chat`, status code berapa?
- Kalo 404: API ga ke-detect, coba redeploy
- Kalo 500: Cek Vercel logs di Dashboard → Deployments → Functions

### Chat loading terus ga ada respon

**Solusi**:
- Cek Vercel Dashboard → Deployments → klik deployment terakhir → tab "Functions"
- Klik function `chat` → liat logs
- Biasanya error muncul di sini

## 💡 Tips

### Free Tier Limits Gemini:
- **60 requests per menit** (cukup banget buat personal use)
- **Gratis selamanya**
- Ga perlu credit card

### Cara Update Code:

1. Edit file yang mau diubah
2. Deploy ulang:
```bash
vercel --prod
```

### Custom Domain (Opsional):

1. Vercel Dashboard → Settings → Domains
2. Tambah domain lo
3. Update DNS sesuai instruksi

## 🎨 Customisasi

### Ubah warna/tema:
Edit bagian `<style>` di `index.html`

### Ubah model AI:
Di `api/chat.js` baris ini:
```javascript
const model = genAI.getGenerativeModel({ model: "gemini-pro" });
```

Model yang available (free):
- `gemini-pro` - Text only (recommended)
- `gemini-pro-vision` - Support gambar

### Tambah history chat:
Modify `api/chat.js`, simpen conversation history di array

## 📊 Cara Kerja

```
User → Frontend (index.html) 
    ↓
    POST /api/chat
    ↓
Serverless Function (api/chat.js)
    ↓
Gemini API (pake GEMINI_API_KEY dari env)
    ↓
Response balik ke Frontend
    ↓
Tampil di chat
```

## 🔒 Keamanan

✅ API key tersimpan aman di Vercel env variables (ga ke-expose)
✅ HTTPS otomatis dari Vercel
✅ CORS udah di-handle
⚠️ Kalo mau production-ready, tambahin rate limiting

## 📚 Resources

- [Gemini API Docs](https://ai.google.dev/docs)
- [Vercel Serverless Functions](https://vercel.com/docs/functions/serverless-functions)
- [Vercel CLI Docs](https://vercel.com/docs/cli)

---

Kalo masih error, share screenshot error-nya atau logs dari Vercel!
