# Гайд по кофигу Vite

#### [Ссылка на параметры конфига из официальной документации](https://vitejs.dev/config/)

#### [Ссылка на параметры конфига на русском (актуальность не подверждена)](https://v3.vitejs.ru/config/)

## Параметры

- [`appType: 'spa' | 'mpa' | 'custom' (default: 'spa')`](https://vitejs.dev/config/shared-options.html#apptype "https://vitejs.dev/config/shared-options.html#apptype") <br>
  Тип приложения
  <br>

- [`assetsInclude: string | RegExp | (string | RegExp)[]`](https://vitejs.dev/config/shared-options.html#assetsinclude "[vitejs.dev](https://vitejs.dev/config/shared-options.html#assetsinclude)") <br>
  Тут можно дополнительно указать форматы, которые стоит обрабатывать как статику,
  вне папки public.
  [Список расширений, которые vite понимает как статику](https://github.com/vitejs/vite/blob/main/packages/vite/src/node/constants.ts#L80 "https://github.com/vitejs/vite/blob/main/packages/vite/src/node/constants.ts#L80")
  <br>
- [`base: string`](https://vitejs.dev/config/shared-options.html#base "https://vitejs.dev/config/shared-options.html#base") <br>
  Base public path для приложения, добавляется после port через /
  Например `http://localhost:5173/test` (base: "test")
  <br>
- [`cacheDir: string (default: "node_modules/.vite")`](https://vitejs.dev/config/shared-options.html#cachedir "https://vitejs.dev/config/shared-options.html#cachedir") <br>
  Каталог для сохранения файлов кэша
  <br>
- [`clearScreen: boolean (default: true)`](https://vitejs.dev/config/shared-options.html#clearscreen "https://vitejs.dev/config/shared-options.html#clearscreen") <br>
  Очистка чего? (что-то делает с консолью) пока хз
  <br>
- [`envDir: string (default: root)`](https://vitejs.dev/config/shared-options.html#envdir "https://vitejs.dev/config/shared-options.html#envdir") <br>
  Путь до папки с файлом конфигурации .env
  <br>
- [`envPrefix: string (default: VITE\_)`](https://vitejs.dev/config/shared-options.html#envprefix "https://vitejs.dev/config/shared-options.html#envprefix") <br>
  Префикс для переменных окружения
  Нельзя использовать `''`, vite выдаст ошибку. Чтобы убрать префикс смотри параметр define
  <br>
- [`logLevel: 'info' | 'warn' | 'error' | 'silent' (default: 'info')`](https://vitejs.dev/config/shared-options.html#loglevel "https://vitejs.dev/config/shared-options.html#loglevel") <br>
  Уровень детализации вывода в консоль
  <br>
- [`mode: 'development' | 'production' (default: 'development' for dev server, 'production' for build)`](https://vitejs.dev/config/shared-options.html#mode "https://vitejs.dev/config/shared-options.html#mode") <br>
  Тип сборки. Значение также можно переопределить с помощью --mode в CLI
  <br>
- [`publicDir: string | false (default: "public")`](https://vitejs.dev/config/shared-options.html#publicdir "https://vitejs.dev/config/shared-options.html#publicdir") <br>
  Путь к папке со статикой
  <br>
- [`root: string (default:  process.cwd())`](https://vitejs.dev/config/shared-options.html#root "https://vitejs.dev/config/shared-options.html#root") <br>
  Путь к файлу index.html
  <br>

Опции для локального запуска (dev-server)

`server: {`

- [`host: string | boolean (default: 'localhost')`](https://vitejs.dev/config/server-options.html#server-host "https://vitejs.dev/config/server-options.html#server-host") <br>
  URL для dev-server который мы хотим запустить.
  Для чего значение true - [подробнее](https://learn.microsoft.com/en-us/windows/wsl/networking#accessing-a-wsl-2-distribution-from-your-local-area-network-lan "https://learn.microsoft.com/en-us/windows/wsl/networking#accessing-a-wsl-2-distribution-from-your-local-area-network-lan")
  Можно установить с помощью CLI параметром `--host value`
  <br>
- [`port: number (default: 5173)`](https://vitejs.dev/config/server-options.html#server-port "https://vitejs.dev/config/server-options.html#server-port") <br>
  Порт
  <br>
- [`strictPort: boolean (default: false)`](https://vitejs.dev/config/server-options.html#server-port "https://vitejs.dev/config/server-options.html#server-port") <br>
  Если порт `(например 3001)` уже занят, то при значении `strictPort: true`, выдаст ошибку.
  Иначе займет другой свободный порт
  <br>
- [`https: {}`](https://vitejs.dev/config/server-options.html#server-https "https://vitejs.dev/config/server-options.html#server-https") <br>
  Включаем TLS + HTTP/2
  Примечание, чтобы использовать TLS соединение нужно еще указать параметр server.proxy
  https: Значение также может быть объектом [параметров](https://nodejs.org/api/https.html#https_https_createserver_options_requestlistener "https://nodejs.org/api/https.html#https_https_createserver_options_requestlistener"), передаваемым в `https.createServer()`.
  Нужен действительный secure сертификат
  <br>
- [`open: boolean | string`](https://vitejs.dev/config/server-options.html#server-open "https://vitejs.dev/config/server-options.html#server-open") <br>
  Автоматически открывает браузер при запуске сервера
  Если строка то будет использоваться как URL
  Можно настраивать [options](https://github.com/sindresorhus/open#app "https://github.com/sindresorhus/open#app")
  <br>
- [`proxy: Record<string, string | ProxyOptions>`](https://vitejs.dev/config/server-options.html#server-proxy "https://vitejs.dev/config/server-options.html#server-proxy") <br>
  Правила прокси-сервера для сервера разработки
  перехватывает запросы на ключи и перенаправялет на значения.
  Пример:
  `'/foo': 'http://localhost:4567'`
  `(shorthand: http://localhost:5173/foo -> http://localhost:4567/foo)`
  <br>
- [`cors: boolean | CorsOptions  (default: true)`](https://vitejs.dev/config/server-options.html#server-cors "https://vitejs.dev/config/server-options.html#server-cors") <br>
  CORS для dev сервера
  подробнее [тут](https://github.com/expressjs/cors#configuration-options "https://github.com/expressjs/cors#configuration-options")
  <br>
- [`headers: OutgoingHttpHeaders`](https://vitejs.dev/config/server-options.html#server-headers "https://vitejs.dev/config/server-options.html#server-headers") <br>
  Для настройки заголовков, которые приходят с сервера.
  <br>
- [`hmr: boolean | { protocol?: string, host?: string, port?: number, path?: string, timeout?: number, overlay?: boolean, clientPort?: number, server?: Server }   (default: true)`](https://vitejs.dev/config/server-options.html#server-hmr "https://vitejs.dev/config/server-options.html#server-hmr") <br>
  HMR
  <br>
- [`warmup: { clientFiles: string[], ssrFiles: string[] }`](https://vitejs.dev/config/server-options.html#server-warmup "https://vitejs.dev/config/server-options.html#server-warmup") <br>
  Указанные файлы будут преобразованы и кешированы предварительно, для быстрой загрузки.
  Улучшает начальную загрузку страницы во время запуска сервера.
  `string` - относительный от root путь до файла. <br>
  Например:

  ```
  clientFiles: ['./src/components.vue', './src/utils/big-utils.js'],
  ssrFiles: ['./src/server/modules.js'],
  ```

- [`watch: object | null`](https://vitejs.dev/config/server-options.html#server-watch+ "https://vitejs.dev/config/server-options.html#server-watch+") <br>
  Для настройки просматриваемых файлов при HMR.
  Если `null` то файлы просматриваться не будут. По умолчанию `exclude` `.git/` и `node_modules/`
  Подробнее [тут](https://github.com/paulmillr/chokidar#api "https://github.com/paulmillr/chokidar#api")
  <br>
- [`middlewareMode: boolean (default: false)`](https://vitejs.dev/config/server-options.html#server-middlewaremode "https://vitejs.dev/config/server-options.html#server-middlewaremode") <br>
  Создает сервер Vite в режиме мидлвара.
  `}`
  <br>

Для работы с файловой системой
`fs : {`

- [`strict: boolean (default: true)`](https://vitejs.dev/config/server-options.html#server-fs-strict "https://vitejs.dev/config/server-options.html#server-fs-strict") <br>
  Ограничить обслуживание файлов за пределами корня рабочей области.
  <br>
- [`allow: string[]`](https://vitejs.dev/config/server-options.html#server-fs-allow " https://vitejs.dev/config/server-options.html#server-fs-allow") <br>
  Ограничить файлы, которые можно обслуживать через `/@fs/`
  Если `server.fs.strict: true`, то этот параметр не устанавливать, будет ошибка
  <br>
- [`deny: string[] (default ['.env', '.env.*', '*.{pem,crt}'])`](https://vitejs.dev/config/server-options.html#server-fs-deny "https://vitejs.dev/config/server-options.html#server-fs-deny") <br>
  Черный список для конфиденциальных файлов, обслуживание которых ограничено сервером Vite dev
  <br>
- [`origin: string`](https://vitejs.dev/config/server-options.html#server-origin "https://vitejs.dev/config/server-options.html#server-origin") <br>
  Определяет источник сгенерированных URL-адресов активов во время разработки.
  <br>
- [`sourcemapIgnoreList: false | (sourcePath: string, sourcemapPath: string) => boolean`](https://vitejs.dev/config/server-options.html#server-sourcemapignorelist "https://vitejs.dev/config/server-options.html#server-sourcemapignorelist")
  `(default: (sourcePath) => sourcePath.includes('node_modules'))` <br>
  Функция для добавления в игнор путей содержащих указанные значения
  `}`
  <br>

Опции для сервера, запускаемого с помощью vite preview (то, что собрали в папку после build)
`preview {`

- [`host: string | boolean (default: 'localhost')`](https://vitejs.dev/config/preview-options.html#preview-host "https://vitejs.dev/config/preview-options.html#preview-host") <br>
  URL для dev-server который мы хотим запустить.
  Для чего значение true - [подробнее](https://learn.microsoft.com/en-us/windows/wsl/networking#accessing-a-wsl-2-distribution-from-your-local-area-network-lan "https://learn.microsoft.com/en-us/windows/wsl/networking#accessing-a-wsl-2-distribution-from-your-local-area-network-lan")
  Можно установить с помощью CLI параметром `--host value`
  <br>
- [`port: number (default: 5173)`](https://vitejs.dev/config/preview-options.html#preview-port "https://vitejs.dev/config/preview-options.html#preview-port") <br>
  Порт
  <br>
- [`strictPort: boolean (default: false)`](https://vitejs.dev/config/preview-options.html#preview-port "https://vitejs.dev/config/preview-options.html#preview-port") <br>
  Если порт `(например 3001)` уже занят, то при значении `strictPort: true`, выдаст ошибку.
  Иначе займет другой свободный порт
  <br>
- [`https: {}`](https://vitejs.dev/config/preview-options.html#preview-https "https://vitejs.dev/config/preview-options.html#preview-https") <br>
  Включаем TLS + HTTP/2
  Примечание, чтобы использовать TLS соединение нужно еще указать параметр server.proxy
  https: Значение также может быть объектом [параметров](https://nodejs.org/api/https.html#https_https_createserver_options_requestlistener "https://nodejs.org/api/https.html#https_https_createserver_options_requestlistener"), передаваемым в `https.createServer()`.
  Нужен действительный secure сертификат
  <br>
- [`open: boolean | string`](https://vitejs.dev/config/preview-options.html#preview-open "https://vitejs.dev/config/preview-options.html#preview-open") <br>
  Автоматически открывает браузер при запуске сервера
  Если строка то будет использоваться как URL
  Можно настраивать [options](https://github.com/sindresorhus/open#app "https://github.com/sindresorhus/open#app")
  <br>
- [`proxy: Record<string, string | ProxyOptions>`](https://vitejs.dev/config/preview-options.html#preview-proxy "https://vitejs.dev/config/preview-options.html#preview-proxy") <br>
  Правила прокси-сервера для сервера разработки
  перехватывает запросы на ключи и перенаправялет на значения. <br>
  Пример:
  `'/foo': 'http://localhost:4567'`
  `(shorthand: http://localhost:5173/foo -> http://localhost:4567/foo)`
  <br>
- [`cors: boolean | CorsOptions  (default: true)`](https://vitejs.dev/config/preview-options.html#preview-cors "https://vitejs.dev/config/preview-options.html#preview-cors") <br>
  CORS для dev сервера
  подробнее [тут](https://github.com/expressjs/cors#configuration-options "https://github.com/expressjs/cors#configuration-options")
  <br>
- [`headers: OutgoingHttpHeaders`](https://vitejs.dev/config/preview-options.html#preview-headers "https://vitejs.dev/config/preview-options.html#preview-headers") <br>
  Для настройки заголовков, которые приходят с сервера.
  `}`
  <br>

  Опции для билд сборки (в продакшн)
  `build: {`
  <br>

- [`target: string | string[] (default: "modules")`](https://vitejs.dev/config/build-options.html#build-target "https://vitejs.dev/config/build-options.html#build-target") <br>
  В какое окружение собираем билд.
  `"modules"` - для браузеров с нативными модулями ES, нативным динамическим импортом ESM и import.meta

  Список значений - `chrome, deno, edge, firefox, hermes, ie, ios, node, opera, rhino, safari` [подробнее](https://esbuild.github.io/api/#target "https://esbuild.github.io/api/#target") <br>
  <br>

- [`modulePreload: boolean | { polyfill?: boolean, resolveDependencies?: ResolveModulePreloadDependenciesFn }`](https://vitejs.dev/config/build-options.html#build-modulepreload "https://vitejs.dev/config/build-options.html#build-modulepreload")
  `(default: { polyfill: true })`
  <br>
  Полифилы
  <br>

- [`outDir: string (default: "dist")`](https://vitejs.dev/config/build-options.html#build-outdir "https://vitejs.dev/config/build-options.html#build-outdir") <br>
  root для билда
  <br>

- [`assetsDir: string (default: "assets")`](https://vitejs.dev/config/build-options.html#build-assetsdir "https://vitejs.dev/config/build-options.html#build-assetsdir") <br>
  Куда складываем результаты билда (сгенерированные чанки и бандлы)
  путь относительно outDir, т.е. по дефолту `process.cwd()/dist/assets`
  <br>

- [`assetsInlineLimit: number | ((filePath: string, content: Buffer) => boolean | undefined)`](https://vitejs.dev/config/build-options.html#build-assetsinlinelimit "https://vitejs.dev/config/build-options.html#build-assetsinlinelimit")
  `(default: 4096 (4 КБ))` <br>
  Все ресурсы, которые весят меньше этого значения будут переведены в base64 и использованы inline.
  Т.е. для них не будет отдельного запроса во вкладке network
  <br>

- [`cssCodeSplit: boolean - (default: true)`](https://vitejs.dev/config/build-options.html#build-csscodesplit "https://vitejs.dev/config/build-options.html#build-csscodesplit") <br>
  Регулирует разделение css кода. Если false - то весь css будет собран в один файл, включая асинхронные модули (подключенные через lazy)
  Если указать `build.lib` => будет `(default: false)`
  <br>

- [`cssTarget: string | string[] - (default: build.target)`](https://vitejs.dev/config/build-options.html#build-csstarget "https://vitejs.dev/config/build-options.html#build-csstarget") <br>
  Для какого окружения собираем css
  <br>

- [`cssMinify: boolean | 'esbuild' | 'lightningcss' - (default: build.minify)`](https://vitejs.dev/config/build-options.html#build-cssminify "https://vitejs.dev/config/build-options.html#build-cssminify") <br>
  Для регулировки минимизации css
  По умолчанию используется `'esbuild'`
  Если хотим использовать `'lightningcss'` нужно настроить в `css: {lightningcss: {}}`
  Можно отключить параметром `false`
  <br>

- [`cssMinify: boolean | 'inline' | 'hidden' - (default: false)`](https://vitejs.dev/config/build-options.html#build-sourcemap "https://vitejs.dev/config/build-options.html#build-sourcemap") <br>
  Отображение исходных текстов программы (не минимизированных)
  Если `'inline'`, исходная карта будет добавлена ​​к результирующему выходному файлу как URI данных
  `'hidden'` работает аналогично `true` за исключением того, что соответствующие комментарии исходной карты в связанных файлах подавляются.
  <br>

- [`rollupOptions: RollupOptions`](https://vitejs.dev/config/build-options.html#build-rollupoptions "https://vitejs.dev/config/build-options.html#build-rollupoptions") <br>
  [Опции](https://rollupjs.org/configuration-options/ "https://rollupjs.org/configuration-options/") для сборщика rollup, который используется в build сборке
  <br>

- [`commonjsOptions: RollupCommonJSOptions`](https://vitejs.dev/config/build-options.html#build-commonjsoptions "https://vitejs.dev/config/build-options.html#build-commonjsoptions") <br>
  [Опции](https://github.com/rollup/plugins/tree/master/packages/commonjs#options "https://github.com/rollup/plugins/tree/master/packages/commonjs#options") для плагина rollup @rollup/plugin-commonjs
  <br>

- [`lib: false | LibraryOptions`](https://vitejs.dev/config/build-options.html#build-lib "https://vitejs.dev/config/build-options.html#build-lib") <br>
  Параметр [настройки](https://vitejs.dev/guide/build.html#library-mode "https://vitejs.dev/guide/build.html#library-mode"), если нужно собрать не приложение а библиотеку, например ui
  <br>

- [`manifest: boolean | string (default: false)`](https://vitejs.dev/config/build-options.html#build-manifest "https://vitejs.dev/config/build-options.html#build-manifest") <br>
  Для создания манифест файла
  Если `true` то будет создан файл `.vite/manifest.json`
  [подробнее](https://vitejs.dev/guide/backend-integration.html "https://vitejs.dev/guide/backend-integration.html")
  <br>

- [`ssrManifest: boolean | string (default: false)`](https://vitejs.dev/config/build-options.html#build-ssrmanifest "https://vitejs.dev/config/build-options.html#build-ssrmanifest") <br>
  Для создания манифест файла ssr
  [подробнее](https://vitejs.dev/guide/ssr.html "https://vitejs.dev/guide/ssr.html")
  <br>

- [`ssr: boolean | string (default: false)`](https://vitejs.dev/config/build-options.html#build-ssr "https://vitejs.dev/config/build-options.html#build-ssr") <br>
  При использовании ssr
  [подробнее](https://vitejs.dev/guide/ssr.html "https://vitejs.dev/guide/ssr.html")
  <br>

- [`ssrEmitAssets: boolean | string (default: false)`](https://vitejs.dev/config/build-options.html#build-ssremitassets "https://vitejs.dev/config/build-options.html#build-ssremitassets") <br>
  Собирает статику на клиенте. Конфликты с серверной статикой решаются разрабочиками
  <br>

- [`minify: boolean | 'terser' | 'esbuild' (default: 'esbuild' для сборки клиента, false для сборки SSR.)`](https://vitejs.dev/config/build-options.html#build-minify "https://vitejs.dev/config/build-options.html#build-minify") <br>
  Управление минификацией. `'esbuild'` и `'terser'` использует одноименные минификаторы.
  Если стоит значение `'terser'`, нужно установить в dev [зависимость](https://www.npmjs.com/package/terser "https://www.npmjs.com/package/terser")
  <br>

- [`terserOptions: {}`](https://vitejs.dev/config/build-options.html#build-terseroptions "https://vitejs.dev/config/build-options.html#build-terseroptions") <br>
  [Опции](https://terser.org/docs/api-reference/#minify-options "https://terser.org/docs/api-reference/#minify-options") для terser минификатора. Использовать если minify: `'terser'`<br> <br>

- [`write: boolean (default: true)`](https://vitejs.dev/config/build-options.html#build-write "https://vitejs.dev/config/build-options.html#build-write") <br>
  Разрешает или запрещает записывать данные на диск при `build`.
  <br>

- [`emptyOutDir: boolean (default: true)`](https://vitejs.dev/config/build-options.html#build-emptyoutdir "https://vitejs.dev/config/build-options.html#build-emptyoutdir") <br>
  Перед билдом очищаем dist
  <br>

- [`copyPublicDir: boolean (default: true)`](https://vitejs.dev/config/build-options.html#build-copypublicdir "https://vitejs.dev/config/build-options.html#build-copypublicdir") <br>
  По умолчанию Vite копирует файлы из publicDir в outDir
  <br>

- [`reportCompressedSize: boolean (default: true)`](https://vitejs.dev/config/build-options.html#build-reportcompressedsize "https://vitejs.dev/config/build-options.html#build-reportcompressedsize") <br>
  Включить/отключить отчеты о размерах, сжатых gzip
  Сжатие больших выходных файлов может быть медленным, поэтому его отключение может повысить производительность сборки для больших проектов.
  <br>

- [`chunkSizeWarningLimit: boolean (default: true)`](https://vitejs.dev/config/build-options.html#build-chunksizewarninglimit "https://vitejs.dev/config/build-options.html#build-chunksizewarninglimit") <br>
  Выдает предупреждение если файл превышает утстановленный лимит (в КБ).

`}`
