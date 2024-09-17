#### Запрос информации о слоях и фильтрах

```
v4/calendar/info 
```

```ts
{
  layers: {
    1: 'Выходные',
    2: 'Вебинары',
    4: 'Отчетность',
    5: 'Уплата налогов'
  },
  filters: [
    { id: 5, name: 'По сотрудникам', layerIds: [4, 5], group: 'Виды отчетности' },
    { id: 6, name: 'Статистическая', layerIds: [4, 5], group: 'Виды отчетности' },
    { id: 7, name: 'Экологическая', layerIds: 4, group: 'Виды отчетности' },
    { id: 9, name: 'ПФР', layerIds: 4, group: 'Контролирующий орган' },
    { id: 10, name: 'РПН', layerIds: 4, group: 'Контролирующий орган' },
    { id: 14, name: 'СФР', layerIds: [4, 5], group: 'Контролирующий орган' },
    { id: 17, name: 'ПСН', layerIds: [4, 5], group: 'Система налогооблажения' },
    { id: 18, name: 'ЕСХН', layerIds: 4, group: 'Система налогооблажения' },
  ],
}
```

При добавлении новых слоев и фильтров лучше вегда использовать новые id'шники, даже если есть пропущенные числа после удаления старых, т.к. состояние выбранных слоев может быть сохранено у пользователя в ссылке

---
#### Запрос эвентов

```
v4/calendar/events
```

```
start = 2024-04-29
end = 2024-06-02
sendAll       // все эвенты в диапазоне дат
1             // слой без значения, если y слоя есть фильтры, то выбраны все
2
4 
5 = 6,14,17   // выбраны 6, 14 и 17 фильтр
```

```
v4/calendar/events?start=2024-04-29&end=2024-06-02&1&2&4&5=6,14
```

```ts
[
  // Выходные
  {
    date: '2024-06-15',
    endDate: '',
    name: '',
    description: '',
    url: '',
    parentIds: { layerId: 1, filterIds: [] },
    attachments: []
  },
  {
    date: '2024-06-16',
    endDate: '',
    name: '',
    description: '',
    url: '',
    parentIds: { layerId: 1, filterIds: [] },
    attachments: []
  },
  // Вебинары
  {
    date: '2024-07-10',
    endDate: '',
    name: 'Вебинар',
    description: 'Описание вебинара',
    url: '/learn/event/50537/',
    parentIds: { layerId: 2, filterIds: [] },
    attachments: [
       { title: 'Вебинар Все про УСН', url: '/link/' },
       { title: 'Название документа', url: '/link/' }
    ]
  },
  // Oтчетность
  {
    date: '2024-06-07',
    endDate: '2024-06-15',
    name: 'Название отчета',
    description: 'Относится ко отчетности по сотрудникам, орган ПФР и СФР',
    url: '',
    parentIds: { layerId: 4, filterIds: [5, 9, 14] },
    attachments: []
  },
  // Уплата налогов
  {
    date: '2024-06-07',
    endDate: '',
    name: 'Название уплаты налогов',
    description: 'Относится к СФР',
    url: '',
    parentIds: { layerId: 4, filterIds: [14] },
    attachments: []
  }
]
```

Если эвент не относится ни к одному фильтру ( пустой filterIds: [] ) - достаточно совпадения по слою - эвент присылается

---

![[layers_filters.png|500]]

<img src="assets/layers_filters.png" width="600">

![[layers_groups_filters.png|700]]

<img src="assets/layers_groups_filters.png" width="700">

Группы могут показаться родительской сущностью по отношению к фильтрам, но на самом деле они лишь собирают фильтры вместе по теме для отображения по столбцам. Группа - характеристика фильтра

---

- [Дорожная карта](road_map.md)

- [Вопросы](questions.md)

- [Старая структура](old_structure.md)

- [Старое ридми 01](old_readme_01.md)

* [Вариант Николая](nikolai.md)
