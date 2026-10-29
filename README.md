# docker-images

Репозиторий с Dockerfile'ами образов. Сборка выполняется компонентом `components/build/buildchain@1` (hadolint → buildkit) через шаблон [.gitlab-ci/build-multiple-job.yml](.gitlab-ci/build-multiple-job.yml).

## Образы

| Каталог | Образ |
|---|---|
| `ansible` | ansible |
| `cr-cleanup-python` | cr-cleanup-python |
| `helm` | helm |
| `helmfile` | helmfile |
| `terraform` | terraform |

## Добавление образа
1. Создать каталог по имени будущего образа
2. Добавить `Dockerfile` + файлы для билда при необходимости. Каталог образа является build context, поэтому пути в `COPY`/`ADD` указываются относительно него
3. Добавить образ в `.gitlab-ci.yml`:
   ```yaml
   include:
     - local: .gitlab-ci/build-multiple-job.yml
       inputs:
         image_name: <имя каталога>
   ```

## Пайплайн
Для каждого образа создаются два джоба:
1. `hadolint` — линт Dockerfile, падение блокирует сборку
2. `build-<image_name>` — сборка и push в `$CI_REGISTRY_IMAGE/<image_name>`

Теги образа: `<ref-slug>-<pipeline-id>` и `<ref-slug>-latest`.

Запуск:
- вручную из веб интерфейса
- автоматически для коммитов в ветки; в MR джобы запускаются вручную
