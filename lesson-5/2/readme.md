#  задание про Тейнты и Лейблы
```
cd ../2
```

## Пометим ноду тейнтом  

```sh
kubectl get nodes
kubectl taint node cl14607bcn1714k4v3im-ixif app=blue:NoSchedule
kubectl get node cl14607bcn1714k4v3im-ixif -o json | jq .spec.taints
```


Задеплоим приложение и что на нашей ноде его нет
Окно 1
```sh
watch kubectl get po -o wide -n demo-ns -o=custom-columns=NAME:.metadata.name,node:.spec.nodeName
```

Окно 2
```sh
kubectl apply -f 01-dep-toleration.yaml
```

Дадим поду toleration и применим.

```sh
kubectl apply -f 01-dep-toleration.yaml
```

Наше приложение появилось на нужной ноде и других

## Пометим туже ноду лейблом

```sh
kubectl get nodes
kubectl label node cl14607bcn1714k4v3im-ixif app=blue
kubectl get node cl14607bcn1714k4v3im-ixif -o json | jq .metadata.labels.app
```

Добавим nodeSelector нашему сервису

```sh
kubectl deploy nginx  -n demo-ns
kubectl apply -f 01-dep-nodeSelector.yaml
```

Наше приложение деплоится только на нужной нам  ноде


Закончим лабу

Удалим лейблы и тейнты

```
kubectl taint node cl14607bcn1714k4v3im-ixif app-
kubectl label node cl14607bcn1714k4v3im-ixif app-
```


Удалим NS

```
kubectl delete ns demo-ns
```
