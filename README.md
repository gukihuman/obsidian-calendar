
### API

Основной адрес API
```
v4/accounting-calendar
```

Параметры API
```
startDate = "2024-04-29"

endDate = "2024-06-02"
```

Response - массив обьектов Day
```ts
type Response = Day[]
```

Day
```js
{
  date: "2024-04-29",
  isDayOff: false,
  events: [
    {
      title: "Отчет о движении средств",
      description: "Уплата авансового платежа по налогу на..."
    }
  ]
}
```

Предлагаю хранить на фронте весь массив выгруженных дней. При переключении периодов будет проверяться не вышли ли мы за пределы имеющихся данных. Если вышли - делаем запрос и подгружаем еще. Можно подгружать с запасом, например соседние месяцы тоже.

---
### Структура компонентов

Предлагаю делать не в Shared как в [старой структуре](old_structure.md), a в Pages / Tool / AccountingCalendar. Календарь вроде достаточно специализирован, не вижу где он еще в подобном виде используется или может быть использован.

```
AccountingCalendar
	
```


	 AccountingCalendar


### Заметки

Есть состояние ограничивающее перелистывание вперед. Предлагаю ограничивать следующим годом.

<img src="assets/max_date.jpg" width="300">

![](settings_layer_one.png)