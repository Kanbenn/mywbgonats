
## WB L0

Здравствуйте. Спасибо, что нашли время на код-ревью 🙏
Очень интересный и необычный проект - большая благодарность методистам.
Краткие пояснения к коду по заданию:

`Данные статичны, исходя из этого подумайте насчет модели хранения в кэше и в PostgreSQL.`
Задействовал тип данных JSONB в PostgreSQL и []byte в кэше для производительности [postgres.go](https://github.com/Kanbenn/mywbgonats/blob/main/internal/storage/postgres.go#L17)

`Подумайте как избежать проблем, связанных с тем, что в канал могут закинуть что-угодно`
сделал json.Unmarshal поля order_id и по нему if - корректные данные пришли в канал или мусор.

`Чтобы проверить работает ли подписка онлайн, сделайте себе отдельный скрипт, для публикации данных в канал`
написал отдельный скрипт [publisher](https://github.com/Kanbenn/mywbgonats/blob/main/cmd/publisher/publisher.go#L11), с возможностью отправки отдельных json-файлов через флаг командной строки -j. 

`Подумайте как не терять данные в случае ошибок или проблем с сервисом`
указал опцию DurableName в подписке на канал, которая позволяет получить пропущенные сообщения при пере-подключении к серверу [subscriber.go](https://github.com/Kanbenn/mywbgonats/blob/main/internal/subscriber/subscriber.go#L33)

`Nats-streaming разверните локально (не путать с Nats)`
развернул локально nats и PostgreSQL через [docker-compose](https://github.com/Kanbenn/mywbgonats/blob/main/docker-compose.yaml)

`Покройте сервис автотестами`
написал юнит-тесты для кэша [cache_test.go](https://github.com/Kanbenn/mywbgonats/blob/main/internal/storage/cache_test.go)

`Устройте вашему сервису стресс-тест: выясните на что он способен. Воспользуйтесь утилитами WRK и Vegeta`
TODO

`Флаги командной строки`
по умолчанию, веб-сервер запускается на адресе `localhost:8080` другой адрес указать через флаг -a [config.go](https://github.com/Kanbenn/mywbgonats/blob/main/internal/config/config.go)

`кэш с поддержкой безопасного асинхронного доступа`
через мьютексы и closure-функции [withRLock](https://github.com/Kanbenn/mywbgonats/blob/main/internal/storage/cache.go#L61)

`Локальный запуск Nats и PostgreSQL в Docker`
```bash
docker-compose up --build 
```