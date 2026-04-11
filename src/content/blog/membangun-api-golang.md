---
title: "Membangun REST API yang Scalable dengan Golang"
description: "Panduan praktis membangun REST API production-ready menggunakan Golang, Gin framework, dan PostgreSQL dengan pola clean architecture."
date: 2026-03-20
tags: ["golang", "backend", "api", "clean-architecture"]
---

# Membangun REST API yang Scalable dengan Golang

Golang telah menjadi pilihan utama bagi banyak engineer backend untuk membangun layanan yang performan dan mudah di-maintain. Dalam artikel ini, kita akan membahas cara membangun REST API yang scalable menggunakan Golang dengan pendekatan **clean architecture**.

## Mengapa Golang?

Golang menawarkan beberapa keunggulan dibanding bahasa lain untuk backend development:

- **Performa tinggi** – Compiled language dengan garbage collector yang efisien
- **Concurrency native** – Goroutine dan channel membuat async programming terasa natural
- **Ekosistem yang matang** – Library seperti `gin`, `echo`, dan `fiber` sangat production-ready
- **Deployment simpel** – Single binary, tidak perlu runtime tambahan

## Setup Project

```bash
mkdir my-api && cd my-api
go mod init github.com/username/my-api
go get github.com/gin-gonic/gin
go get gorm.io/gorm
go get gorm.io/driver/postgres
```

## Struktur Folder (Clean Architecture)

```
my-api/
├── cmd/
│   └── main.go
├── internal/
│   ├── domain/
│   │   └── user.go
│   ├── repository/
│   │   └── user_repository.go
│   ├── usecase/
│   │   └── user_usecase.go
│   └── delivery/
│       └── http/
│           └── user_handler.go
├── pkg/
│   └── database/
│       └── postgres.go
└── go.mod
```

## Mendefinisikan Domain

```go
// internal/domain/user.go
package domain

import "time"

type User struct {
    ID        uint      `json:"id" gorm:"primaryKey"`
    Name      string    `json:"name"`
    Email     string    `json:"email" gorm:"unique"`
    CreatedAt time.Time `json:"created_at"`
}

type UserRepository interface {
    FindByID(id uint) (*User, error)
    Save(user *User) error
    FindAll() ([]User, error)
}

type UserUsecase interface {
    GetUser(id uint) (*User, error)
    CreateUser(user *User) error
    ListUsers() ([]User, error)
}
```

## Handler Layer

```go
// internal/delivery/http/user_handler.go
package http

import (
    "net/http"
    "strconv"

    "github.com/gin-gonic/gin"
    "github.com/username/my-api/internal/domain"
)

type UserHandler struct {
    usecase domain.UserUsecase
}

func (h *UserHandler) GetUser(c *gin.Context) {
    id, err := strconv.ParseUint(c.Param("id"), 10, 64)
    if err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"error": "invalid id"})
        return
    }

    user, err := h.usecase.GetUser(uint(id))
    if err != nil {
        c.JSON(http.StatusNotFound, gin.H{"error": "user not found"})
        return
    }

    c.JSON(http.StatusOK, user)
}
```

## Tips Production

1. **Selalu gunakan context timeout** pada setiap database call
2. **Implementasi graceful shutdown** agar request yang sedang berjalan selesai lebih dulu
3. **Gunakan middleware** untuk logging, recovery, dan rate limiting
4. **Pisahkan konfigurasi** menggunakan environment variables

## Kesimpulan

Clean architecture memisahkan concern dengan jelas, membuat codebase lebih testable dan mudah dikembangkan. Golang dengan pendekatannya yang eksplisit sangat cocok dipadukan dengan pola ini.

Happy coding! 🚀
