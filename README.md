
[Вариант Николая](nikolai.md)

---

![[Illustration.jpg|600]]

<img src="assets/Illustration.jpg" width="600">

Эвент принадлежит всегда только одному слою. Эвент также может принадлежать группам, если они есть, но никогда к категориям напрямую. Категории только обьединяют группы по теме.

![[layers_groups.png|600]]

<img src="assets/layers_groups.png" width="600">

![[layers_cats_groups.png|700]]

<img src="assets/layers_cats_groups.png" width="700">

---
#### Начальный запрос справочников

```
v4/calendar-info 
```

```ts
export type Info = {
  id: number
  title: string
}
export interface LayersInfo extends Info {
  groupsIds?: number[]
  catIds?: number[]
}
export interface CategoryInfo extends Info {
  groupsIds?: number[]
}
export type ResponseInfo = {
  layers: LayersInfo[]
  categories: CategoryInfo[]
  groups: Info[]
}
const responseInfo: ResponseInfo = {
  layers: [
    { id: 1, title: 'Выходные' },
    { id: 2, title: 'Мероприятия Клерк', groupsIds: [1, 2, 3] },
    { id: 3, title: 'Персональные события' },
    { id: 4, title: 'Отчетность', catIds: [1, 2, 3] },
  ],
  categories: [
    { id: 1, title: 'Виды отчетности', groupsIds: [4, 5, 6, 7, 8] },
    {
      id: 2,
      title: 'Контролирующий орган',
      groupsIds: [9, 10, 11, 12, 13, 14],
    },
    { id: 3, title: 'Система налогооблажения', groupsIds: [15, 16, 17, 18] },
  ],
  groups: [
    { id: 1, title: 'Вебинары' },
    { id: 2, title: 'Онлайн-курсы' },
    { id: 3, title: 'Курсы повышения квалификации' },
    { id: 4, title: 'Бухгалтерская и налоговая' },
    { id: 5, title: 'По сотрудникам' },
    { id: 6, title: 'Статистическая' },
    { id: 7, title: 'Экологическая' },
    { id: 8, title: 'Алкогольная' },
    { id: 9, title: 'ПФР' },
    { id: 10, title: 'РПН' },
    { id: 11, title: 'ФНС' },
    { id: 12, title: 'ФСС' },
    { id: 13, title: 'ФСРАР' },
    { id: 14, title: 'СФР' },
    { id: 15, title: 'ОСНО' },
    { id: 16, title: 'УСН' },
    { id: 17, title: 'ПСН' },
    { id: 18, title: 'ЕСХН' },
  ],
}
```

При добавлении новых слоев, групп и категорий лучше вегда использовать новые id'шники, даже если есть пропущенные числа после удаления старых, т.к. состояние выбранных слоев может быть сохранено у пользователя в ссылке

---
#### Последующий запрос эвентов

```
v4/calendar-events
```

```
startDate = 2024-04-29
endDate = 2024-06-02
layers = 1,2,3
groups = 1,2,3,4,5
```

```
v4/calendar-events?start=2024-04-29&end=2024-06-02&layers=1,2,3,4&groups=1,2,3,4,5
```

```ts
export type CalendarEvents = {
  layerId: number // хоть и фильтруем уже в запросе, id слоя нужен для цвета
  groupIds?: number[] // нужно для отображения группы в карточке события - "УСН"
  date: string // тип Date, но фетчится как string
  title?: string
  description?: string
}
const responseEvents: CalendarEvents[] = [
  { layerId: 1, date: '2024-06-15' },
  { layerId: 1, date: '2024-06-16' },
  { layerId: 1, date: '2024-06-22' },
  { layerId: 1, date: '2024-06-23' },
  {
    layerId: 2,
    groupIds: [1],
    date: '2024-06-10',
    title: 'Название вебинара',
    description: 'Описание вебинара',
  },
  {
    layerId: 2,
    groupIds: [3],
    date: '2024-06-12',
    title: 'Название курса повышения квалификации',
    description: 'Описание курса повышения квалификации',
  },
  {
    layerId: 3,
    date: '2024-06-15',
    title: 'Название персонального события',
    description: 'Описание персонального события',
  },
  {
    layerId: 4,
    groupIds: [5, 9, 11],
    date: '2024-06-07',
    title: 'Название отчета',
    description: 'Относится ко отчетности по сотрудникам, орган ПФР и ФНС',
  },
]
```

Фильтруем эвенты на бэке по layerId и groupIds, указанным в параметрах API. Если указан слой без групп - возвращаем все эвенты слоя (например, выходные), в остальных случаях только эвенты указанных групп

---

Основной компонент: [AccountingCalendar](AccountingCalendar/AccountingCalendar.md) *(дерево компонентов в сайдбаре навигации)*

id и цвет слоя, задаем на фронте по макету
```ts
const colorLayerMap = {
  default: 'grey-500', // если не нашли id или не указан
  1: 'red-500',
  2: 'green-500',
  3: 'blue-500',
  4: 'yellow-500',
}
```


---

- [Дорожная карта](road_map.md)

- [Вопросы](questions.md)

- [Старая структура](old_structure.md)

- [Старое ридми 01](old_readme_01.md)
