# SEO-аудит plumix.net

Актуально на 2026-10-24. Проверены живой сайт (`curl`), исходники `landing`
(ветка `release`, коммит `2fc8975`) и собранный `dist/`. Частотности и
сложности — экспертная оценка по выдаче: SEO-инструмент (Ahrefs/Semrush) не
подключён, PageSpeed API в день проверки упёрся в квоту. Перед запуском
блога цифры стоит сверить в Ahrefs / Google Search Console.

## 0. Статус исправлений (2026-10-24)

Сделано в `landing` (ветка `release`, не закоммичено и не задеплоено):

- Редирект-цикл: `DirectorySlash Off`, абсолютные `https://` в 301, `*.html` → чистый URL.
  Проверено на локальном Apache 2.4: старый `.htaccess` воспроизводит цикл, новый отдаёт 200 на
  хабы и 301 на `/docs/`, `/docs.html`, `/index.html`.
- Факты: `net10.0`, платформы, версия из одной константы `PLUMIX_VERSION`, роадмап M1–M6 + RC,
  пакеты (добавлены `Plumix.Android`, F#, порты Flutter-пакетов), FAQ и JSON-LD. Пример в
  «Getting started» вызывал несуществующий `PlumixHost.Run` — заменён на `PlumixApplication`;
  все примеры кода в доках и блоге собраны против пакетов с NuGet.
- `/changelog` рендерит `Plumix/CHANGELOG.md` (копия в `content/changelog.md`,
  `npm run sync:changelog`).
- On-page: H1 с ключами, description ≤160, каталог разбит на H2-группы, Material и Cupertino
  расширены до реального состава пакетов (111 виджетов вместо 67).
- Техника: шрифты self-host (`@fontsource`, только латиница), `lastmod` из поля `updated` вместо
  даты сборки, удалены `favicon.jpg` и `placeholder.svg`.
- Блог: `/blog`, 4 поста (кластеры A, C, D + релиз), RSS, `BlogPosting`, OG-картинка на пост,
  посты — отдельные lazy-чанки.

Не сделано — нужно руками:

- Деплой (`push` в `release`) и затем Search Console / Bing Webmaster: отправить sitemap,
  запросить переобход `/docs`, `/controls`, `/netbench`.
- GitHub About / Website / Topics (нет авторизации `gh`).
- Кэш `.js` (`max-age=604800`): статику отдаёт nginx Beget, `.htaccess` на неё не действует —
  настраивается в панели хостинга, если вообще нужно.

## 1. Итог

Фундамент сильный: каждый маршрут пререндерится в статический HTML со своими
title, description, canonical, OG-тегами и JSON-LD; www и http уходят 301,
у 404 честный статус, есть sitemap. **Но три хаба — `/docs`, `/controls`,
`/netbench` (priority 0.9 в sitemap, ссылки на них в шапке каждой страницы) —
сейчас недоступны: бесконечный редирект.** Google их не проиндексирует.

Три главных приоритета:

1. **Починить редиректы** (`DirectorySlash Off`) — 15 минут, критично.
2. **Вычистить устаревшие факты**: на сайте `net8.0`, «только десктоп», v0.5;
   в реальности `net10.0`, хосты Android/iOS/браузер, `0.4.0-beta.2`.
   Инструкция установки из `/docs` сейчас ведёт пользователя к ошибке.
3. **Запустить `/blog`** на той же SSG-схеме. 13 страниц — слишком мало,
   чтобы ранжироваться по чему-то кроме бренда; трафик принесут сравнения
   («Avalonia vs Flutter», «MAUI alternative») и разборы концепций Flutter на C#.

Общая оценка: **technically good, critical bug + content too thin**.

## 2. SSR / пререндер

**Вывод: SSR не нужен — уже есть SSG, и для этого сайта это правильный выбор.**

`scripts/prerender.mjs` собирает клиент (`vite build`), серверный бандл
(`vite build --ssr src/entry-server.tsx`), рендерит каждый маршрут через
`renderToString` + `StaticRouter` и пишет `dist/<route>.html`; `main.tsx`
гидрирует (`hydrateRoot`). Поисковик без JS видит полный текст и мету — это
проверено на `dist/*.html` (450–1100 слов контента на странице). Хостинг —
статика на Beget, Node-процесс не нужен. Настоящий SSR (рендер на запрос)
дал бы только TTFB-штраф и VPS.

Что доработать в текущей схеме:

| Что | Почему | Как |
|---|---|---|
| Маршруты из контента | Сейчас `PRERENDER_ROUTES = Object.keys(ROUTE_SEO)` — таблица ручная, для блога не масштабируется | Генерировать записи `ROUTE_SEO` из frontmatter постов (`import.meta.glob`) |
| `lastmod` | Всем URL ставится дата сборки — для Google это шум, он перестаёт доверять `lastmod` | Брать дату из frontmatter (`updated`) или `git log -1 --format=%cs -- <file>` |
| Один JS-бандл на всё | 317 КБ (95 КБ gzip) гидрируются на каждой странице, включая будущие статьи | `React.lazy` по маршрутам; для статей — ленивые только интерактивные блоки |
| Одна OG-картинка | Все страницы шарятся одинаково | Генерировать OG на пост при сборке (`satori` + `@resvg/resvg-js`) |
| RSS | Нужен для агрегаторов (.NET Weekly, dev.to import, Feedly) | `dist/blog/rss.xml` в том же `prerender.mjs` |

**Когда переезжать на Astro.** Если блог + доки вырастут до 50+ страниц, Astro
даёт из коробки content collections, MDX, RSS, sitemap, нулевой JS на
статьях и React-острова для существующих компонентов. Сейчас переезд — 3–5
дней ради выгоды, которой пока нет; текущую SSG-схему расширить — 1–2 дня.
Рекомендация: **расширить текущую, к Astro вернуться при ~50 страницах**.

## 3. Технический чек-лист

| Проверка | Статус | Детали |
|---|---|---|
| Хабы `/docs`, `/controls`, `/netbench` | **Fail** | Цикл `301 /docs → http://…/docs/ → https://…/docs/ → /docs → …`. `mod_dir` видит каталог `dist/docs/` и добавляет слэш раньше, чем `.htaccess` отдаёт `docs.html`; правило «убрать слэш» возвращает обратно. Плюс каждый hop идёт через `http://`, потому что Apache за nginx не знает о TLS |
| Остальные маршруты | Pass | `/docs/architecture`, `/controls/core`, `/changelog` → 200 |
| `/docs.html`, `/index.html` | Warning | Отдают 200 — дубли. Canonical спасает, но лучше 301 на чистый URL |
| HTTPS, www → apex | Pass | 301 одним хопом |
| 404 | Pass | Настоящий 404 со своей страницей, `noindex` |
| robots.txt, sitemap.xml | Pass / Warning | Есть; `lastmod` = дата сборки у всех URL |
| Canonical, OG, Twitter | Pass | Абсолютные URL, на каждой странице |
| JSON-LD | Warning | WebSite, Organization, SoftwareApplication, FAQPage, BreadcrumbList, TechArticle — хорошо. Но `softwareVersion: "0.5"`, `operatingSystem: "Windows, macOS, Linux"` и ответ FAQ про платформы устарели. FAQ rich results Google показывает только авторитетным сайтам — ценность низкая |
| Gzip, кэш HTML | Pass | HTML 48 КБ → 9 КБ, `no-cache` на HTML |
| Кэш ассетов | Warning | `.js` отдаётся с `max-age=604800` вместо `immutable` на год — nginx Beget перебивает заголовок |
| Шрифты | Warning | Google Fonts с чужого домена: лишний DNS/TLS, риск CLS. Self-host `woff2` из `public/` |
| Мусор в `public/` | Low | `favicon.jpg` (225 КБ) и `placeholder.svg` уезжают в `dist/` |
| `lang`, `hreflang` | Pass | `en`; русской версии нет — `hreflang` пока не нужен |
| Core Web Vitals | Не измерено | Квота PageSpeed API. По косвенным признакам (статический HTML, 95 КБ JS) должно быть зелёным; проверить в Search Console |
| Search Console / Bing Webmaster | Неизвестно | Подтвердить, что сайт добавлен и sitemap отправлен |

## 4. On-page

Title все уникальные, 41–61 символ; description есть везде; H1 один на
странице; alt у изображений есть; перелинковка через шапку и футер.
Проблемы:

| Страница | Проблема | Severity | Исправление |
|---|---|---|---|
| `/docs`, `/controls`, `/netbench` | Недоступны (редирект-цикл) | Critical | См. §3, `DirectorySlash Off` |
| `/docs` | «Plumix targets `net8.0`… on Windows, macOS and Linux» — фреймворк на `net10.0`, есть Android/iOS/браузер | Critical | Обновить требования и установку, добавить раздел про мобильные хосты |
| `/` | Статус «v0.5 · July 2026»; Cupertino описан как «Checkbox, Radio, Switch», хотя в beta.1 портирован весь `cupertino_ui` | High | Сверить с `Plumix/CHANGELOG.md` и `FRAMEWORK_PLAN.md` |
| `/changelog` | Версии v0.1.0…v0.5.0 не совпадают с реальными (`0.4.0-beta.2`) | High | Генерировать страницу из `Plumix/CHANGELOG.md` при сборке |
| `/` (FAQ + JSON-LD) | «Which platforms… Windows, macOS and Linux desktop… net8.0» | High | Обновить ответ: desktop + Android + iOS + browser (с пометкой статуса) |
| `/` | H1 «Flutter's UI model, written in C#» без бренда и без «framework» | Medium | Например «Plumix — Flutter's UI framework, written in C#» |
| `/docs/*` | H1 без ключа: «Getting started», «Stateful widgets», «Layout protocol» | Medium | «Getting started with Plumix», «Stateful widgets in C#», «Layout protocol in C#» |
| `/docs/architecture`, `/docs/layout`, `/controls/*`, `/changelog` | Description 165–182 символа — обрежется | Low | Сократить до ≤160 |
| `/controls/*` | Нет H2: каталог — плоские карточки | Medium | H2 по группам (Layout, Text, Input, Scrolling…) |
| `/controls/cupertino` | 251 слово — thin content | Medium | Расширить после обновления фактов о Cupertino |
| Все | Нет контентных ссылок между страницами, только навигация | Medium | Из доков ссылаться на соседние разделы и виджеты; из будущих постов — на доки |

Вне сайта, но влияет на SEO:

| Где | Проблема | Исправление |
|---|---|---|
| GitHub `Plumix-Net/Plumix` (15 ★) | Пустые About, Website и Topics | Description из `overview.md`, Website `https://plumix.net`, topics: `flutter`, `csharp`, `dotnet`, `ui-framework`, `avalonia`, `cross-platform`, `material-design`, `cupertino` |
| GitHub `Plumix.Packages`, `NetBench` | То же | То же, плюс для NetBench — `load-testing`, `http-benchmark` |
| Выдача по «Flutter for C#» | В топе NuGet-страницы `Plumix 0.1.0-alpha.5` — индекс видит старую версию, сайт не попадает | Сайт должен стать первой ссылкой; README и NuGet `projectUrl` → plumix.net |

## 5. Семантическое ядро

Сайт англоязычный, ядро — EN. Оценки: спрос — по наличию тематических
SERP и обсуждений; сложность — по тому, кто в топе. ★ — где у Plumix
уникальное право на ответ (конкурентов почти нет).

### Кластер A. Бренд и категория «Flutter на C#» — главная, `/docs`

| Ключ | Спрос | Сложность | Интент | Страница |
|---|---|---|---|---|
| flutter for c# ★ | средний | лёгкая | commercial | `/` + пост «Flutter for C# developers in 2026» |
| flutter in c# / flutter .net ★ | средний | лёгкая | commercial | `/` |
| flutter-like ui framework c# ★ | низкий | лёгкая | commercial | `/` |
| c# flutter alternative | низкий | лёгкая | commercial | пост-сравнение |
| plumix, plumix c# | низкий | лёгкая | navigational | `/` |

В выдаче сейчас NuGet Plumix, архивный FlutterSharp, заброшенный
xamarin.flutter — живого конкурента нет.

### Кластер B. Сравнения фреймворков — блог, pillar

| Ключ | Спрос | Сложность | Интент | Страница |
|---|---|---|---|---|
| avalonia vs flutter | средний | средняя | commercial | пост |
| maui vs flutter | высокий | высокая | commercial | пост (угол: «а если хочется Flutter на C#») |
| maui vs avalonia vs uno 2026 | высокий | высокая | commercial | pillar «C# cross-platform UI in 2026» |
| .net maui alternative | средний | средняя | commercial | пост |
| best c# cross-platform ui framework | средний | высокая | commercial | pillar |
| avalonia alternative | низкий | лёгкая | commercial | пост |

Конкуренты в выдаче — блоги Syncfusion, startdebugging.net, dev.to,
tekrevol, alternativeto. Профиль ссылок у них средний, пробиться можно
глубиной и кодом, которого у них нет.

### Кластер C. Портирование и миграция с Flutter

| Ключ | Спрос | Сложность | Интент | Страница |
|---|---|---|---|---|
| port flutter app to c# ★ | низкий | лёгкая | informational | гайд |
| dart to c# / convert dart to c# | средний | средняя | informational | гайд (конвертер-лендинги от docuwriter в топе) |
| statefulwidget in c# / setstate c# ★ | низкий | лёгкая | informational | `/docs/state` + пост |
| flutter developer learn c# / .net | низкий | лёгкая | informational | пост |
| flutter widget to c# ★ | низкий | лёгкая | informational | пошаговый гайд |

### Кластер D. Концепции Flutter (трафик Flutter-разработчиков)

| Ключ | Спрос | Сложность | Интент | Страница |
|---|---|---|---|---|
| widget element renderobject / flutter three trees | средний | средняя | informational | `/docs/architecture` + длинный пост |
| constraints go down sizes go up | средний | средняя | informational | `/docs/layout` + пост |
| flutter custom renderobject | низкий | средняя | informational | пост с C#- и Dart-кодом |
| flutter rendering pipeline explained | средний | средняя | informational | пост |
| boxconstraints explained | низкий | средняя | informational | пост |

Конкуренты — docs.flutter.dev и Medium. Ниша — объяснять механику с двух
сторон (Dart → C#): это уникальный формат.

### Кластер E. Code-first UI на C#

| Ключ | Спрос | Сложность | Интент | Страница |
|---|---|---|---|---|
| c# ui without xaml | низкий | лёгкая | informational | пост |
| declarative ui c# | низкий | лёгкая | informational | пост (MauiReactor, Comet, Fabulous) |
| mvu c# / elmish ui .net | низкий | лёгкая | informational | пост про `Plumix.Elmish` / F# |
| c# hot reload preserve state | низкий | лёгкая | informational | пост |
| skia ui framework c# | низкий | лёгкая | informational | пост |

### Кластер F. Платформы

| Ключ | Спрос | Сложность | Интент | Страница |
|---|---|---|---|---|
| c# android app without maui ★ | низкий | лёгкая | informational | гайд + `/docs/android` |
| c# ios app skia | низкий | лёгкая | informational | гайд |
| .net 10 cross-platform ui | средний | средняя | commercial | pillar |
| c# linux desktop app gui | средний | средняя | informational | пост |
| c# webassembly ui framework | низкий | средняя | commercial | пост |

### Кластер G. Пакеты (порты Flutter-пакетов)

| Ключ | Спрос | Сложность | Интент | Страница |
|---|---|---|---|---|
| bloc pattern c# / flutter bloc .net ★ | низкий | лёгкая | informational | пост + `/packages/bloc` |
| provider c# inheritedwidget ★ | низкий | лёгкая | informational | пост + `/packages/provider` |
| slang i18n c# | низкий | лёгкая | informational | `/packages/slang` |

### Кластер H. Каталог виджетов (programmatic long-tail)

`<Flutter widget> in C#` — по странице на виджет: «ListView in C#»,
«GestureDetector C#», «Material 3 AppBar C# desktop», «CupertinoPicker C#».
Каждый запрос — единицы показов, но их сотни, конкуренции ноль, и это
ровно та боль, которую решает Plumix. Шаблон: что делает виджет, C#-пример
рядом с Dart-оригиналом, ссылка на исходник порта, отличия. Генерировать из
данных, которые уже есть в `Controls.tsx`.

### Кластер I. NetBench (отдельная аудитория: бэкенд/QA)

| Ключ | Спрос | Сложность | Интент | Страница |
|---|---|---|---|---|
| http load testing app android / iphone ★ | низкий | лёгкая | transactional | `/netbench` |
| load test api from phone | низкий | лёгкая | transactional | `/netbench` |
| k6 / jmeter alternative gui | средний | средняя | commercial | пост |
| p50 p95 p99 latency explained | средний | средняя | informational | пост (вход в воронку NetBench) |
| rps vs concurrent users | низкий | средняя | informational | пост |

Мобильного нагрузочника в выдаче нет вообще — только десктоп и облака
(alternativeto: Loadster, Locust, Gatling, hey). Как только приложение
появится в сторах, `/netbench` может забрать этот запрос целиком.

## 6. Блог: структура и первые статьи

**Структура URL:** `/blog` (лента), `/blog/<slug>` (пост), `/blog/rss.xml`.
Теги не делать до 30+ постов — пустые таксономии только размывают вес.
Посты — `content/blog/*.mdx` с frontmatter
(`title`, `description`, `date`, `updated`, `cluster`, `canonical`);
`ROUTE_SEO` и sitemap собираются из frontmatter. JSON-LD: `BlogPosting` с
`author`, `datePublished`, `dateModified`, `image`. В конце каждого поста —
блок «Try Plumix» со ссылкой на `/docs` и 2–3 ссылки на связанные посты.

**Первая волна, 12 постов — по 1 в неделю, Q4 2026 – Q1 2027:**

| # | Пост | Кластер | Приоритет | Объём |
|---|---|---|---|---|
| 1 | Flutter for C# developers in 2026: what actually exists | A | high | 1 день |
| 2 | Avalonia vs Flutter vs MAUI vs Uno in 2026 — from a Flutter developer's seat (pillar) | B | high | 2–3 дня |
| 3 | Porting a Flutter widget to C#, line by line (реальный виджет из `flutter-src`) | C | high | 1–2 дня |
| 4 | Widget, Element, RenderObject: Flutter's three trees explained with C# | D | high | 1 день |
| 5 | Constraints go down, sizes go up — the layout protocol in Dart and C# | D | medium | 1 день |
| 6 | Building an Android app in C# without MAUI | F | high | 1 день |
| 7 | StatefulWidget and setState in C#: lifecycle and hot reload | C | medium | полдня |
| 8 | Bloc and Provider for .NET: porting Flutter state management | G | medium | 1 день |
| 9 | How we built NetBench: Avalonia on desktop, Plumix on mobile | I + B | medium | 1 день |
| 10 | p50, p95, p99: reading latency percentiles in a load test | I | medium | полдня |
| 11 | Writing a custom RenderObject in C# | D | medium | 1 день |
| 12 | Plumix 0.4 beta: full Cupertino port, Android and iOS hosts (анонс релиза) | A | high | полдня |

Посты релизов (#12) — на каждый тег `v*.*.*`: это свежесть сайта и
поводы для рассылок и соцсетей.

**Дистрибуция.** Каждый пост — кросспостинг на dev.to / Hashnode с
`canonical_url` на plumix.net; русская адаптация — на Хабр (там уже есть
статья о Plumix; canonical Хабр не поддерживает, поэтому текст
переписывать, а не копировать). Каналы: r/dotnet, r/csharp, r/FlutterDev
(посты 2–5), Show HN к релизу 0.4/0.5, .NET Weekly, awesome-dotnet,
awesome-avalonia.

## 7. Конкуренты

Прямых «Flutter на C#» живых нет, поэтому сравнение — с сайтами, которые
забирают выдачу по категории. Колонки конкурентов — по общему знанию, сайты
не краулились; объёмы страниц и ссылок сверить в Ahrefs.

| Измерение | plumix.net | avaloniaui.net | platform.uno | MauiReactor (gitbook) |
|---|---|---|---|---|
| Индексируемых страниц | 13 (3 хаба недоступны) | сотни (доки, блог, кейсы) | сотни (доки, блог, сравнения) | десятки (доки) |
| Блог | нет | есть, регулярный | есть, частый | нет (посты в devblogs Microsoft) |
| Страницы-сравнения | нет | есть | есть (vs MAUI, vs Flutter) | нет |
| Ссылочный профиль | слабый: NuGet, 1 статья Хабра, GitHub 15 ★ | сильный (29k ★) | сильный | средний (650+ ★, devblogs) |
| Техническая база | SSG + JSON-LD — на уровне лидеров | хорошо | хорошо | средне |
| Уникальная позиция | архитектура Flutter на C# — ни у кого | XAML, WPF-наследник | WinUI XAML везде | MVU поверх MAUI |
| **Победитель** | техника — ничья; контент и ссылки — конкуренты | | | |

Вывод: соревноваться по «best C# UI framework» в лоб — долго. Быстрее
занять пустые ниши (кластеры A, C, D, H, I), а сравнения (B) писать как
pillar для ссылок и соцсетей.

## 8. План действий

### Quick wins (эта неделя)

| Что | Эффект | Время | Зависимости |
|---|---|---|---|
| `DirectorySlash Off` в `public/.htaccess`; правило «убрать слэш» — с абсолютным `https://plumix.net/$1`; 301 с `*.html` на чистый URL | high | 30 мин | деплой `release` |
| Обновить факты: `net10.0`, платформы, версия `0.4.0-beta.2`, Cupertino — в `/`, `/docs`, FAQ и JSON-LD | high | 2 ч | сверка с `FRAMEWORK_PLAN.md` |
| GitHub About / Website / Topics для трёх репо | medium | 15 мин | — |
| Search Console + Bing Webmaster, отправить sitemap, запросить переобход хабов | high | 30 мин | после фикса редиректов |
| H1 с ключами в доках, description ≤160 | low | 30 мин | — |
| Удалить `favicon.jpg`, `placeholder.svg`; self-host шрифтов | low | 1 ч | — |

### Стратегические (квартал)

| Что | Эффект | Время | Зависимости |
|---|---|---|---|
| `/blog` на текущем SSG: MDX, frontmatter → `ROUTE_SEO`, RSS, `BlogPosting`, OG на пост | high | 1–2 дня | — |
| 12 постов первой волны, 1/неделю, с кросспостингом | high | ~12 дней работы за квартал | блог |
| `/changelog` из `Plumix/CHANGELOG.md` при сборке | medium | полдня | — |
| Доки: Android, iOS, Browser, Material, Cupertino, пакеты — каждая страница под свой ключ | high | 3–5 дней | — |
| Программные страницы виджетов `/controls/<widget>` | medium–high | 2–3 дня на шаблон | данные `Controls.tsx` |
| Ссылки: awesome-списки, Show HN, Хабр, .NET Weekly, доклад на DotNext / .NET Conf | high | постоянно | контент |
| Lazy-роуты, `lastmod` из git | low | полдня | — |
| Переезд на Astro | medium | 3–5 дней | только при ~50+ страницах |
