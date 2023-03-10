Resources
1. Создаем deployment с ресурсами

Перейдите в директорию: 4.resources

cd ~/slurm/practice/2.application-abstractions/4.resources/

kubectl apply -f deployment-with-resources.yaml

Смотрим что получилось
kubectl get pod

Должны увидеть примерно следующий вывод:

NAME                             READY     STATUS              RESTARTS   AGE
my-deployment-57fff9c845-2qv5l   0/1       ContainerCreating   0          1s
my-deployment-57fff9c845-h8bbw   0/1       ContainerCreating   0          1s

2. Увеличиваем количество ресурсов для нашего деплоймента

Для этого выполним команду

kubectl patch deployment my-deployment --patch '{"spec":{"template":{"spec":{"containers":[{"name":"nginx","resources":{"requests":{"cpu":"10"},"limits":{"cpu":"10"}}}]}}}}'

Смотрим что получилось
kubectl get pod

Находим pod в состоянии Pending

NAME                             READY   STATUS    RESTARTS   AGE
my-deployment-68684546b9-gm894   0/1     Pending   0          37s
my-deployment-9ddd794f9-jvff6    1/1     Running   0          51s
my-deployment-9ddd794f9-ztvq9    1/1     Running   0          51s

Смотрим, почему pod не может запуститься

kubectl describe po my-deployment-845d88fdcf-9bd29

Видим в Events предупреждения:

Events:
  Type     Reason            Age               From               Message
  ----     ------            ----              ----               -------
  Warning  FailedScheduling  87s   default-scheduler  0/8 nodes are available: 8 Insufficient cpu.

3. Чистим за собой кластер

kubectl delete deployment --all
