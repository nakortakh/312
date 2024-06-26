Vite (произносится как `вите`) - новый инструмент сборки, вдохновителем которого является Evan Vue (создатель фреймврока Vue)

https://vitejs.dev/
https://vitejs.ru/

v 5.2.11 Vite:

- для dev режима использует под капотом esbuild - https://www.npmjs.com/package/esbuild
- для production режима использует rollup - https://www.npmjs.com/package/rollup

И тот и другой инструмент является популярным, т.к. число скачиваний в неделю

- esbuild - 25 432 289
- rollup - 19 217 610

Во многом, для большинства кейсов отдельно знать esbuild и rollup в частности не потребуется. Но для каких-то специфических вещей придется разобраться в особенностях того или иного сборщика + еще шарить за vite.

Из плюсов vite:

- Быстрая запуск dev-сервера даже на холодную 
- Быстрый HMR, без необходимости обновления страницы полностью (хотя пишут, что и в вебпаке так можно сделать)
- Есть своя CLI, которая помогает создать новое приложение.
- Есть поддержка Module Federation
- Многие вещи доступны под капотом (работа с 
- Можем использовать синтаксис ESM (import.meta) далее подробнее
- Подробная документация
- Большое сообщество
- Поддерживает плагины от Rollup

Отличия от webpack:
- работа с шаблоном html (HTMLWebpackPlugin), работа с переменными окружения (DefinePlugin), работа с css (в том числе и модулями) под капотом. Не требует отдельных плагинов.
- при работе в dev режиме Vite не проверяет типы, делегируя это IDE и build
- в dev режиме моментально срабатывает HMR, в отличии от webpack
- работает с многими расширениями из коробки, в webpack нужно устанавливать модули.
- файл конфига может иметь расширение ts и использовать esm модули, а не cjs как в webpack

Т.к. build собирается Rollup, ощутимого прироста по сравнению с webpack не ожидается. Но в планах у разработчиков Vite использовать [Rolldown](https://rolldown.rs/about "https://rolldown.rs/about"). Это Rollup переписанный на Rust.Что в будущем позволит при поднятии версии vite получить ощутимый прирост в производительности build сборок.


Минусы: 
- Есть проблемы с поддержкой плагина module federation. Сейчас вышла версия 2.0, не известно будет ли обновляться плагин в Vite.
- Проблемы с интеграцией собранных Vite модулей со старыми cjs модулями, которые собираются Webpack. Проблемы не будет если все собирать на Vite
- Для некоторых кейсов необходимо разбираться в esbuild и rollup документации.

Список официальных [Plugins](https://vitejs.dev/plugins/ "https://vitejs.dev/plugins/")

Использование [import.meta](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import.meta "https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import.meta")




Быстрый HMR достигается благодаря golang (esbuild написан на нем) и использованию новых возможностей es-модулей. Без компиляции в старые cjs модули и т.д.
Плюс во время dev режима не выполняется пребилд и проверка типов, т.е. приложение стартует как есть.
  
Примечание - Для Vite требуется Node.js версии 18+



`npm create vite@latest`

![image](https://github.com/expMax-web/312/assets/84277100/33c73c66-5d45-49f2-aadc-a29a27bd6812)


