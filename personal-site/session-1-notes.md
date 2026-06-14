# Создание личного сайта — Сессия 1 и 2
**Дата:** 14 июня 2026

---

## 🎯 Что сделали в итоге
- Профессиональный GitHub профиль
- Живой сайт на `personal-site-eight-iota-37.vercel.app`
- Автодеплой: каждый `git push` = обновление сайта

---

## 📌 Часть 1 — GitHub профиль

### Шаг 1. Сменили username
**Что:** `eeexxro` → `osadullaev`
**Зачем:** Единый бренд везде — GitHub, домен, почта, LinkedIn
**Где:** GitHub → Settings → Account → Change username

### Шаг 2. Создали Profile README
**Что:** Репозиторий с именем точь-в-точь как username → `osadullaev/osadullaev`
**Зачем:** GitHub магия — README этого репо автоматически показывается на главной странице профиля как визитная карточка
**Содержимое:** Имя, должность, badges со стеком, timeline карьеры, контакты

### Шаг 3. Создали репозитории
| Репо | Зачем |
|------|-------|
| `osadullaev` | Profile README — визитка |
| `personal-site` | Исходник сайта |
| `devops-notes` | База знаний, заметки из Obsidian |

### Шаг 4. Структура devops-notes
**Что:** Создали папку `courses/README.md`
**Зачем:** Хранить заметки по курсам внутри одного репо
**Как создать папку на GitHub:** Add file → имя файла написать как `courses/README.md` → GitHub сам создаст папку

### Шаг 5. Удалили лишние репо
**Что:** Удалили форки `Mastering-GitLab-12` и `pure-bash-bible`
**Зачем:** Форки чужих книг не говорят о тебе — только занимают место

### Шаг 6. Запинили репо
**Что:** Customize your pins → выбрали 3 репо
**Зачем:** На главной профиля показываются только запиненные — остальные скрыты

---

## 📌 Часть 2 — Локальная среда и сайт

### Шаг 7. Установили VS Code
**Что:** Редактор кода
**Зачем:** Здесь пишем весь сайт, встроенный терминал

### Шаг 8. Установили Node.js
**Что:** Движок для запуска JavaScript
**Зачем:** Без него не работает npm и Astro
**Команда:**
```bash
winget install OpenJS.NodeJS.LTS
```
**Проверка:**
```bash
node --version  # должно показать v24.x.x
```

### Шаг 9. Настроили PowerShell
**Проблема:** `npm не может быть загружен, выполнение скриптов отключено`
**Причина:** Windows блокирует запуск скриптов по умолчанию
**Решение:**
```bash
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```

### Шаг 10. Установили Astro
**Что:** Фреймворк для сайта
**Зачем:** Генерирует быстрый статичный HTML, идеален для блога + портфолио

**Аналогия из DevOps:**
```
Node.js  =  Docker (движок)
npm      =  apt/yum (пакетный менеджер)
Astro    =  nginx (конкретное приложение)
```

**Команда:**
```bash
cd C:\Users\eeexx
npm create astro@latest personal-site -- --template minimal
```

**Проблема 1:** `personal-site is not empty` — папка уже существовала
**Решение:** Удалили папку и создали заново
```bash
Remove-Item -Recurse -Force personal-site
npm create astro@latest personal-site -- --template minimal
```

**Проблема 2:** `Dependencies failed to install` — зависимости не установились автоматически
**Причина:** Конфликт путей (папка была в OneDrive с кириллицей в пути)
**Решение:** Создали проект в `C:\Users\eeexx\` напрямую, а не в папке с русскими буквами
```bash
cd personal-site
npm install
```

### Шаг 11. Запустили локальный сервер
```bash
npm run dev
```
**Результат:** Сайт доступен на `http://localhost:4321`
**Зачем:** Видим сайт локально до деплоя — как staging среда

### Шаг 12. Структура Astro проекта
```
personal-site/
├── public/          → статичные файлы (картинки, favicon)
├── src/
│   └── pages/
│       └── index.astro  → главная страница = localhost:4321
├── node_modules/    → пакеты (не трогать, не в git)
├── astro.config.mjs → конфиг
└── package.json     → список зависимостей
```
**Правило:** работаешь только в `src/` и `public/`

### Шаг 13. Написали первую страницу
**Файл:** `src/pages/index.astro`
**Что добавили:** Тёмный фон, градиентное имя, теги со стеком
**Hot Reload:** Сохранил файл → браузер обновился автоматически

---

## 📌 Часть 3 — Git и деплой

### Шаг 14. Установили Git
**Проблема:** `git не распознано как имя командлета`
**Причина:** Git не был установлен на Windows
**Решение:**
```bash
winget install Git.Git
# перезапустить терминал после установки
```

### Шаг 15. Настроили Git профиль
**Проблема:** `Author identity unknown — Please tell me who you are`
**Причина:** Git не знает от чьего имени делать коммиты
**Решение:**
```bash
git config --global user.email "eeexxro@gmail.com"
git config --global user.name "Odiljon Sadullayev"
```

### Шаг 16. Запушили код на GitHub
```bash
git init                    # инициализируем git в папке
git add .                   # добавляем все файлы
git commit -m "init: first version of personal site"  # сохраняем
git branch -M main          # называем ветку main
git remote add origin https://github.com/osadullaev/personal-site.git  # указываем репо
git push -u origin main     # пушим
```

**Проблема:** `rejected — remote contains work that you do not have`
**Причина:** В GitHub репо уже был README, локально его не было — конфликт
**Решение 1 (pull):** не сработало из-за конфликта в README
**Решение 2 (force push):**
```bash
git push -u origin main --force
```
**Когда использовать --force:** Только когда уверен что локальная версия правильная и хочешь перезаписать удалённую. На личных репо — нормально.

### Шаг 17. Задеплоили на Vercel
**Что:** Vercel — платформа для хостинга
**Зачем:** Бесплатно, глобальный CDN, автодеплой при каждом git push

**Процесс:**
1. vercel.com → Sign Up with GitHub
2. Dashboard → Add New Project
3. Выбрали `osadullaev/personal-site`
4. Vercel сам определил что это Astro проект
5. Нажали Deploy

**Результат:** `personal-site-eight-iota-37.vercel.app` — живой сайт!

---

## 🔄 Как теперь обновлять сайт

```bash
# 1. Открыть VS Code в папке personal-site
# 2. Внести изменения в src/pages/index.astro
# 3. Сохранить Ctrl+S
# 4. В терминале:
git add .
git commit -m "описание что изменил"
git push
# 5. Vercel автоматически задеплоит за ~30 секунд
```

---

## ⏭️ Следующие шаги

1. Купить домен `osadullaev.dev` (Cloudflare Registrar ~$10/год)
2. Подключить домен к Vercel
3. Добавить секции: About, Projects, Blog
4. Создать Layout компонент (шапка + футер)
5. Перенести Obsidian заметки в `devops-notes`
