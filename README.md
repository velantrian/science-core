# 🧠 Velantrim · Science Core KB

**Source of Truth** для машинной базы знаний проекта Velantrim.
Платоническое ядро всех наук — структура и законы, а не каталог фактов.

> Не «все 2 068 366 видов», а «механизм клеточного деления, применимый ко всем».
> Не «все 6 145 минералов», а «топ-500 + кристаллохимические принципы».

---

## 📂 Структура репозитория

```
science-core/
├── TZ_CORE_00_MASTER.md           ← мастер-спецификация v3.2 (source of truth)
├── TZ_CORE_01_PHYSICS.md          ← карты подтем по доменам
├── TZ_CORE_02_MATH.md
├── TZ_CORE_03_CHEMISTRY.md
├── TZ_CORE_04_MINERALS.md
├── TZ_CORE_05_BIOLOGY.md
├── TZ_CORE_06_LOGIC.md
├── TZ_CORE_07_VARIANT.md
├── TZ_CORE_08_PRACTICAL.md
├── TZ_CORE_09_FRONTIER_10_ABSTRACT.md
├── TZ_CLAUDE_CODE.md              ← инструкция для агентов-сборщиков
│
├── schema.json                    ← JSON Schema Draft 2020-12 (v3.2)
│
├── data/
│   └── raw/                       ← JSON-батчи фактов (по 25 в каждом)
│       ├── physics_001.json
│       ├── math_001.json
│       └── ...
│
├── registry/
│   └── collected.json             ← реестр id, подтем, батчей
│
└── output/
    └── graph.json                 ← собранный граф (nodes + edges)
```

---

## 🧬 Эпистемические тиры

| tier        | Смысл                                                      | confidence | Примеры                          |
|-------------|------------------------------------------------------------|:----------:|----------------------------------|
| `invariant` | Не изменится через 50 лет                                  | 0.90–1.0   | F = m·a, теорема Пифагора        |
| `variant`   | Меняется со временем — нужна дата и источник               | 0.40–0.90  | население страны, состав сплавов |
| `practical` | Технологические процессы с условиями применимости          | 0.70–0.95  | BOF, КМОП, ПЦР                   |
| `logic`     | Правила мышления и рассуждения                             | 0.80–1.0   | modus ponens, fallacies          |
| `frontier`  | Открытые нерешённые проблемы                               | 0.10–0.60  | гипотеза Римана, P vs NP         |
| `abstract`  | Механизмы разума, паттерны культуры                        | 0.50–0.90  | мысленный эксперимент, архетипы  |

---

## 📦 Схема факта v3.2 — минимальный пример

```json
{
  "id": "law.physics.mechanics.newton_2",
  "schema_version": "3.2",
  "domain": "physics.mechanics",
  "subtopic": "physics.mechanics.dynamics",
  "tier": "invariant",
  "type": "law",
  "statement": "Ускорение тела прямо пропорционально приложенной силе и обратно пропорционально его массе.",
  "formal_notation": "F = m·a",
  "conditions": "Инерциальная система отсчёта; v << c",
  "limits": ["Не применимо при v близких к c", "Не применимо для квантовых объектов"],
  "prereq": ["concept.physics.force", "concept.physics.mass"],
  "derives_from": [],
  "confidence": 0.99,
  "tags": ["mechanics", "newton", "dynamics", "force"]
}
```

Полная спецификация полей, реестр 30 типов, шкала confidence и формат id — в `TZ_CORE_00_MASTER.md`.

---

## 🔧 Pipeline

Пайплайн сбора и валидации живёт в соседнем репозитории `velantrim-eiti/velantrim_core/`:

```bash
collect.py       # генерирует промпт для очередного батча
validate.py      # проверяет батч по schema.json v3.2
check_links.py   # ищет оборванные prereq и derives_from
build_graph.py   # собирает все батчи в output/graph.json
```

Чтобы запустить пайплайн на этом репозитории, укажите путь к `data/raw/`
из `science-core` в качестве `RAW_DIR` для `validate.py` и `build_graph.py`.

---

## 📊 Плановые объёмы

| Домен        | Tier                     | Целевой объём фактов |
|--------------|--------------------------|:--------------------:|
| Physics      | invariant                |     600–700          |
| Math         | invariant                |     650–750          |
| Chemistry    | invariant                |  2 000–2 500         |
| Minerals     | invariant + variant      |     450–550          |
| Biology      | invariant                |     600–700          |
| Logic        | logic                    |     800–900          |
| Variant      | variant                  |  4 500–5 000         |
| Practical    | practical                |     750–850          |
| Frontier     | frontier                 |     400–500          |
| Abstract     | abstract                 |     180–220          |
| **ИТОГО**    |                          | **~11 000–13 000**   |

---

## ✅ Чек-лист батча

```
☑ subtopic не покрыта в collected.json?
☑ validate.py → 0 критических ошибок?
☑ 0 педагогических сигналов в statements?
☑ Все id в формате ≥3 сегментов, строчные?
☑ Все type из реестра 30 значений?
☑ confidence соответствует правилам type→confidence?
☑ Реестр collected.json обновился?
```

---

## 🚫 Что НЕ собираем в Core KB

- Перечни всех видов / всех минералов / всех ISO-стандартов → это Encyclopedia layer
- Конкретные версии ПО и протоколов — вариантное, устаревает
- Исторический контекст и биографии учёных — педагогика
- Объяснения, аналогии, упражнения — генерируются LLM на лету

---

*Velantrim · Science Core KB · Schema v3.2*
