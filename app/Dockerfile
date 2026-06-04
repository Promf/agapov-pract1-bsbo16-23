# Stage 1: Build
FROM golang:1.21-alpine AS builder

RUN apk add --no-cache git

WORKDIR /app

# Копируем все исходники сразу
COPY . .

# Скачиваем и проверяем зависимости
RUN go mod tidy && go mod download

# Собираем бинарник
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o main .

# Stage 2: Final
FROM alpine:latest

RUN apk --no-cache add ca-certificates

WORKDIR /app

# Копируем бинарник
COPY --from=builder /app/main .

# Копируем статику в /static (Go ищет файл по пути /static/index.html)
COPY --from=builder /app/static /static

EXPOSE 8080

CMD ["./main"]