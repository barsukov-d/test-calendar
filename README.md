# test-calendar

## Project setup

```
pnpm install
```

### Compiles and hot-reloads for development

```
pnpm run serve
```

### Compiles and minifies for production

```
pnpm run build
```

### Lints and fixes files

```
pnpm run lint
```

### Customize configuration

See [Configuration Reference](https://cli.vuejs.org/config/).

# Сборка проекта

pnpm run build

# Создание ветки gh-pages (если еще не создана)

git checkout --orphan gh-pages

# Копирование файлов из dist в корень репозитория

cp -r dist/\* .

# Добавление и коммит изменений

git add .
git commit -m "Deploy to GitHub Pages"

# Пуш изменений на ветку gh-pages

git push origin gh-pages --force
