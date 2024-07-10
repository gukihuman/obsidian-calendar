#### Начальный запрос справочников

```
v4/calendar-info 
```

```ts
export type Info = {
  id: number
  name: string
}
export interface GroupInfo extends Info {
  layerId: number
  category: string // по дефолту пустая строка
}
export type ResponseInfo = {
  layers: Info[]
  groups: GroupInfo[]
}
const responseInfo: ResponseInfo = {
  layers: [
    { id: 1, name: 'Выходные' },
    { id: 2, name: 'Мероприятия Клерк' },
    { id: 3, name: 'Персональные события' },
    { id: 4, name: 'Отчетность' },
  ],
  groups: [
    { id: 1, name: 'Вебинары', layerId: 2, category: '' },
    { id: 2, name: 'Онлайн-курсы', layerId: 2, category: '' },
    { id: 3, name: 'Курсы повышения квалификации', layerId: 2, category: '' },
    {
      id: 4,
      name: 'Бухгалтерская и налоговая',
      layerId: 4,
      category: 'Виды отчетности',
    },
    { id: 5, name: 'По сотрудникам', layerId: 4, category: 'Виды отчетности' },
    { id: 6, name: 'Статистическая', layerId: 4, category: 'Виды отчетности' },
    { id: 7, name: 'Экологическая', layerId: 4, category: 'Виды отчетности' },
    { id: 8, name: 'Алкогольная', layerId: 4, category: 'Виды отчетности' },
    { id: 9, name: 'ПФР', layerId: 4, category: 'Контролирующий орган' },
    { id: 10, name: 'РПН', layerId: 4, category: 'Контролирующий орган' },
    { id: 11, name: 'ФНС', layerId: 4, category: 'Контролирующий орган' },
    { id: 12, name: 'ФСС', layerId: 4, category: 'Контролирующий орган' },
    { id: 13, name: 'ФСРАР', layerId: 4, category: 'Контролирующий орган' },
    { id: 14, name: 'СФР', layerId: 4, category: 'Контролирующий орган' },
    { id: 15, name: 'ОСНО', layerId: 4, category: 'Система налогооблажения' },
    { id: 16, name: 'УСН', layerId: 4, category: 'Система налогооблажения' },
    { id: 17, name: 'ПСН', layerId: 4, category: 'Система налогооблажения' },
    { id: 18, name: 'ЕСХН', layerId: 4, category: 'Система налогооблажения' },
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
start = 2024-04-29
end = 2024-06-02
layerIds = 1,2,3 // необязательный параметр
groupIds = 1,2,3,4,5 // необязательный параметр
```

При отсутствии параметров фильтрации присылаются все эвенты в указанном диапазоне дат.

```
v4/calendar-events?start=2024-04-29&end=2024-06-02&layerIds=1,2,3&groupIds=1,2,3
```

```ts
export type ParentIds = {
  // хоть и фильтруем уже в запросе, id слоя нужен для цвета
  layerId: number
  // нужно для отображения группы в карточке события - "УСН"
  groupIds: number[] // может быть пустым
}
export type CalendarEvents = {
  date: string // тип Date, но фетчится как string
  name: string
  description: string
  parentIds: ParentIds
}
const responseEvents: CalendarEvents[] = [
  // Выходные
  {
    date: '2024-06-15',
    name: '',
    description: '',
    parentIds: { layerId: 1, groupIds: [] },
  },
  {
    date: '2024-06-16',
    name: '',
    description: '',
    parentIds: { layerId: 1, groupIds: [] },
  },
  {
    date: '2024-06-22',
    name: '',
    description: '',
    parentIds: { layerId: 1, groupIds: [] },
  },
  {
    date: '2024-06-23',
    name: '',
    description: '',
    parentIds: { layerId: 1, groupIds: [] },
  },
  // Мероприятия Клерк
  {
    date: '2024-06-10',
    name: 'Название вебинара',
    description: 'Описание вебинара',
    parentIds: { layerId: 2, groupIds: [1] },
  },
  {
    date: '2024-06-12',
    name: 'Название курса повышения квалификации',
    description: 'Описание курса повышения квалификации',
    parentIds: { layerId: 2, groupIds: [3] },
  },
  // Персональные события
  {
    date: '2024-06-15',
    name: 'Название персонального события',
    description: 'Описание персонального события',
    parentIds: { layerId: 3, groupIds: [] },
  },
  // Отчеты
  {
    date: '2024-06-07',
    name: 'Название отчета',
    description: 'Относится ко отчетности по сотрудникам, орган ПФР и ФНС',
    parentIds: { layerId: 4, groupIds: [5, 9, 11] },
  },
]
```

Совпадение по группам должно быть строгим, эвент присылается, только если все группы эвента указаны в запросе. А вот в случае, если эвент не принадлежит ни к одной группе ( пустой groupIds: [] ) - достаточно совпадения по слою, эвент присылается.

---

![[excali_scheme_02.png|600]]

<img src="Excalidraw/excali_scheme_02.png" width="600">

![[layers_groups.png|600]]

<img src="assets/layers_groups.png" width="600">

![[layers_cats_groups.png|700]]

<img src="assets/layers_cats_groups.png" width="700">

Категории могут показаться родительской сущностью по отношению к группам, но на самом деле они лишь собирают группы вместе по теме. Категория - характеристика группы.

![[Illustration.jpg|600]]

<img src="assets/Illustration.jpg" width="600">

Эвент принадлежит всегда только одному слою. Эвент также может принадлежать группам, если они есть, но никогда к категориям напрямую. Категории только обьединяют группы по теме.

---
Основной компонент: [AccountingCalendar](AccountingCalendar/AccountingCalendar.md) *(дерево компонентов в сайдбаре навигации)*

id и цвет слоя, задаем на фронте по макету
```ts
const colorLayerMap = {
  0: 'grey-500', // дефолтное значение
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

* [Вариант Николая](nikolai.md)
