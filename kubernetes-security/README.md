# Домашнее задание №5

Выполненные шаги в `HW5`:
1. Созданы каталоги для тасков 
```
mkdir task01 task02 task03
```
2. В рамках `task01` cозданы - `service-account.yaml`, `rolebinding.yaml`,`service-account-no.yaml`
3. Выполнено:
```
sudo kubectl apply -f service-account.yaml
sudo kubectl apply -f rolebinding.yaml
sudo kubectl apply -f service-account-no.yaml
```
4. Выполнена проверка создания аккаунтов:
```
sudo kubectl get serviceaccount

NAME      SECRETS   AGE
bob       0         3m8s
dave      0         24s
default   0         24h
```
5. В рамках `task02` созданы - `namespace-prometheus.yaml`, `service-account.yaml`, `role.yaml`, `rolebinding.yaml`
6. Выполнено:
```
sudo kubectl apply -f namespace-prometheus.yaml -f service-account.yaml -f role.yaml -f rolebinding.yaml
```
7. Выполнена проверка:
```
sudo kubectl get sa -n prometheus

NAME      SECRETS   AGE
carol     0         39s
default   0         39s
```
8. 