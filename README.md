# Файловая система

## Задание

Необходимо реализовать такую файловую систему, в которой можно перемещаться по директориям и просматривать их содержимое
с помощью консольных команд.

Файловая система должна поддерживать следующие команды в формате `<команда>` или `<команда> <аргумент>`:

| Команда | Аргумент       | Описание                                                                        |
|---------|----------------|---------------------------------------------------------------------------------|
| `cd`    | нет            | Переключение текущей директории. Без аргумента ничего не делает                 |
| `cd`    | `..`           | Переключение текущей директории на родительскую                                 |
| `cd`    | имя директории | Переключение текущей директории на вложенную, соответствующую переданному имени |
| `exit`  | нет            | Выход из системы                                                                |
| `ls`    | нет            | Вывод списка вложенных файлов и директорий                                      |
| `pwd`   | нет            | Вывод полного пути текущей директории, начиная от корневой                      |
| `quit`  | нет            | то же, что и `exit`                                                             |

В системе предусмотрена следующая иерархия файлов:

- `/` — корневая директория
  - `Documents/` — директория с документами
    - `Books/` — директория с книгами (случайно тут оказалась)
      - `Гарри Поттер.pdf` — одна из серии книг о Гарри Поттере
      - `Мастер и Маргарита.epub` — электронная книга с произведением Булгакова
    - `Ипотека/` — директория с документами для ипотеки
      - `НДФЛ.pdf` — справка НДФЛ
    - `Паспорт.pdf` — сканы паспорта
    - `СНИЛС.jpg` — скан СНИЛС
  - `Movies/` — директория с фильмами
    - `Ужасы/` — директория со страшными фильмами
      - `Молчание ягнят.mkv` — знаменитый ужастик с Энтони Хопкинсом
      - `Чужой.mp4` — научная фантастика с молодой Сигурни Уивер
    - `Фантастика/` — директория с фильмами-сказками
      - `Аватар.mov` — фантастический фильм про синих человечков
    - `Любовь и голуби.mov` — советсткая классика
  - `Music/` — директория с музыкой (почему-то пустая)
  - `Photos/` — директория с фотографиями
    - `Выпускной/` — директория с фото школьного выпускного
      - `001.jpg` — фото
      - `003.jpg` — фото
      - `004.jpg` — фото
    - `Свадьба/` — директория с бракосочетания
      - `wed_21.jpg` — фото
      - `wed_22.jpg` — фото
      - `wed_23.jpg` — фото
      - `wed_27.jpg` — фото
  - `я.jpg` — лучшее селфи

## Работа с системой

1. Система выводит текущие время (формат `день.месяц часы:минуты`) и полный путь директории, а также предлагает пользователю ввести команду.
2. Пользователь вводит команду и нажимает `Enter`.
3. Система выполняет введенную команду и выводит результат, если требуется.
4. Система снова предлагает пользователю ввети команду (возвращаемся к п. 1).

Пример работы системы:

```bash
[01.01 13:30] / $ cd Photos
[01.01 13:30] /Photos/ $ cd Выпускной
[01.01 13:30] /Photos/Выпускной/ $ ls
[f] 001.jpg
[f] 003.jpg
[f] 004.jpg
[01.01 13:30] /Photos/Выпускной/ $ exit
```

## Команды системы

### Выход из системы

Выход из системы осуществляется с помощью ввода команды `exit` или `quit`. При этом производится программый выход
с нулевым кодом — `os.Exit(0)`.

### Вывод текущего пути

С помощью команды `pwd` выводится полный путь текущей директории, начиная с корневой:

```bash
[01.01 13:31] /Documents/Books/ $ pwd
/Documents/Books/
[01.01 13:31] /Documents/Books/ $
```

### Вывод содержимого

С помощью команды `ls` выводится список всех вложенных в текущую директорию файлов и поддиректорий в формате:
`[<флаг>] <имя файла>`, где флаг — `f` для файлов и `d` для директорий. Пример:

```bash
[01.01 13:32] /Movies/ $ ls
[d] Ужасы
[d] Фантастика
[f] Любовь и голуби.mov
```

### Переключение текущей директории

Все переходы по директориям осуществляются с помощью команды `cd`.

Если ввести команду `cd` без аргумента, то ничего не произойдет:

```bash
[01.01 13:33] /Movies/ $ cd
[01.01 13:33] /Movies/ $
```

Если ввести команду `cd ..` (с аргументом в виде двух точек), то происходит переход в родительскую директорию:

```bash
[01.01 13:34] /Movies/Ужасы/ $ cd ..
[01.01 13:34] /Movies/ $
```

При этом если попытаться перейти в родителя корневой директории, то ничего не произойдет:

```bash
[01.01 13:35] / $ cd ..
[01.01 13:35] / $
```

Если для команды `cd` в качестве аргумента указать имя вложенной директории, то происходит переключение на нее:

```bash
[01.01 13:36] /Movies/ $ cd Ужасы
[01.01 13:36] /Movies/Ужасы/ $
```

Если в аргументе передать файл или несуществующую директорию, то выводится ошибка `Неизвестная директория: <имя>`:

```bash
[01.01 13:37] /Movies/ $ cd Любовь и голуби.mov
Неизвестная директория: Любовь и голуби.mov
[01.01 13:37] /Movies/Ужасы/ $
```

### Прочее

Если ввести пустую команду (просто нажать `Enter`), то ничего не должно происходить.
Однако, если ввести несуществующую команду, то должна выводиться ошибка `Неизвестная команда: <команда>`:

```bash
[01.01 13:38] /Movies/ $ abcd efg hij
Неизвестная команда: abcd
[01.01 13:38] /Movies/ $
```

## Загрузка иерархии файлов

В системе файлы и директории являются одной и той же сущностью.
Директория отличается от файла только специальным булевым признаком.  

В `data.go` находятся исходные данные иерархии файлов:

- `fileNames` — массив имен файлов.
- `directories` — мапа, в которой ключи — индексы файлов из `fileNames`, которые являются директориями.
- `fileParents` — мапа, в которой ключ — индекс файла, а значение — индекс родительской директории, в которую он вложен.

Если для директории или файла нет родительской директории из `fileParents`, значит, они лежат в корневой директории.

Необходимо в `main.go` в функции `loadFiles` из исходных данных `data.go` собрать структуру "Дерево", узлы которого имплементируют интерфейс `File`.
Функция `loadFiles` должна вернуть назад корень дерева — корневую директорию с пустым именем (при этом ее полный путь — `/`).
Через такой корень можно перемещаться вглубь дерева и назад к корню.

Структура "Дерево" — это набор объектов, которые связаны друг с другом следующим образом:

- у дерева есть корень — это объект, с которого начинается дерево;
- каждый объект дерева называется "Узел". У узла есть указатель на родителя, а также указатели на вложенные объекты — так называемые ветки;
- у корня указатель на родителя пустой;
- самый последний узел в ветке называется "Листом", у него нет указателей на вложенные объекты.

В файловом дереве листы — это или файлы, или пустые директории.

# Контроль качества

Для самоконтроля необходимо выполнить тесты `main_test.go`. Модифицировать тесты запрещено!
