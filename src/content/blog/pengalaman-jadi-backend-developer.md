---
title: "Setahun Jadi Backend Developer: Pelajaran yang Mengubah Cara Saya Menulis Kode"
description: "Refleksi perjalanan satu tahun sebagai backend developer — dari junior yang takut deploy, hingga mulai percaya diri mendesain sistem."
date: 2026-01-05
tags: ["career", "backend", "reflection", "tips"]
---

# Setahun Jadi Backend Developer

Satu tahun bukan waktu yang lama, tapi bagi seorang developer, itu bisa terasa seperti seumur hidup kalau kamu belajar dengan sungguh-sungguh. Ini adalah refleksi jujur dari perjalanan saya.

## Awal yang Penuh Kebingungan

Hari pertama kerja, saya diberikan codebase dengan puluhan ribu baris kode. Tidak ada dokumentasi yang memadai. Senior bilang, "Coba explore dulu, nanti kalau ada yang nggak ngerti tanya."

Saya buka file pertama. Saya tutup lagi.

Saya ulangi lima kali.

Pelajaran pertama: **kemampuan membaca kode orang lain sama pentingnya dengan kemampuan menulis kode sendiri**.

## Hal-hal yang Saya Pelajari Selama Setahun

### 1. Database Bukan Hanya Tempat Menyimpan Data

Sebelumnya saya pikir database hanya tentang `SELECT`, `INSERT`, `UPDATE`. Ternyata saya salah besar.

- **Indexing** bisa mengubah query dari 10 detik menjadi 50 millisecond
- **Transaction** menjaga konsistensi data saat terjadi kegagalan
- **EXPLAIN ANALYZE** adalah teman terbaik ketika query lambat
- **N+1 query** adalah musuh yang tidak kelihatan tapi mematikan performa

```sql
-- ❌ Sebelum: N+1 problem
SELECT * FROM users;
-- Lalu untuk setiap user:
SELECT * FROM orders WHERE user_id = ?;

-- ✅ Sesudah: JOIN yang proper
SELECT u.*, o.*
FROM users u
LEFT JOIN orders o ON o.user_id = u.id;
```

### 2. Error Handling Itu Seni

Kode yang baik bukan hanya yang berhasil dieksekusi, tapi juga yang gagal dengan **bermartabat**.

```go
// ❌ Sebelum
func getUser(id int) *User {
    user, _ := db.FindUser(id)
    return user
}

// ✅ Sesudah
func getUser(id int) (*User, error) {
    user, err := db.FindUser(id)
    if err != nil {
        return nil, fmt.Errorf("getUser: %w", err)
    }
    if user == nil {
        return nil, ErrUserNotFound
    }
    return user, nil
}
```

### 3. Logging yang Berguna

Log bukan sekadar `fmt.Println("masuk sini")`. Log yang baik harus bisa menjawab: **apa yang terjadi, kapan, dan pada konteks apa?**

```go
log.WithFields(log.Fields{
    "user_id":    userID,
    "action":     "transfer",
    "amount":     amount,
    "request_id": ctx.Value("request_id"),
}).Info("Transfer initiated")
```

### 4. Review Code Adalah Hadiah, Bukan Hukuman

Awalnya saya takut kalau PR saya diberi banyak komentar. Sekarang saya justru senang — setiap komentar adalah kesempatan belajar gratis dari orang yang lebih berpengalaman.

### 5. Dokumentasi = Surat Cinta untuk Diri Sendiri di Masa Depan

Kode yang kamu tulis hari ini akan kamu baca 6 bulan lagi dalam kondisi bingung. Tulis komentar untuk hal-hal yang **mengapa**, bukan yang **apa**.

```go
// Gunakan 3 retry dengan backoff eksponensial karena downstream service
// sesekali mengalami spike latency saat jam sibuk (07:00-09:00 WIB).
// Lihat: https://jira.company.com/TICK-1234
for i := 0; i < 3; i++ {
    err = callDownstream()
    if err == nil {
        break
    }
    time.Sleep(time.Duration(math.Pow(2, float64(i))) * time.Second)
}
```

## Yang Masih Terus Saya Pelajari

- Desain sistem distribusi yang fault-tolerant
- Observability: metrics, tracing, dan alerting yang proper
- Keamanan aplikasi dari perspektif developer

## Pesan untuk Junior Developer

Jangan takut bertanya. Jangan takut salah. Kode yang jelek tapi jujur jauh lebih baik dari kode yang kelihatan pintar tapi tidak ada yang ngerti.

Semua senior pernah jadi junior. 🙏
