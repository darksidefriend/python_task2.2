MkDocs Static Site with CI/CD
==============================

Описание проекта
----------------

Данный репозиторий содержит **статический сайт**, собранный с использованием **MkDocs** и кастомной темы,\
а также настроенный **CI/CD-pipeline** для автоматической сборки и деплоя на **GitHub Pages** через **GitHub Actions**.

Проект предназначен для демонстрации:

-   работы с MkDocs,

-   кастомизации темы,

-   сборки фронтенд-ресурсов (CSS/JS),

-   автоматического деплоя при push в ветку `main`.

* * * * *

Project Structure
-----------------

python_task2.2/\
├── .github/\
│   └── workflows/\
│       └── deploy.yml\
├── docs/\
│   └── index.md\
├── src/\
│   ├── css/\
│   │   └── style.css\
│   └── js/\
│       └── script.js\
├── theme/\
│   ├── main.html\
│   ├── css/\
│   │   └── style.css\
│   └── js/\
│       └── script.js\
├── mkdocs.yml\
├── package.json\
├── package-lock.json\
├── postcss.config.js\
├── README.md\
└── .gitignore

* * * * *

Используемые технологии
-----------------

-   **Python 3.11**

-   **MkDocs**

-   **Node.js 20**

-   **npm**

-   **PostCSS**

-   **GitHub Actions**

-   **GitHub Pages**

-   **YAML**

* * * * *

Конфигурация MkDocs
--------------------

### `mkdocs.yml`

site_name: Лабораторная работа Савельева Даниила\
site_description: Пример сайта на MkDocs с кастомной темой и CI/CD\
site_author: Савельев Даниил

theme:\
  name: null\
  custom_dir: theme/

nav:\
  - Главная: index.md

extra:\
  author:\
    name: Савельев Даниил\
    group: Р4210

### Configuration Explanation

-   `site_name`, `site_description`, `site_author` --- метаданные сайта

-   `theme.custom_dir` --- подключение кастомной темы

-   `nav` --- навигация сайта

-   `extra` --- дополнительные пользовательские данные

* * * * *

Frontend билд
---------------------

Проект использует **Node.js** для сборки и обработки фронтенд-ресурсов:

-   `src/css` → стили

-   `src/js` → JavaScript

-   `postcss.config.js` → конфигурация PostCSS

-   `npm run build`:

    -   сборка CSS и JS,

    -   минификация HTML,

    -   валидация,

    -   сборка MkDocs-сайта в папку `site/`

* * * * *

CI/CD пайплайн
--------------

### GitHub Actions Workflow

Файл: `.github/workflows/deploy.yml`

name: Build and Deploy to GitHub Pages

on:\
  push:\
    branches: [ main ]

permissions:\
  contents: write

jobs:\
  build-and-deploy:\
    runs-on: ubuntu-latest\
    steps:\
      - name: Checkout\
        uses: actions/checkout@v4

      - name: Setup Python\
        uses: actions/setup-python@v5\
        with:\
          python-version: '3.11'

      - name: Install MkDocs and dependencies\
        run: |\
          pip install mkdocs

      - name: Setup Node.js\
        uses: actions/setup-node@v4\
        with:\
          node-version: '20'\
          cache: 'npm'

      - name: Install Node dependencies\
        run: npm ci

      - name: Build frontend (CSS/JS) and MkDocs site, minify HTML, validate\
        run: npm run build

      - name: Deploy to GitHub Pages\
        uses: peaceiris/actions-gh-pages@v4\
        with:\
          github_token: ${{ secrets.GITHUB_TOKEN }}\
          publish_dir: ./site\
          publish_branch: gh-pages

* * * * *

Сборка и стадии деплоя
---------------------------

### 1\. Source Checkout

Клонирование репозитория в CI-окружение.

### 2\. Python Environment Setup

Установка Python 3.11 и MkDocs.

### 3\. Node.js Environment Setup

Установка Node.js 20 и npm-зависимостей.

### 4\. Project Build

Выполнение команды:

npm run build

На данном этапе:

-   собираются CSS и JS,

-   применяется PostCSS,

-   собирается сайт MkDocs,

-   результат помещается в каталог `site/`.

### 5\. Deployment

Содержимое `site/` автоматически публикуется в ветку `gh-pages`.

* * * * *

Local Development
-----------------

### Install dependencies

pip install mkdocs\
npm install

### Запускаем MkDocs локально

mkdocs serve

После запуска сайт будет доступен по адресу:

http://127.0.0.1:8000

* * * * *

Результат
------

В результате выполнения проекта:

-   создан статический сайт на MkDocs,

-   реализована кастомная тема,

-   настроена сборка фронтенд-ресурсов,

-   автоматизирован CI/CD-процесс,

-   сайт автоматически публикуется на GitHub Pages.

* * * * *

Заключение
----------

Проект демонстрирует полный цикл разработки и развёртывания статического сайта:\
от локальной разработки до автоматического деплоя с использованием GitHub Actions и YAML-конфигураций.