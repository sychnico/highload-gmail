# Gmail


## 1. Тема и целевая аудитория

**Gmail** - почтовый сервис компании Google, позволяющий пользователям отправлять и получать электронные письма и файлы.

### Функционал MVP

1. Отправка письма.
2. Получение письма.
3. Чтение письма.
4. Просмотр папки.
5. Прикрепление файла к письму.
6. Создание черновика.
7. Удаление письма.
8. Скачать файл.

### Целевая аудитория

- всего пользователей - 2.5 млрд[4];
- всего посещений - около 2 млрд активных пользователей в месяц[3];
- пользователь получает в среднем 40 писем в день[17];
- 68% писем помечается как спам[2];
- 22% полученных писем открываются менее, чем через час после получения[2];
- 50% полученных писем удаляется[7];
- 75% пользователей заходят с мобильных устройств[2][4][5];
- стредняя продолжительность посещения - 00:11:58[2];
- суммарное посещение в день - 28 минут[2]
- географическое распределение трафика[11]:
  
  ![image](https://github.com/user-attachments/assets/61c3984f-00cf-48e1-ae4b-3a9fc615cb70)

- демографические показатели[11]:
  
  ![image](https://github.com/user-attachments/assets/1f7420f2-615a-4178-880c-7aae0f6e935e)

## 2. Расчет нагрузки

### Продуктовые метрики

| Метрика | Значение |
| --- | ----------- |
| MAU | 2B[3] |
| DAU | 661M[19] |
| Хранилище пользователя | 15GB[15] |
| Средняя загрузка хранилища | 17000[16]*177KB = 3GB |
| Максимальный размер письма | 25MB[14] |
| Средний размер письма без вложений | 75KB[8] |
| Средний размер письма с вложениями | 500KB[9] |
| Количество писем с вложениями | 24%[9] |
| Количество просмотренных вложений | 6%[10] |
| Средний размер письма | 0.24 * 500KB[9] + 0.76 * 75KB[8] = 177KB |
| Максимальное количество писем пользователя в день | 2000[13] |
| Всего писем в день | 121B[3] |
| Писем от пользователей | 661M*40[17] = 26.4B |
| Автоматических писем | 121B - 26.4B = 94.6B |
| Писем пользователя в день | 40[17] |
| Просмотр почты в день | 28[2] / 12[2] = 2.3 |
| Создание черновика в день | 40[17] |
| удаление писем в день | 40[17]*0.5[7] = 20 |

### Технические метрики

 Общий размер хранения:
 
 2.5B * 3GB = 7500 PB

**Сетевой трафик**

| Запрос | Объем на 1 запрос | Количество в день | RPS | Суточный трафик |
| --- | ----------- | ---- | ---- | ---- |
| Отправка письма пользователем | 177KB | 26.4B | 305.5K | 4.7PB |
| Отправка письма ботом | 177KB | 94.6B | 1.1M | 16.7PB |
| Просмотр папки | 50KB | 661M * 2.3 = 1.5B | 17.5K | 75.9TB |
| Просмотр письма | 75KB | 121B | 1.4M | 9.1PB |
| Скачивание файла | 425KB | 1.7B | 19.7K | 722.5TB |
| Загрузка файла | 425KB | 29B | 336K | 12.3PB |
| Создание черновика | 75KB | 26.4B | 305.5K | 1980TB |
| Удаление писем | 50KB | 6.6B | 76.4K | 330TB |

Итого:

Суточный трафик - 45.9PB в день = 531.3 ГБит/сек

RPS - 3.5M

## 3. Глобальная балансировка нагрузки

### Функциональное разбиение по доменам

Основной домен - mail.goolge.com

Домен для загрузки вложений - mail-attachment.googleusercontent.com

### Расположение датацентров

Так как почтовый сервис не требует максимально низких задержек, то можно будет ограничитться единственным датацентром.

Располагаться он будет в Нью-Йорке(США) Такое расположение выбрано исходя из географического распределения трафика[11], а также карты подводных кабелей[12].

## 4. Локальная балансировка нагрузки

### Балансировка входящих запросов

Для балансировки входящих забросов будут использоваться балансировщики L7 - Nginx и DNS для service discovery. Nginx должен раз в 1 секунду обновлять DNS на сдучай выхода из строя одного из серверов.

### Балансировка межсервисных запросов

Для балансировки межсервисных запросов будет использоваться схема с двумя видами балансировщиков - для внешнего и для внутреннего трафика.

### Схема отказоустойчивости

Для обеспечения отказоустойчивости будет использоваться VRRP. Балансировщики ставятся попарно, когда один падает, другой из пары встает на его место с тем же IP.

### Нагрузка по терминации SSL

Для терминации SSL при рассчитанном RPS понадобятся 3.5M / 10K[20] = 350 серверов, х2 для VRRP получим 700 серверов.

## 5. Логическая схема БД

![image](https://github.com/user-attachments/assets/bb48971e-6f77-4d9d-b459-200400f94a54)

### Описание таблиц

**Таблица User**

| Поле | Тип данных | Размер |
| --- | ----------- | ---- |
| id | int | 4B |
| email | text | 256B |
| username | text | 256B |
| password | text | 256B |
| avatar_url | text | 256B |
| created_at | timestamp | 8B |
| updated_at | timestamp | 8B |

**Таблица Message**

| Поле | Тип данных | Размер |
| --- | ----------- | ---- |
| id | int | 4B |
| subject | text | 256B |
| body | text | 75KB |
| created_at | timestamp | 8B |
| updated_at | timestamp | 8B |

**Таблица Email_transaction**

| Поле | Тип данных | Размер |
| --- | ----------- | ---- |
| id | int | 4B |
| parent_transaction_id | int | 4B |
| sender_email | text | 256B |
| recipient_email | text | 256B |
| message_id | int | 4B |
| date | timestamp | 8B |
| is_read | bool | 1B |
| is_cc | bool | 1B |
| updated_at | timestamp | 8B |

Здесь поле is_cc обозначает является ли письмо копией. Если письмо не является копией или является скрытой копией (bcc), это поле равно false в списке получателей пользователь видит только себя (recipient_email). Иначе - необходимо выполнить sql-запрос для поиска всех получателей. 

Если бы поле называлось is_bcc было бы невозможно отличить явные копии от не скопированных писем (в обоих случаях false), что вынуждало бы выполнять вышеупомянутый sql-запрос даже когда получатель один, неоправданно увеличивая нагрузку на базу данных.

**Таблица Attachment**

| Поле | Тип данных | Размер |
| --- | ----------- | ---- |
| id | int | 4B |
| message_id | int | 4B |
| url | text | 256B |

Также каждой записи в таблице Attachment соответствует файл вложения. Средний размер вложения - 425KB.

**Таблица Label**

| Поле | Тип данных | Размер |
| --- | ----------- | ---- |
| id | int | 4B |
| user_id | int | 4B |
| name | text | 256B |
| created_at | timestamp | 8B |
| updated_at | timestamp | 8B |

### Требования к конситентности

- Уникальность ключей: все первичные и внешние ключи в таблицах должны быть уникальными
- Обновление временных меток (created_at, updated_at)
- Проверка целостности данных: все внешние ключи ссылаются на существующие записи в связанных таблицах.

### Распределение нагрузки по ключам

| Таблица | RPS на чтение | RPS на запись |
| --- | ----------- | ---- |
| Message | 1.4M | 1.4M |
| Email_transaction | 1.4M | 2.8M |
| Attachment | 19.7K | 336K |

## 6. Физическая схема БД

![image](https://github.com/user-attachments/assets/8ea5dbd8-27ca-4de6-99f2-26e6a22333e0)

### Индексы

**Таблица User**

- idx_user_email(email) - индекс для быстрой проверки авторизации

**Таблица Email_transaction**

- idx_email_recipient(recipient_email, date) - индекс для быстрой загрузки входящих
- idx_email_sender(sender_email, date) - индекс для поиска писем по отправителю

**Таблица Email_labels**

- idx_transaction_id(email_transaction_id) - индекс для быстрой загрузки меток письма
- idx_transaction_user(user_id, date) - индекс для быстрой загрузки меток пользователя

**Таблица Attachment**

- idx_message_id(message_id) - индексдля быстрой загрузки вложений

### Денормализация

**Таблица User**

- Поле unread_count для быстрого доступа к количеству непрочитанных писем

**Таблица Email_transaction**

- Поля sender_username, recipient_username для быстрого отображения отправителя и получателя.
- Поле has_attachments для избегания лишнего поиска.
- Поле owner_id для избегания лишних join.

### Выбор СУБД, шардирование и резервирование

| Таблица | СУБД | Шардирование | Резервирование |
| --- | ----------- | ---- | --- |
| User | PostgreSQL | Шардирование по user.id | Репликация master-slave, 2 реплики |
| Email_transaction | PostgreSQL | Шардирование по owner_id | Репликация master-slave, 2 реплики |
| Email_labes | PostgreSQL | Шардирование по user_id | Репликация master-slave, 2 реплики |
| Attachment | PostgreSQL | Шардирование по message_id | Репликация master-slave, 2 реплики |
| File | S3 |  | Резервирование средствами S3 |

### Клиентские библиотеки, интеграции

- lib/pq - библиотека для работы с Postgresql на языке go.
- aws/aws-sdk-go/service/s3 - библиотека для работы с S3 на языке go.
- Cassandra для кэширования

## 7. Алгоритмы

Основным сложным и неочевидным алгоритмом в почтовом сервисе является алгоритм получения писем в метке пользователя, отсортированных по времени. Для решения этой задачи был создан вышеупомянутый индекс idx_email_recipient. Алгоритм выглядит следующим образом:

    SELECT t.id, t.sender_email, t.recipient_email, t.subject, t.body, t.sending_date, t.isread
    FROM email_transaction AS tr
    JOIN email_labels AS lb ON t.id = lb.email_transaction_id
    WHERE lb.name = $2
    AND t.owner_id = $1
    ORDER BY t.date DESC

## 8. Технологии

| Технология | Область применения | Мотивация |
| --- | ----------- | ---- |
| Golang | Backend | Многопоточность, производительность |
| TypeScript | Frontend | Статическая типизация, безопасность |
| React | Frontend | Кроссплатформенность, VDOM |
| Vite | Frontend | Скорость |
| PostgreSQL | База данных | Шардирование, транзакции |
| Cassandra | NoSQL БД | Скорость, Горизонтальное масштабирование |
| Amazon S3 | Хранилище файлов | Резервирование |
| nginx | балансировка трафика | Хорошие показатели эффективности при больших RPS, терминация SSL |
| Kubernetes | Оркестрация | Масштабирование, отказоустойчивость |

## Список источников

1. https://www.mailmodo.com/guides/email-marketing-statistics/
2. https://www.dragapp.com/blog/gmail-stats/
3. https://usesignhouse.com/blog/gmail-stats/
4. https://clean.email/blog/email-providers/gmail-statistics
5. https://top.mail.ru/
6. https://inclient.ru/email-stats/
7. https://www.demandsage.com/gmail-statistics/
8. https://mailmeteor.com/tools/email-size
9. https://www.radicati.com/wp/wp-content/uploads/2009/05/email-stats-report-exec-summary.pdf
10. https://www.mxhero.com/post/save-the-planet-replace-email-attachments-with-file-share-links
11. https://www.similarweb.com/ru/website/gmail.com/
12. https://submarine-cable-map-2023.telegeography.com/
13. https://support.google.com/a/answer/166852?hl=en
14. https://support.google.com/mail/answer/6584?hl=en
15. https://support.google.com/mail/answer/9312312?hl=en
16. https://techjury.net/blog/gmail-statistics/
17. https://prosperitymedia.com.au/how-many-emails-are-sent-per-day-in-2025/
18. https://corp.vkcdn.ru/media/files/RUS_Press_Release_9M_2024.pdf
19. Для расчета DAU использовались данные аналогичного сервиса - почты mail.ru[18]. Для этого MAU почты gmail было умножено на отношение DAU к MAU почты mail.ru (Допускаем что это отношение будет приблизительно одинаковым для всех почтовых сервисов): 2B[3] * 16.2M[18] / 49M[18] = 661M.
20. https://blog.nginx.org/blog/testing-the-performance-of-nginx-and-nginx-plus-web-servers
