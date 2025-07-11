# Desafio Full Cycle - Imagem Docker com Go

Este repositório contém a solução para o desafio proposto pelo curso **Full Cycle**, onde o objetivo é criar uma imagem Docker extremamente enxuta que, ao ser executada, imprima a mensagem:

```
Full Cycle Rocks!!
```

---

## 🧩 O Desafio

1. Criar um programa simples em **Go** que imprime `Full Cycle Rocks!!`.
2. Construir uma **imagem Docker** contendo apenas o necessário para executar o binário.
3. A imagem precisa ter **menos de 2MB**.
4. Ao executar o comando:

```bash
docker run jotacosta/fullcycle
```

A saída deve ser exatamente:

```
Full Cycle Rocks!!
```

---

## ✅ Solução

Para atingir esse objetivo, foi utilizado:

- **Go (Golang)** para gerar um binário estático.
- **Build multistage** no Docker, com a imagem `golang:alpine` para compilação.
- **Imagem `scratch`** como base final, garantindo o menor tamanho possível.
- Uso de flags `-ldflags="-s -w"` para reduzir ainda mais o tamanho do binário.
- Execução sem módulos (`GO111MODULE=off`), visto que o projeto é simples e não usa dependências externas.

---

### 📁 Estrutura do Projeto

```
.
├── Dockerfile
└── main.go
```

---

### 📄 Arquivo `main.go`

```go
package main

import "fmt"

func main() {
    fmt.Println("Full Cycle Rocks!!")
}
```

---

### 🐳 Dockerfile utilizado

```Dockerfile
FROM golang:alpine AS build

WORKDIR /app

COPY main.go .

RUN GO111MODULE=off go build -ldflags="-s -w" -v -o fullcycle

FROM scratch

COPY --from=build /app/fullcycle /fullcycle

ENTRYPOINT [ "/fullcycle" ]
```

---

## 🚀 Como testar

### 1. Build da imagem:

```bash
docker build -t jotacosta/fullcycle .
```

### 2. Executar o container:

```bash
docker run --rm jotacosta/fullcycle
```

### ✅ Saída esperada:

```
Full Cycle Rocks!!
```

---

## 📦 Tamanho da Imagem

A imagem gerada possui aproximadamente **1.7MB**, atendendo ao requisito do desafio de manter a imagem abaixo de 2MB.

---

## 📤 Publicação

A imagem está disponível no Docker Hub:

🔗 [https://hub.docker.com/r/jotacosta/fullcycle](https://hub.docker.com/r/jotacosta/fullcycle)

---

## 👨‍💻 Autor

Desenvolvido por **Josimar Costa**.