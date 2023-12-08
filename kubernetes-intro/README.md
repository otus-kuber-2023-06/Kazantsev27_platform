# Домашнее задание №1

Выполненные шаги в `HW1`:
1. Установлен `kubectl` и `minickube`
1. Выполнен запуск `minikube`.
1. Написан `Dockerfile` и помещен в каталог `kubernetes-intro/web`
1. Собран образ и помещен в реестр `Docker Hub`.
1. Реализован манифест `web-pod.yaml` для создания pod web и помещен в директорию `kubernetes-intro`.
1. Протестирована команда `kubectl describe`.
1. Добавлен в pod `init контейнер`, генерирующий страницу `index.html`.
1. Описан `volumeMounts`.
1. Применен обновленный манифест `web-pod.yaml`.
1. Выполнена проверка работы приложения: http://localhost:8080/index.html
1. Склонирован репозиторий [frontend](https://github.com/GoogleCloudPlatform/microservices-demo), собран образ и помещен в реестр `Docker Hub`.
1. Реализован манифест `frontend-pod.yaml`
1. Реализован манифест `frontend-pod-healthy.yaml`, где были добавлены необходимые переменные окружения.