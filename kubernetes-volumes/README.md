# Домашнее задание №4

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
### Создание и использование PVC в Kubernetes
9. Создан `my-pv.yaml`
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data
``` 
10. Создан `my-pvc.yaml`
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 500Mi
```
11. Создан pod (модуль) `my-pod.yaml`
```
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
    - name: mypod
      image: nginx
      volumeMounts:
        - mountPath: "/app/data"
          name: mypv
  volumes:
    - name: mypv
      persistentVolumeClaim:
        claimName: my-pvc
```
12. Выполняем:
```
sudo kubectl apply -f my-pv.yaml -f my-pvc.yaml -f my-pod.yaml
```
13. Подключаемся к Pod my-pod: `sudo kubectl exec -it my-pod -- bash`
14. Создаем файл `data.txt` в каталоге `/app/data`:
```
root@my-pod:/app/data# echo "Kazantsev27" > data.txt
root@my-pod:/app/data# cat data.txt
Kazantsev27
root@my-pod:/app/data#
```
15. Удалил существующий Pod - my-pod и далее создан новый Pod: 
```
sudo kubectl delete po my-pod
sudo kubectl apply -f my-pod.yaml
```
16. Просмотр содердимого `/app/data`
```
root@my-pod:/# cd /app/data/
root@my-pod:/app/data# ls -l
total 4
-rw-r--r-- 1 root root 12 Dec 10 16:52 data.txt
root@my-pod:/app/data# cat data.txt
Kazantsev27
root@my-pod:/app/data#
```
17. 