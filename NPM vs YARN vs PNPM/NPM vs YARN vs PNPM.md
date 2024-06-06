# NPM vs YARN vs PNPM
## Что такое менеджер пакетов?
Система управления пакетами (также иногда «менеджер пакетов» или «пакетный менеджер») — набор программного обеспечения, позволяющего управлять процессом установки, удаления, настройки и обновления различных компонентов программного обеспечения.

B ряде экосистем языков программирования созданы собственные менеджеры пакетов, обеспечивающие установку приложений на этих языках и необходимых библиотек, среди таковых Composer (PHP), NPM (JavaScript, менеджер пакетов в составе Node.js), Pip (Python), Gem (Ruby), NuGet (.NET).

## Основные этапы установки пакетов
- Скачивание архива
- Парсинг и распаковка
- Копирование файлов в каталог node_modules

## Как Node.js резолвит модули
./home/SomeUser/AllProjects/TheBestOfTheBestProject/index.js вызовем require('react')

Базовая работа алгоритма
<br/>
./home/SomeUser/AllProjects/TheBestOfTheBestProject/node_modules/react
<br/>
./home/SomeUser/AllProjects/node_modules/react
<br/>
./home/SomeUser/node_modules/react
<br/>
./home/node_modules/react
<br/>
./node_modules/react

## Как работает npm
### Каталог node_modules при npm v1 и npm v2 полностью повторял структуру зависимостей

```
TheBestOfTheBestProject
  |--node_modules
    |--react
      |--node_modules
        |--loose-envify
          |--node_modules
            |--js-tokens
        |--object-assign
```
### Начиная с npm v3 список зависимостей стал плоским
```
TheBestOfTheBestProject
  |--node_modules
    |--react
    |--loose-envify
    |--js-tokens
    |--object-assign
```
Но здесь могут быть проблемы: 
- Сложный недетерменированный результат при подъеме (hoisting) модулей на уровень выше: при каждом подъеме (hoisting) модулей мы можем получать разный результат.
- Фантомные зависимости
- Дублирование пакетов

Происходит это из-за особенности алгоритма: при поднятии пакета на уровень выше, происходит проверка на наличие аналогичного пакета и если на уровне выше есть тот же пакет любой версии, то поднятие не происходит. Это может привести к дублированию пакетов.

## Как работает yarn plug'n'play
В yarn pnp отказались от копирования файлов с распаковкой в пользу каталога .yarn/cache
```
TheBestOfTheBestProject
  |--.yarn/caсhe/
    |--react-npm-17.0.1-y19NCjxPq4qG5KdKW8bQZ.zip
    |--loose-envify-npm-1.4.0-QNK6k83GxYFCjPYt9FUBa.zip
    |--js-tokens-npm-4.0.0-P4UKwUqh9H8wTu6ei6wdEY8YAC.zip
    |--object-assign-npm-4.1.1-iot6za5pejdihqLzGkVPvuKrAt.zip
```
Но для работы данной схемы требуются файлы .pnp.cjs и .pnp.loader.mjs для подмены модулей и функций в момент исполнения кода (Runtime).

Что делает Runtime:
- Переопределяет поведение файловой систмы и require
- Читает из архива
- Валидирует связи между модулями
- Кастомизация поиска модуля для ECMAScript modules (.pnp.loader.mjs)

Особенности:
- Быстрая установка любых версий зависимостей с отсутствием дублей
- Более медленный запуск кода т к нужно ходить в архивы и валидировать связи между пакетами
- Отсутствует официальная поддержка TS (подключается с помощью специальных патчей)

## Как работает pnpm
Используются symlink'и
```
TheBestOfTheBestProject
  |--node_modules
    |--.pnpm
      |--react@17.0.2
        |--node_modules
          |--loose-envify -> ../../loose-envify@1.4.0/node_modules/loose-envify
          |--object-assign -> ../../object-assign@4.1.1/node_modules/object-assign
          |--react
      |--loose-envify@1.4.0
        |--node_modules
          |--loose-envify
          |--js-tokens -> ../../js-tokens@4.0.0/node_modules/js-tokens
      |--object-assign@4.1.1
        |--node_modules
          |--object-assign
      |--js-tokens@4.0.0
        |--node_modules
          |--js-tokens
  |--react -> .pnpm/react@17.0.2/node_modules/react
```
Особенности:
- Любая зависимость может достучаться до любого пакета, который мы установили сами через package.json
- Устанавливает один экземпляр любой версии пакета
- Кэш должен находится на одном диске с проектом

