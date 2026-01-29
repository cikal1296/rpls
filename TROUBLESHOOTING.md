# 🔧 TROUBLESHOOTING GUIDE - Error 500 Fix

## Error yang Lo Alami:

```
/api/chat:1 Failed to load resource: the server responded with a status of 500 ()
Error: Gagal mendapatkan respons dari AI
```

## ✅ SOLUSI STEP-BY-STEP:

### STEP 1: Cek API Key Udah Bener Belum

1. Masuk ke **Vercel Dashboard** (https://vercel.com)
2. Pilih project lo
3. Klik tab **"Settings"**
4. Klik **"Environment Variables"** di sidebar
5. **CEK INI**:
   - Ada variable dengan name **`GEMINI_API_KEY`**? 
   - Value-nya udah diisi API key yang bener?
   - Centang **Production**, **Preview**, DAN **Development**?

**KALO BELUM ADA atau SALAH:**

6. Klik **"Add New"**
7. Isi:
   ```
   Key: GEMINI_API_KEY
   Value: [paste API key lo dari Google AI Studio]
   ```
8. Centang **SEMUA** environment (Production, Preview, Development)
9. Klik **"Save"**

### STEP 2: Redeploy (WAJIB!)

Setelah tambah/edit environment variable, lo **WAJIB REDEPLOY**:

1. Klik tab **"Deployments"**
2. Klik titik tiga (...) di deployment paling atas
3. Klik **"Redeploy"**
4. Pilih **"Use existing Build Cache"**
5. Klik **"Redeploy"** lagi

Tunggu sampe selesai deploy (ada centang hijau ✓)

### STEP 3: Cek Function Logs

Kalo masih error 500, cek logs-nya:

1. Di **Deployments**, klik deployment yang baru
2. Klik tab **"Functions"**
3. Klik function **"api/chat.js"**
4. Scroll ke bawah, liat **"Recent Invocations"**
5. Klik salah satu invocation untuk liat error detail

**Error yang mungkin muncul:**

### ERROR: "Cannot find module '@google/generative-ai'"

**Solusi**: Dependencies ga ke-install

1. Hapus folder `node_modules` (kalo ada)
2. Pastikan `package.json` ada
3. Redeploy:
```bash
vercel --prod
```

### ERROR: "GEMINI_API_KEY is not defined"

**Solusi**: API key belum ke-load

1. Ulang STEP 1 di atas
2. PASTIIN redeploy setelah save env variable
3. Tunggu 1-2 menit setelah redeploy baru test lagi

### ERROR: "API key not valid"

**Solusi**: API key salah atau expired

1. Buka https://aistudio.google.com/app/apikey
2. Bikin API key baru (kalo perlu)
3. Copy API key yang BARU
4. Update di Vercel Environment Variables
5. Redeploy

### ERROR: "Quota exceeded" atau "Rate limit"

**Solusi**: Gemini free tier limit tercapai

Free tier limits:
- 60 requests per menit
- Kalo lebih, harus tunggu 1 menit

Tunggu 1 menit terus coba lagi.

## 🧪 TEST MANUAL

Kalo mau test API endpoint langsung, pake cara ini:

### Via Browser Console (F12):

```javascript
fetch('https://YOUR-PROJECT.vercel.app/api/chat', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: JSON.stringify({ message: 'Hello' })
})
.then(r => r.json())
.then(d => console.log(d))
```

Ganti `YOUR-PROJECT` dengan domain Vercel lo.

### Via curl (Terminal):

```bash
curl -X POST https://YOUR-PROJECT.vercel.app/api/chat \
  -H "Content-Type: application/json" \
  -d '{"message":"Hello"}'
```

**Response yang BENER:**
```json
{
  "success": true,
  "response": "Hello! How can I help you today?"
}
```

**Response ERROR:**
```json
{
  "error": "API Key belum di-setup di Vercel Environment Variables"
}
```

## 🔍 CEK VERCEL LOGS REAL-TIME

1. Buka Terminal
2. Install Vercel CLI (kalo belum):
```bash
npm install -g vercel
```

3. Login:
```bash
vercel login
```

4. Masuk ke folder project:
```bash
cd my-ai-web
```

5. Liat logs real-time:
```bash
vercel logs --follow
```

6. Buka website lo di browser, test chat
7. Liat logs di terminal - bakal muncul error detail

## ⚡ QUICK FIX CHECKLIST

Centang satu-satu:

- [ ] API key udah dibuat di Google AI Studio
- [ ] API key udah di-paste ke Vercel Environment Variables
- [ ] Name environment variable persis: `GEMINI_API_KEY` (case-sensitive!)
- [ ] Semua environment (Production, Preview, Development) udah dicentang
- [ ] Udah redeploy setelah save environment variable
- [ ] Tunggu 1-2 menit setelah redeploy sebelum test
- [ ] Browser cache udah di-clear (Ctrl+Shift+R / Cmd+Shift+R)
- [ ] Test di incognito/private window

## 🆘 MASIH ERROR?

Kirim info ini:

1. **Screenshot** dari Vercel → Settings → Environment Variables
2. **Screenshot** dari Vercel → Deployments → Functions → Logs
3. **Screenshot** dari Browser Console (F12) pas error muncul

Dengan info itu gua bisa bantu debug lebih lanjut!

## 📝 COMMON MISTAKES

❌ **SALAH**: Nulis API key langsung di code
✅ **BENER**: Simpen di Vercel Environment Variables

❌ **SALAH**: Nama variable: `GEMINI_KEY` atau `API_KEY`
✅ **BENER**: Nama variable persis: `GEMINI_API_KEY`

❌ **SALAH**: Cuma centang Production aja
✅ **BENER**: Centang Production + Preview + Development

❌ **SALAH**: Ga redeploy setelah tambah env variable
✅ **BENER**: WAJIB redeploy setelah save env variable

---

Ikutin step-by-step di atas, harusnya fix! 💪
