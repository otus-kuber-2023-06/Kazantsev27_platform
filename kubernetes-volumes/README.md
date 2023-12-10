# Домашниt задание №4

Выполненные шаги в `HW4`:
1. Запуск kind - `kind create cluster`
2. Не актуально: `export KUBECONFIG="$(kind get kubeconfig-path --name="kind")"`
3. Создаем `minio-statefulset.yaml`
4. Разворачиваем `sudo kubectl apply -f minio-statefulset.yaml`
```
NAME      READY   STATUS    RESTARTS   AGE
minio-0   1/1     Running   0          78s

NAME           STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
data-minio-0   Bound    pvc-b046a4b0-bec4-40da-ac20-b80cccd90996   10Gi       RWO            standard       86s

NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   REASON   AGE
pvc-b046a4b0-bec4-40da-ac20-b80cccd90996   10Gi       RWO            Delete           Bound    default/data-minio-0   standard                84s
```
5. Применение Headless Service - создаем `minio-headlessservice.yaml`
```
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)    AGE
kubernetes   ClusterIP   10.96.0.1    <none>        443/TCP    4m21s
minio        ClusterIP   None         <none>        9000/TCP   13s
```
6. Проверяем ресурсы k8s:
```
NAME    READY   AGE
minio   1/1     4m37s

NAME      READY   STATUS    RESTARTS   AGE
minio-0   1/1     Running   0          5m11s

NAME           STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
data-minio-0   Bound    pvc-b046a4b0-bec4-40da-ac20-b80cccd90996   10Gi       RWO            standard       5m20s

NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                  STORAGECLASS   REASON   AGE
pvc-b046a4b0-bec4-40da-ac20-b80cccd90996   10Gi       RWO            Delete           Bound    default/data-minio-0   standard                5m22s
```
7. Создан секрет `minio-secret.yaml`
```
apiVersion: v1
kind: Secret
metadata:
  name: minio-secret
type: kubernetes.io/basic-auth
stringData:
  username: minio
  password: minio123
```
8. Исправлена конфигурация `minio-statefulset.yaml`