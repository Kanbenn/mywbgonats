Здравствуйте. Спасибо, что нашли время на код-ревью.
Старался выполнить все пункты задания. Получилось сделать всё, кроме бенчмарков - не дошли руки.
Краткие пояснения к коду по заданию:

# Данные статичны, исходя из этого подумайте насчет модели хранения в кэше и в PostgreSQL.
Задействовал тип данных JSONB в PostgreSQL и []byte в кэше.

# Подумайте как избежать проблем, связанных с тем, что в канал могут закинуть что-угодно
сделал json.Unmarshal одного поля order_id и по нему делал решение - корректные данные пришли в канал или мусор.

# Чтобы проверить работает ли подписка онлайн, сделайте себе отдельный скрипт, для публикации данных в канал
написал отдельный скрипт в директории cmd/publisher для отправки json'а в канал nats-streaming

# Подумайте как не терять данные в случае ошибок или проблем с сервисом
указал опцию DurableName в подписке на канал, которая позволяет получить пропущенные сообщения при пере-подключении к серверу.

# Nats-streaming разверните локально (не путать с Nats)
развернул локально nats и PostgreSQL через docker-compose.

# Флаги командной строки
по умолчанию, веб-сервер запускается на адресе localhost:8080 
можно указать другой адрес через флаг -a

# Запуск программы
```bash
docker-compose up --build 
```