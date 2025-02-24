# Gmail


## 1. Тема и целевая аудитория

**Gmail** - почтовый сервис компании Google, позволяющий пользователям отправлять и получать электронные письма и файлы.

### Функционал MVP

1. Отправка письма.
2. Получение письма.
3. Просмотр папки.
4. Прикрепление файла к письму.
5. Создание черновика.
6. Удаление письма.

### Целевая аудитория

- всего пользователей - 2.1 млрд;
- всего посещений - около 2 млрд активных пользователей в месяц;
- пользователь получает в среднем 20 писем в день;
- 68% писем помечается как спам;
- 22% полученных писем открываются менее, чем через час после получения;
- 50% полученных писем удаляется
- 75% пользователей заходят с мобильных устройств;
- стредняя продолжительность посещения - 00:11:58;
- географическое распределение трафика:
  
  ![image](https://github.com/user-attachments/assets/61c3984f-00cf-48e1-ae4b-3a9fc615cb70)

- демографические показатели:
  
  ![image](https://github.com/user-attachments/assets/1f7420f2-615a-4178-880c-7aae0f6e935e)

## 2. Расчет нагрузки

### Продуктовые метрики

| Метрика | Значение |
| --- | ----------- |
| MAU | 2B |
| DAU | 660M |
| Хранилище пользователя | 15GB |
| Средняя загрузка хранилища | 5GB |
| Максимальный размер письма | 25MB |
| Средний размер письма | 75KB |
| Максимальное количество писем пользователя в день | 500 |
| Всего писем в день | 121B |
| Писем пользователя в день | 20 |
| Просмотр почты в день | 2.3 |
| Создание черновика в день | 20 |
| удаление писем в день | 10 |

### Технические метрики

 Общий размер хранения:
 Максимальный - 2.1B * 15GB = 31500 PB
 Средний - 2.1B * 5GB = 10500 PB

**Сетевой трафик**

| Запрос | Объем на 1 запрос | Количество в день | RPS | Суточный трафик |
| --- | ----------- | ---- | ---- | ---- |
| Отправка письма | 25MB | 121B | 1.4M | 3025PB |
| Просмотр папки | 50KB | 1518M | 17.5K | 75.9MB |
| Создание черновика | 25MB | 13.2B | 152.7K | 330PB |
| Удаление писем | 50KB | 6.6B | 76.4K | 330MB |

Итого - 3355PB в день = 38831 ГБит/сек

## Список источников

- https://www.mailmodo.com/guides/email-marketing-statistics/
- https://www.dragapp.com/blog/gmail-stats/
- https://usesignhouse.com/blog/gmail-stats/
- https://top.mail.ru/
- https://inclient.ru/email-stats/
- https://www.demandsage.com/gmail-statistics/
- https://mailmeteor.com/tools/email-size
