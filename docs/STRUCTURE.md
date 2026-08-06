# Как устроен проект

```
Generator/
├── generator/index.html      прототип генератора        → /generator/
├── onboarding/index.html     прототип онбординга        → /onboarding/
├── shared/generator/         механика генератора,
│   ├── generator.js          переиспользуется всеми
│   └── generator.css         прототипами
├── assets/                   картинки ресурсов
│   └── onboarding/           картинки только для онбординга
└── docs/                     не публикуется (.gitignore)
```

Фича = папка со своим `index.html`. У каждой свой адрес на GitHub Pages,
их можно давать людям по отдельности.

Генератор — не страница, а модуль. Его встраивают онбординг и будущие
здания:

```js
const gen = createGenerator(document.getElementById('app'), {
  assets: '../assets',
  storeKey: 'onboarding',   // своё хранилище, чтобы не делить грядки
  version: 'select',        // A · tool | B · direct | C · select
  mode: 'single',
  cells: 5,                 // меньше 25 — сетка сама станет в один ряд
  chrome: false,            // спрятать служебную кнопку версий
  back: false               // спрятать «‹»: у хоста своя
});

gen.on('change', s => console.log(s.empty, s.growing, s.ready));
gen.highlight(7);           // подсветить ячейку для подсказки
gen.cells();                // копия грядок: состояния и endsAt
gen.setCells([{ index: 0, state: 'ready', typeId: 'strawberry', endsAt: 0 }]);
```
