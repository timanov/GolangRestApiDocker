Доступны API:
```bash
curl localhost:8000/books
````
```bash
curl localhost:8000/books/{id}
````
```bash
curl -X POST localhost8000:/books
````
```bash
curl -X PUT localhost:8000/books/{id}
```
```bash
curl -x DELETE localhost:8000/books/{id}
```

1. Скачивание зависимостей
```bash
go mod download
```

2. Билд приложения
```bash
go build ./src/app/Main.go
```

3. Запуск приложения go:
```bash
go run ./src/app/Main.go
```
или запуск собранного бинаря:
```bash
./Main
```

**Билд образа с приложением:**
```bash
docker build -t restgo:1.0 .
```

**Запуск контейнера:***
```bash
docker run --name restgo -d -p 9000:9000 restgo:1.0 
```

**Остановить и удалить контейнер:***
```bash
docker rm -f restgo
```

Узнать что за приложение запущено на 8080 порту:
```bash
netstat -tuln | grep 8000
```
или
```bash
sudo lsof -i :8000
```