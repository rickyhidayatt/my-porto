---
title: "Belajar TypeScript dari Nol: Panduan untuk Developer JavaScript"
description: "Mulai perjalanan TypeScript-mu. Dari tipe dasar, interface, generics, hingga integrasi dengan framework modern."
date: 2026-02-14
tags: ["typescript", "javascript", "frontend", "backend"]
---

# Belajar TypeScript dari Nol

Jika kamu sudah familiar dengan JavaScript tapi belum pernah menyentuh TypeScript, artikel ini adalah titik awal yang tepat. TypeScript bukan bahasa yang berbeda — ia adalah **JavaScript dengan superpower**.

## Apa Itu TypeScript?

TypeScript adalah superset dari JavaScript yang menambahkan **static typing**. Artinya, kamu bisa mendefinisikan tipe data secara eksplisit, dan compiler akan menangkap kesalahan sebelum kode dijalankan.

```typescript
// JavaScript
function tambah(a, b) {
    return a + b;
}

// TypeScript
function tambah(a: number, b: number): number {
    return a + b;
}

console.log(tambah(2, 3));    // ✅ 5
console.log(tambah(2, "3")); // ❌ Error saat compile!
```

## Tipe Dasar

```typescript
// Primitives
let nama: string = "Ricky";
let umur: number = 25;
let aktif: boolean = true;

// Array
let skills: string[] = ["Golang", "TypeScript", "PostgreSQL"];
let scores: Array<number> = [90, 85, 92];

// Tuple
let koordinat: [number, number] = [106.8, -6.2];

// Union type
let id: string | number = "abc123";
id = 42; // juga valid
```

## Interface vs Type

```typescript
// Interface — cocok untuk objek dan class
interface User {
    id: number;
    name: string;
    email?: string; // optional
}

// Type alias — lebih fleksibel
type Status = "active" | "inactive" | "pending";
type ID = string | number;

// Menggunakan keduanya
interface ApiResponse<T> {
    data: T;
    status: Status;
    message: string;
}

const response: ApiResponse<User> = {
    data: { id: 1, name: "Ricky" },
    status: "active",
    message: "Success",
};
```

## Generics

Generics memungkinkan kamu membuat fungsi atau class yang bisa bekerja dengan berbagai tipe:

```typescript
function ambilPertama<T>(arr: T[]): T | undefined {
    return arr[0];
}

const angka = ambilPertama([1, 2, 3]);   // type: number
const kata = ambilPertama(["a", "b"]);   // type: string
```

## Integrasi dengan Node.js / Express

```typescript
import express, { Request, Response } from "express";

const app = express();

interface UserBody {
    name: string;
    email: string;
}

app.post("/users", (req: Request<{}, {}, UserBody>, res: Response) => {
    const { name, email } = req.body;
    res.json({ id: 1, name, email });
});
```

## Tips Memulai

1. **Aktifkan strict mode** di `tsconfig.json` — ini memaksa kamu menulis kode yang lebih aman
2. **Jangan gunakan `any` secara berlebihan** — ini mengalahkan tujuan TypeScript
3. **Mulai dengan file `.ts` sederhana** sebelum migrasi proyek besar
4. **Manfaatkan type inference** — TypeScript sering bisa menebak tipe tanpa kamu tulis eksplisit

```json
// tsconfig.json
{
  "compilerOptions": {
    "strict": true,
    "target": "ES2020",
    "module": "commonjs",
    "outDir": "./dist"
  }
}
```

## Kesimpulan

TypeScript adalah investasi yang sangat worth it. Setelah terbiasa, kamu tidak akan mau kembali ke JavaScript murni. Editor support yang luar biasa dan deteksi error saat develop adalah game changer untuk produktivitas.

Selamat belajar! 💻
