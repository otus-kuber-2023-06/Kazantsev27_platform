# Домашние задание №3

Выполненные шаги в `HW3`:
1. Добавлен `readinessProbe` в `web-pod.yaml`
1. Проверка состояния пода `kubectl get pod/web`
1. Проверка `Conditions`
1. Добавлен `livenessProbe` в `web-pod.yaml`
1. Создан `web-deploy.yaml`, удален старый под `kubectl delete pod/web --grace-period=0 --force`
1. Запущен `web-deploy.yaml` - `kubectl apply -f web-deploy.yaml`
```
web-6cd4659bc7-csj47   0/1     Running   0             78s
web-6cd4659bc7-q252k   0/1     Running   0             78s
web-6cd4659bc7-zjw87   0/1     Running   0             78s
```
1. Внесены исправления и далее применены изменения `kubectl apply -f web-deploy.yaml`
```
web-6d77b6bd4f-69mln   1/1     Running   0             19s
web-6d77b6bd4f-tx9b8   1/1     Running   0             15s
web-6d77b6bd4f-zzm9d   1/1     Running   0             11s
``` 
1. 
