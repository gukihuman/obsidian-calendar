
[вопросы](questions.md)

[старая структура](old_structure.md)

Основной компонент: [AccountingCalendar](AccountingCalendar/AccountingCalendar.md) *(дерево компонентов в сайдбаре навигации)*

![[rules.png|550]]

<img src="assets/rules.png" width="550">

---
#### API

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
  isDayOff: false, // выходные и праздники
  events: [
    {
      title: "Отчет о движении средств",
      description: "Уплата авансового платежа по налогу на...",
      mainCategory: 'buh', // задается в админке специально для календаря
      reportCategories: ['По сотрудникам', 'Алкогольная'], // виды отчетности
      controllingAuthority: [''], // контролирующий орган
      taxSystem: ['ПФР'], // система налогооблажения
    },
    {
      title: "Отчет о движении средств",
      description: "Уплата авансового платежа по налогу на...",
      mainCategory: 'klerk' // мероприятия Клерк
    },
    {
      title: "Отчет о движении средств",
      description: "Уплата авансового платежа по налогу на...",
      mainCategory: 'personal' // персональные события
    }
  ],
}
```

---

```ts
type ReportCategories = 'Бухгалтерская и налоговая' | 'По сотрудникам' | 'Статистическая' | 'Экологическая' | 'Алкогольная'
type controllingAuthority = 'ПФР' | 'РПН' | 'Росстат' | 'ФНС' | 'ФСС' | 'ФСРАР' | 'СФР'
type taxSystem = 'ОСНО' | 'УСН' | 'ПСН' | 'ЕСХН'
type mainCategory = 'buh' | 'klerk' | 'personal'
```

---
#### Дорожная карта втреч

1:
- формат данных по апи
- структура компонентов

2:
- фетчинг / обработка данных
- прокидывание данных по компонентам / пропсы компонентов