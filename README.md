🎮 Документация игрового движка PEngen
PEngen — это модульный JavaScript-движок для 2D-разработки, вдохновленный архитектурой SFML. Движок разделен на два основных компонента: GnuEngenSFML (графика и ввод) и GameEngine (физика и логика).

🚀 Начало работы
1. Подключение
Чтобы начать, просто подключите файл PEngen.js в ваш HTML-документ.

HTML

<!DOCTYPE html>
<html>
<head>
    <title>Моя игра на PEngen</title>
</head>
<body>
    <script src="https://raw.githubusercontent.com/PerrotSoft/PEngen-Web/main/PEngen.js"></script>
    <script src="game.js"></script>
</body>
</html>
2. Структура проекта
Для запуска игры необходимы минимальные файлы:

Файл	Назначение
index.html	Точка входа, подключает движок.
game.js	Ваша основная игровая логика и цикл.
res/	Папка для ресурсов (шрифты, изображения, аудио).

Экспортировать в Таблицы
📦 Компоненты Движка
1. GnuEngenSFML (Графика, Окно, Ввод)
Отвечает за отрисовку объектов, управление окном (Canvas) и обработку пользовательского ввода.

Класс GnuEngenSFML (Ядро графики)
Метод/Свойство	Описание	Пример
new GnuEngL()	Создание экземпляра движка.	Program.gEngine = new GnuEngenSFML();
InitWindow(w, h, title)	Инициализация игрового окна.	gEngine.InitWindow(800, 600, "My Game");
AddObject(obj)	Добавление объекта для отрисовки.	gEngine.AddObject(mySprite);
Draw()	Основная функция отрисовки (вызывается в цикле).	requestAnimationFrame(loop);

Экспортировать в Таблицы
Класс GnuEngenSFML.D2Gun (Графический объект)
Базовый класс для всех отрисовываемых элементов: спрайтов, текста, кнопок.

Свойство	Тип	Описание
x, y, sx, sy	Number	Позиция и размеры.
type	ObjectType	Обязательный тип объекта (Sprite, Button, Text и т.д.).
texture	Image	Объект изображения.
onClick	Function	Обработчик, вызываемый при клике (для кнопок).

Экспортировать в Таблицы
Пример создания TextButton:

JavaScript

const startBtn = new GnuEngenSFML.D2Gun();
startBtn.x = 100; 
startBtn.y = 200;
startBtn.sx = 200; 
startBtn.sy = 50;
startBtn.type = GnuEngenSFML.ObjectType.TextButton;
startBtn.text = "Начать игру";
startBtn.font = Program.font;

// Добавление логики клика
startBtn.onClick = () => { console.log("Game Start!"); };

Program.gEngine.AddObject(startBtn);
2. GameEngine (Физика и Логика)
Отвечает за физическую симуляцию, обновление положения объектов и коллизии.

Класс GameEngine.GameObject (Физический объект)
Объекты, участвующие в физическом мире.

Свойство	Тип	Описание
vx, vy	Number	Скорость (Velocity) по осям X и Y.
width, height	Number	Размеры для расчета AABB коллизий.
isStatic	Boolean	Если true, объект не двигается физикой (по умолчанию false).
mass	Number	Масса объекта (влияет на отскок).
OnUpdate	Function	Пользовательская логика, вызываемая каждый кадр.
visual	D2Gun	Привязанный графический объект.

Экспортировать в Таблицы
Пример создания физического объекта:

JavaScript

// 1. Создаем графический компонент
const blockVisual = new GnuEngenSFML.D2Gun();
blockVisual.base_color = 0xFF0000; // Красный
blockVisual.type = GnuEngenSFML.ObjectType.Cube;

// 2. Создаем физический компонент
const blockPhysics = new GameEngine.GameObject();
blockPhysics.x = 50; 
blockPhysics.y = 50;
blockPhysics.width = 50; 
blockPhysics.height = 50;
blockPhysics.mass = 5.0;
blockPhysics.vx = 100; // Движение вправо
blockPhysics.visual = blockVisual; // Связываем с графикой

Program.engine.AddObject(blockPhysics);
⏳ Вспомогательные Классы
3.1. Класс Clock (Время)
Используется для измерения времени, прошедшего между кадрами (deltaTime).

Метод	Описание
restart().asSeconds()	Возвращает deltaTime (время, прошедшее с последнего кадра) в секундах.
Clock.WaitSeconds(seconds)	Асинхронный. Приостанавливает выполнение async функции на заданное время.

Экспортировать в Таблицы
Использование deltaTime:

JavaScript

// Внутри главного игрового цикла (loop)
const deltaTime = Program.engine.clock.restart().asSeconds();

// Пример движения, зависящего от времени
player.x += player.vx * deltaTime;
3.2. Ввод с клавиатуры и мыши
Статические классы для отслеживания состояния ввода.

Команда	Описание
Keyboard.isKeyPressed('W')	Проверяет, нажата ли клавиша (используйте верхний регистр, например, 'A', 'SPACE').
Mouse.GetPosition(window)	Возвращает текущие координаты курсора { x, y } относительно игрового окна.
Mouse.IsButtonPressed()	Проверяет, нажата ли левая кнопка мыши.

Экспортировать в Таблицы
🧩 Управление Сценой (SuperUpdate)
Класс-посредник, позволяющий удобно управлять объединенными объектами, имеющими как физические, так и графические компоненты.

Класс SuperUpdate.GameOBJN
Единая сущность для объединения компонентов:

Свойство	Описание
name	Уникальное имя объекта для поиска (через FindObject(name)).
d2Gun	Графический компонент (GnuEngenSFML.D2Gun).
gameObject	Физический компонент (GameEngine.GameObject).
gnuType	Тип компонента, который управляет объектом (для внутренней логики).

Экспортировать в Таблицы
⬇️ Скачать Движок
Файл	Описание	Скачать
PEngine.js	Основной, скомпилированный файл движка.	Скачать PEngine.js
Example.js	Шаблон/пример файла для старта проекта.	Скачать Example.js