## Cравнение команд между npm, yarn и pnpm.
| npm команда                 | yarn команда               | pnpm аналог                |
| --------------------------- | -------------------------- | -------------------------- |
| npm install                 | yarn                       | pnpm install               |
| npm install [module_name]   | yarn add [module_name]     | pnpm add [module_name]     |
| npm uninstall [module_name] | yarn remove [module_name]  | pnpm remove [module_name]  |
| npm update                  | yarn upgrade               | pnpm update                |
| npm list                    | yarn list                  | pnpm list                  |
| npm run [scriptName]        | yarn [scriptName]          | pnpm [scriptName]          |
| npx [command]               | yarn dlx [command]         | pnpm dlx [command]         |
| npm exec                    | yarn exec [commandName]    | pnpm exec [commandName]    |
| npm init [initializer]      | yarn create [initializer]  | pnpm create [initializer]  |

## Скорость установки пакетов
В большей степени нас интересуют моменты, когда у нас есть:
- только lock file
- lock file и cache
- lock file, cache и node_modules

![benchmarks](https://github.com/nakortakh/312/blob/main/NPM%20vs%20YARN%20vs%20PNPM/imgs/thumbnail-benchmarks-js-pm.png)

### На примере установки зависимостей в мф ufo-services-front
npm/pnpm install with cache and node_modules and lock file     
| npm                         | pnpm                       |
| --------------------------- | -------------------------- |
| ![npm install with cache and node_modules and lock file](https://github.com/VitekShuk/articles/blob/main/NPM%20vs%20YARN%20vs%20PNPM/imgs/npm%20install%20with%20cache%20and%20lock%20file%20and%20node_modules.JPG)                 | ![pnpm install with cache and node_modules and lock file](https://github.com/VitekShuk/articles/blob/main/NPM%20vs%20YARN%20vs%20PNPM/imgs/pnpm%20install%20with%20cache%20and%20node_modules%20and%20lock%20file.JPG) |

npm/pnpm install with cache and lock file and without node_modules    
| npm                         | pnpm                       |
| --------------------------- | -------------------------- |
| ![npm install with cache and lock file and without node_modules](https://github.com/VitekShuk/articles/blob/main/NPM%20vs%20YARN%20vs%20PNPM/imgs/npm%20install%20with%20cache%20and%20without%20node_modules.JPG) | ![pnpm install with cache and lock file and without node_modules](https://github.com/VitekShuk/articles/blob/main/NPM%20vs%20YARN%20vs%20PNPM/imgs/pnpm%20install%20without%20node_modules%20and%20with%20lock%20file%20and%20cache.JPG) |

npm/pnpm install without cache and node_modules and lock file
| npm                         | pnpm                       |
| --------------------------- | -------------------------- |
| ![npm install without cache and node_modules and lock file](https://github.com/VitekShuk/articles/blob/main/NPM%20vs%20YARN%20vs%20PNPM/imgs/npm%20install%20without%20all.JPG) |  ![pnpm install without cache and node_modules and lock file](https://github.com/VitekShuk/articles/blob/main/NPM%20vs%20YARN%20vs%20PNPM/imgs/pnpm%20install%20without%20cache%20and%20node_modules%20and%20lock%20file.JPG) |


## Деплой на среды
### npm

### yarn
У yarn'а есть две стратегии кэширования
1. Offline mirror. Установив в .yarnrc.yml свойство enableGlobalCache в значение false, он сохранит кеш пакета в локальной папке вашего проекта (по умолчанию .yarn/cache), которую затем можно будет добавить в Git. Таким образом, каждый коммит гарантированно будет установлен, даже если реестр npm выйдет из строя.
2. Zero-installs. Для этой стратегии используется расширение для гита git lfs.
   ```
   git lfs install
   git lfs track "*.zip"
   ```
   После этого все zip-архивы хранятся внутри git-истории просто как ссылки. Ссылки ведут на реальные архивы, которые загружаются при pull, clone и тд.

### pnpm

## Предварительный вывод
1. yarn pnp и pnpm быстрее чем npm при использовании лок файла и кэша
2. yarn pnp и pnpm пезопаснее чем npm т к используют для проверки контрольные суммы
3. yarn сложнее в настройке чем pnpm
4. у yarn и pnpm есть возможность выбрать структуру каталога node_modules, npm это не позволяет
5. При использовании yarn или pnpm пакеты занимают меньше дискового пространства

