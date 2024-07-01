
[вопросы](questions.md)

[старая структура](old_structure.md)


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

Предлагаю хранить на фронте весь массив выгруженных дней. При переключении периодов будет проверяться не вышли ли мы за пределы имеющихся данных. Если вышли - делаем запрос и подгружаем еще. Можно подгружать с запасом, например соседние месяцы тоже.

---

```ts
type ReportCategories = 'Бухгалтерская и налоговая' | 'По сотрудникам' | 'Статистическая' | 'Экологическая' | 'Алкогольная'
type controllingAuthority = 'ПФР' | 'РПН' | 'Росстат' | 'ФНС' | 'ФСС' | 'ФСРАР' | 'СФР'
type taxSystem = 'ОСНО' | 'УСН' | 'ПСН' | 'ЕСХН'
type mainCategory = 'buh' | 'klerk' | 'personal'
```

---

По структуре компонентов предлагаю делать не в Shared как в [старой структуре](old_structure.md), a в Pages / Tool / AccountingCalendar. Календарь вроде достаточно специализирован, не вижу где он еще в подобном виде используется или может быть использован.

---
#### Дорожная карта втреч

1:
- формат данных по апи
- структура компонентов

2:
- фетчинг данных
- прокидывание данных по компонентам / пропсы компонентов