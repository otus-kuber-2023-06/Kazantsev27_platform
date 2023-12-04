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
1. Добавлен блок `strategy`
    ```
    strategy:
        type: RollingUpdate
        rollingUpdate:
        maxUnavailable: 0
        maxSurge: 100%
    ```
1. Установлен `kubespy` - `go install github.com/pulumi/kubespy@latest`
1. Выполнено `kubespy trace deploy web`
    ```
    [ADDED apps/v1/Deployment]  default/web
        Rolling out Deployment revision 2
        ✅ Deployment is currently available
        ✅ Rollout successful: new ReplicaSet marked 'available'

    ROLLOUT STATUS:
    - [Current rollout | Revision 2] [ADDED]  default/web-6d77b6bd4f
        ✅ ReplicaSet is available [3 Pods available of a 3 minimum]
        - [Ready] web-6d77b6bd4f-zzm9d
        - [Ready] web-6d77b6bd4f-tx9b8
        - [Ready] web-6d77b6bd4f-69mln
    ```
1. Создан `web-svc-cip.yaml` и запущен `kubectl apply -f web-svc-cip.yaml` и проверено `kubectl get svc`
1. Подключение в ВМ Minikube и выполнение проверок.
1. Включение IPVS. `kubectl --namespace kube-system edit configmap/kube-proxy`, редактирование в vi.
1. `kube-proxy --cleanup` are depricated.
1. Очищены все правила `iptables` - `iptables-restore iptables.cleanup`.
1. Перезапущен minikube - `minikube start --extra-config=kube-proxy.proxy-mode=ipvs`
1. Выплнена команда `kubectl get service -o yaml`
1. ping кластерного IP.
1. Выполнено `kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.12/config/manifests/metallb-native.yaml`
1. Выполнение `kubectl create secret generic -n metallb-system memberlist --from-literal=secretkey="$(openssl rand -base64 128)"` выдало `error: failed to create secret secrets "memberlist" already exists`. Уже сущесвтует.
1. Убедился, что созданы нужные объекты `kubectl --namespace metallb-system get all`
    ```
    NAME                              READY   STATUS    RESTARTS   AGE
    pod/controller-586bfc6b59-n2hls   1/1     Running   0          103s
    pod/speaker-vws4w                 1/1     Running   0          103s

    NAME                      TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
    service/webhook-service   ClusterIP   10.106.32.17   <none>        443/TCP   103s

    NAME                     DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR            AGE
    daemonset.apps/speaker   1         1         1       1            1           kubernetes.io/os=linux   103s

    NAME                         READY   UP-TO-DATE   AVAILABLE   AGE
    deployment.apps/controller   1/1     1            1           103s

    NAME                                    DESIRED   CURRENT   READY   AGE
    replicaset.apps/controller-586bfc6b59   1         1         1       104s
    ```
1. Создан `metallb-cr.yaml` и `metallb-l2.yaml`
1. Сделана копия `web-svc-cip.yaml` в `web-svc-lb.yaml`
1. Запущены манифесты.
1. Просмотр логов пода-контроллера MetalLB `kubectl -n metallb-system logs pod/controller-586bfc6b59-n2hls`
1. Добавлен маршрут ` sudo ip route add 172.17.255.0/24 via 192.168.49.2`
1. Выполнено `kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/baremetal/deploy.yaml`
1. Ищем легкие пути `minikube addons enable ingress` - протестил.
1. Создан `nginx-lb.yaml`
1. Посмотр IP `kubectl describe -n ingress-nginx po ingress-nginx-controller-6cc5ccb977-4p25d`
1. Создан `web-svc-headless.yaml`
    ```
    Name:              web-svc
    Namespace:         default
    Labels:            <none>
    Annotations:       <none>
    Selector:          app=web
    Type:              ClusterIP
    IP Family Policy:  SingleStack
    IP Families:       IPv4
    IP:                None
    IPs:               None
    Port:              <unset>  80/TCP
    TargetPort:        8000/TCP
    Endpoints:         10.244.0.100:8000,10.244.0.101:8000,10.244.0.102:8000
    Session Affinity:  None
    Events:            <none>
    ```
1. Проверена доступность страницы.