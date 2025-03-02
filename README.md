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

- всего пользователей - 2.1 млрд;
- всего посещений - около 1.8 млрд активных пользователей в месяц;
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
| MAU | 1.8B |
| DAU | 600M |
| Хранилище пользователя | 15GB |
| Средняя загрузка хранилища | 5GB |
| Максимальный размер письма | 25MB |
| Средний размер письма без вложений | 75KB |
| Средний размер письма с вложениями | 500KB |
| Количество писем с вложениями | 24% |
| Количество просмотренных вложений | 6% |
| Средний размер письма | 0.24 * 500KB + 0.76 * 75KB = 177KB |
| Максимальное количество писем пользователя в день | 500 |
| Всего писем в день | 121B |
| Писем пользователя в день | 20 |
| Просмотр почты в день | 2.3 |
| Создание черновика в день | 20 |
| удаление писем в день | 10 |

### Технические метрики

 Общий размер хранения:
 
 2.1B * 5GB = 10500 PB

**Сетевой трафик**

| Запрос | Объем на 1 запрос | Количество в день | RPS | Суточный трафик |
| --- | ----------- | ---- | ---- | ---- |
| Отправка письма | 177KB | 121B | 1.4M | 21.4PB |
| Просмотр папки | 50KB | 1518M | 17.5K | 75.9MB |
| Просмотр письма | 75KB | 121B | 1.4M | 9.1PB |
| Скачивание файла | 425KB | 1.7B | 19.7K | 722.5TB |
| Загрузка файла | 425KB | 29B | 336K | 12.3PB |
| Создание черновика | 75KB | 13.2B | 152.7K | 990TB |
| Удаление писем | 50KB | 6.6B | 76.4K | 330MB |

Итого:

Суточный трафик - 44.5PB в день = 512 ГБит/сек

## 3. Глобальная балансировка нагрузки

### Расположение датацентров

**Северная Америка**

- Лос-Анджелес(США)
- Нью-Йорк(США)

**Южная Америка**

- Сан-Паулу(Бразилия)

**Европа**

- Хельсинки(Финляндия)
- Марсель(Франция)

**Азия**

- Сингапур
- Мумбай(Индия)

## Список источников

1. https://www.mailmodo.com/guides/email-marketing-statistics/
2. https://www.dragapp.com/blog/gmail-stats/
3. https://usesignhouse.com/blog/gmail-stats/
4. https://top.mail.ru/
5. https://inclient.ru/email-stats/
6. https://www.demandsage.com/gmail-statistics/
7. https://mailmeteor.com/tools/email-size
8. https://www.radicati.com/wp/wp-content/uploads/2009/05/email-stats-report-exec-summary.pdf
9. https://www.mxhero.com/post/save-the-planet-replace-email-attachments-with-file-share-links
10. https://www.similarweb.com/ru/website/gmail.com/
11. https://submarine-cable-map-2023.telegeography.com/
