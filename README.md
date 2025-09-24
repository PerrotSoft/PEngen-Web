# 🎮 PEngen Engine Web: Полная Документация (v1.0)

**PEngen Web** — это модульный 2D-движок на чистом JavaScript, имитирующий структуры, похожие на **SFML**, с отдельными компонентами для **графики (GnuEngenSFML)** и **игровой логики/физики (GameEngine)**.

---

## ⬇️ Быстрый старт и Подключение

Вы можете подключить движок онлайн, используя прямую ссылку на репозиторий.

| Файл | Назначение | Прямая ссылка |
| :--- | :--- | :--- |
| **PEngen  Web** | Основной, скомпилированный файл движка. | [Скачать PEngine Web](https://codeload.github.com/PerrotSoft/PEngen-Web/zip/refs/heads/main) |

### HTML-шаблон для подключения

```html
<!DOCTYPE html>
<html>
<head>
    <title>Моя игра на PEngen</title>
</head>
<body>
    <script src="[https://raw.githubusercontent.com/PerrotSoft/PEngen-Web/main/PEngen.js](https://raw.githubusercontent.com/PerrotSoft/PEngen-Web/main/PEngen.js)"></script>
    
    <script src="game.js"></script>
</body>
</html>
````

-----

## 🚀 1. Компонент GnuEngenSFML (Графика и Ввод)

Класс `GnuEngenSFML` управляет окном (**Canvas**), отрисовкой всех визуальных элементов, анимацией и обработкой интерактивных компонентов.

### 1.1. Класс `GnuEngenSFML` (Отрисовка)

| Метод/Свойство | Описание | Пример использования |
| :--- | :--- | :--- |
| `new GnuEngenSFML()` | Создает экземпляр графического движка. | `Program.gEngine = new GnuEngenSFML();` |
| `InitWindow(w, h, title)` | Инициализирует окно (Canvas). Показывает интро-экран. | `gEngine.InitWindow(800, 600, "My Game");` |
| `AddObject(obj)` | Добавляет объект **`D2Gun`** в список отрисовки. | `gEngine.AddObject(mySprite);` |
| `DeleteObject(obj)` | Удаляет объект **`D2Gun`** из отрисовки. | `gEngine.DeleteObject(intro);` |
| `UpdateObject(old, new)` | Заменяет старый объект новым в списке отрисовки. | `gEngine.UpdateObject(oldText, newText);` |
| **`Draw()`** | **Основная функция отрисовки.** Вызывает отрисовку всех объектов, обновляет анимации, обрабатывает `hover`-эффекты кнопок. **Должна вызываться в цикле игры.** | `requestAnimationFrame(gameLoop);` |
| `objects` | Массив (`D2Gun[]`) всех объектов для отрисовки. | |

### 1.2. Класс `GnuEngenSFML.D2Gun` (Графический объект)

Базовый класс для всех 2D элементов, отображаемых на экране.

| Свойство | Тип | Описание |
| :--- | :--- | :--- |
| `x, y` | `Number` | Позиция объекта. |
| `sx, sy` | `Number` | Размеры объекта (ширина/высота). |
| **`type`** | `ObjectType` | **Обязательно** устанавливается тип объекта (см. `ObjectType`). |
| `texture` | `Image` | HTML-объект `Image` для спрайтов/кнопок без анимации. |
| `animation` | `Animation` | Объект **`Animation`** для анимированных спрайтов/кнопок. |
| `base_color` | `Number (Hex)` | Цвет для `Cube`, `Line`, `Mesh` или цвет текста (например, `0xFFFFFF`). |
| `text` | `String` | Содержимое текста для `Text` и `TextButton`. |
| `font` | `Font` | Объект `Font` для текста. |
| `onClick` | `Function` | Обработчик, вызывается при клике (для `Button`, `TextButton`). |
| `inputString` | `String` | Текущая введенная строка (для `Input`). |
| `isActive` | `Boolean` | Флаг активности поля ввода (для `Input`). |

### 1.3. Перечисление `GnuEngenSFML.ObjectType` (Типы объектов)

| Свойство | Значение | Описание |
| :--- | :--- | :--- |
| `ObjectType.Cube` | `0` | Залитый цветом прямоугольник. |
| `ObjectType.Line` | `1` | Линия между `(x, y)` и `(x+sx, y+sy)`. |
| `ObjectType.Sprite` | `2` | Отрисовка текстуры (`texture` или `animation`). |
| `ObjectType.Text` | `3` | Отрисовка текста (`text`). |
| `ObjectType.Mesh` | `4` | Залитый цветом прямоугольник (похоже на `Cube`). |
| `ObjectType.Button` | `5` | Кнопка-спрайт с hover-эффектом и `onClick`. |
| `ObjectType.TextButton` | `6` | Кнопка-прямоугольник с текстом, hover-эффектом и `onClick`. |
| `ObjectType.Input` | `7` | Поле ввода для текста. |

#### 💡 Примеры использования `D2Gun`

**Создание Cube:**

```javascript
const redBlock = new GnuEngenSFML.D2Gun();
redBlock.x = 10; redBlock.y = 10;
redBlock.sx = 50; redBlock.sy = 50;
redBlock.base_color = 0xFF0000; // Красный
redBlock.type = GnuEngenSFML.ObjectType.Cube;
Program.gEngine.AddObject(redBlock);
```

**Создание TextButton:**

```javascript
const startBtn = new GnuEngenSFML.D2Gun();
startBtn.x = 100; startBtn.y = 200;
startBtn.sx = 200; startBtn.sy = 50;
startBtn.type = GnuEngenSFML.ObjectType.TextButton;
startBtn.text = "Начать игру";
startBtn.font = Program.font;
startBtn.onClick = () => Program.StartGame();
Program.gEngine.AddObject(startBtn);
```

### 1.4. Класс `GnuEngenSFML.Animation`

Используется для анимирования объектов `D2Gun`.

| Свойство/Метод | Тип | Описание |
| :--- | :--- | :--- |
| `new GnuEngenSFML.Animation()` | Конструктор. | |
| `Frames` | `Массив (Image[])` | Список объектов `Image`, составляющих кадры анимации. |
| `FrameSpeed` | `Number` | Время **(в секундах)** между кадрами (по умолчанию `0.1`). |
| `Update(deltaTime)` | **Автоматически вызывается** в `gEngine.Draw()`. Обновляет текущий кадр. | |
| `GetCurrentFrame()` | Возвращает текущий объект `Image` кадра. | |

-----

## ⚙️ 2. Компонент GameEngine (Физика и Логика)

Класс `GameEngine` управляет игровыми объектами, их движением, физикой и столкновениями.

### 2.1. Класс `GameEngine`

| Метод/Свойство | Описание | Пример использования |
| :--- | :--- | :--- |
| `new GameEngine()` | Создает экземпляр игрового движка. | `Program.engine = new GameEngine();` |
| `AddObject(obj)` | Добавляет объект **`GameObject`** в симуляцию. | `engine.AddObject(myPhysicsObject);` |
| `DeleteObject(obj)` | Удаляет объект из симуляции и его `visual` из `gEngine`. | `engine.DeleteObject(enemy);` |
| `UpdateObject(old, new)` | Заменяет старый игровой объект новым в симуляции и обновляет его `visual`. | |
| **`Run()`** | **Запускает основной игровой цикл,** который обрабатывает физику, коллизии, логику и вызывает `gEngine.Draw()`. | `Program.engine.Run();` |

### 2.2. Класс `GameEngine.GameObject` (Физический объект)

| Свойство | Тип | Описание |
| :--- | :--- | :--- |
| `x, y` | `Number` | Позиция объекта в физическом мире. |
| `vx, vy` | `Number` | **Скорость** по осям X и Y (velocity). |
| `width, height` | `Number` | Размеры для расчета **AABB коллизий**. |
| `isStatic` | `Boolean` | Если `true`, объект не двигается физикой. (По умолчанию `false`). |
| `mass` | `Number` | Масса объекта (влияет на отскок). (По умолчанию `1.0`). |
| **`OnUpdate`** | `Function` | Функция, вызываемая в цикле логики: `(self) => { ... }`. |
| `visual` | `GnuEngenSFML.D2Gun` | Привязанный графический объект. Его позиция **автоматически обновляется** движком. |

#### 💡 Пример создания физического объекта

```javascript
const boxVisual = new GnuEngenSFML.D2Gun();
// ... настройка boxVisual ...

const boxPhysics = new GameEngine.GameObject();
boxPhysics.x = 50; boxPhysics.y = 50;
boxPhysics.width = 50; boxPhysics.height = 50;
boxPhysics.mass = 5.0;
boxPhysics.vx = 100; // Начать движение

boxPhysics.visual = boxVisual;
boxPhysics.OnUpdate = (self) => {
    // Пользовательская логика
};
Program.engine.AddObject(boxPhysics);
```

-----

## ⏳ 3. Общие Вспомогательные Классы

### 3.1. Класс `Clock` (Время и DeltaTime)

| Метод/Свойство | Описание |
| :--- | :--- |
| `new Clock()` | Создает часы. |
| **`restart()`** | **Сбрасывает** часы и возвращает объект с прошедшим временем. |
| `asSeconds()` | Метод, который нужно вызывать на результате `restart()`, чтобы получить **deltaTime** (время в секундах). |
| `Clock.WaitSeconds(seconds)` | **Статический асинхронный метод.** Ждет указанное количество секунд. **Используется с `await`**. |

### 3.2. Классы Ввода (`Keyboard` и `Mouse`)

| Команда | Класс | Описание |
| :--- | :--- | :--- |
| `Keyboard.isKeyPressed(key)` | `Keyboard` | Проверяет, нажата ли клавиша. **Используйте `.toUpperCase()`**. |
| `Mouse.GetPosition(window)` | `Mouse` | Возвращает объект `{ x: num, y: num }` с координатами курсора. |
| `Mouse.IsButtonPressed(button)` | `Mouse` | Проверяет, нажата ли кнопка мыши (имитирует левую кнопку). |

### 3.3. Класс `GnuEngenSFML.FloatRect` (Ручные Коллизии)

Используется для **ручной** проверки прямоугольных коллизий.

| Метод | Описание |
| :--- | :--- |
| `new FloatRect(x, y, width, height)` | Создает прямоугольник. |
| `intersects(otherRect)` | Возвращает `true`, если прямоугольник пересекается с `otherRect`. |
| `Contains(x, y)` | Возвращает `true`, если точка `(x, y)` находится внутри прямоугольника. |

-----

## 🧩 4. Компонент SuperUpdate (Сложное Управление Сценой)

`SuperUpdate` предназначен для удобного управления объектами, которые могут быть либо графическими, либо физическими, либо и теми, и другими, объединяя их под одним **именем**.

### 4.1. Класс `SuperUpdate`

| Метод | Описание | Пример использования |
| :--- | :--- | :--- |
| `new SuperUpdate(gEngine, engine)` | **Конструктор.** Принимает экземпляры обоих движков. | `Program.su = new SuperUpdate(gEngine, engine);` |
| `AddObject(obj)` | Добавляет объект **`GameOBJN`** и его компоненты в соответствующий движок. | `su.AddObject(playerObjN);` |
| `DeleteObject(name)` | Удаляет объект с указанным именем из списка и из соответствующих движков. | `su.DeleteObject("player");` |
| `FindObject(name)` | Находит объект **`GameOBJN`** по имени. | `const p = su.FindObject("player");` |
| `Clear()` | Удаляет **все объекты** из `SuperUpdate` и обоих движков. | `su.Clear();` |
| `Exists(name)` | Проверяет, существует ли объект с таким именем. | |
| `Update()` | Обновляет все объекты в списках движков. | |

### 4.2. Класс `SuperUpdate.GameOBJN` (Единый объект)

| Свойство | Тип | Описание |
| :--- | :--- | :--- |
| `name` | `String` | **Уникальное имя** объекта для поиска. |
| `d2Gun` | `GnuEngenSFML.D2Gun` | Графический компонент. |
| `gameObject` | `GameEngine.GameObject` | Физический компонент. |
| `gnuType` | `SuperUpdate.GnuType` | Тип компонента, который будет добавлен/удален: `GnuType.Gnu` (графика) или `GnuType.Engen` (физика). |

#### 💡 Пример добавления объекта через SuperUpdate

```javascript
// 1. Создание компонентов (связь обязательна)
const playerVisual = new GnuEngenSFML.D2Gun();
const playerPhysics = new GameEngine.GameObject();
playerPhysics.visual = playerVisual; 

// 2. Создание объединенного объекта
const playerObjN = new SuperUpdate.GameOBJN();
playerObjN.name = "Player";
playerObjN.d2Gun = playerVisual;
playerObjN.gameObject = playerPhysics;
playerObjN.gnuType = SuperUpdate.GnuType.Engen; 

// 3. Добавление в SuperUpdate
Program.su.AddObject(playerObjN);
```

