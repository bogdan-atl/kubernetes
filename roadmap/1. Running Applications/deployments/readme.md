Deployment (развертывание) – это высокоуровневый объект в Kubernetes, который позволяет управлять набором однородных подов (pods) и обеспечивает обновление и масштабирование. Deployment создает и управляет ReplicaSet'ами, которые в свою очередь управляют подами.

Основные возможности Deployment:

Обновление подов: с помощью Deployment вы можете обновлять поды, развернутые на кластере, без простоя или потери доступности сервиса.
Роллинг-апдейты: это стратегия обновления, при которой поды обновляются путем постепенного замещения старых подов новыми.
Откат обновлений: если что-то пошло не так во время обновления, вы можете откатиться к предыдущей версии Deployment.
Масштабирование: с помощью Deployment можно легко масштабировать количество подов вверх или вниз, основываясь на нагрузке или других факторах.
Пример манифеста Deployment:
<pre><code>

apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80</code></pre>

Конечно, я рад помочь! Deployment (развертывание) – это высокоуровневый объект в Kubernetes, который позволяет управлять набором однородных подов (pods) и обеспечивает обновление и масштабирование. Deployment создает и управляет ReplicaSet'ами, которые в свою очередь управляют подами.

Основные возможности Deployment:

Обновление подов: с помощью Deployment вы можете обновлять поды, развернутые на кластере, без простоя или потери доступности сервиса.
Роллинг-апдейты: это стратегия обновления, при которой поды обновляются путем постепенного замещения старых подов новыми.
Откат обновлений: если что-то пошло не так во время обновления, вы можете откатиться к предыдущей версии Deployment.
Масштабирование: с помощью Deployment можно легко масштабировать количество подов вверх или вниз, основываясь на нагрузке или других факторах.
Пример манифеста Deployment:

<pre><code>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80</code></pre>

В этом примере создается Deployment с 3 репликами подов, запущенных на базе образа nginx:1.14.2. Здесь:

replicas: количество реплик подов, которые должны быть запущены.
selector: определяет, какие поды относятся к данному Deployment. В данном случае мы выбираем все поды с меткой app: nginx.
template: определяет спецификацию пода, которая будет использована для создания новых подов.
Для управления Deployment вы можете использовать различные команды kubectl. Например:

kubectl get deployments: для просмотра всех развертываний в кластере.
kubectl scale deployment my-nginx-deployment --replicas=5: для масштабирования развертывания до 5 реплик.
kubectl rollout status deployment my-nginx-deployment: для проверки статуса обновления развертывания.
kubectl rollout undo deployment my-nginx-deployment: для отката к предыдущей версии развертывания.
Deployment – это мощный и гибкий инструмент управления жизненным циклом подов в Kubernetes. Используя его, вы можете обновлять, масштабировать и откатывать приложения в вашем кластере без простоя или потери доступности. Он обеспечивает автоматическое восстановление подов в случае их отказа или удаления, что делает управление приложениями в Kubernetes надежным и удобным.

Дополнительные примеры с использованием Deployment:

Обновление образа контейнера:
<pre><code>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.16.1
        ports:
        - containerPort: 80</code></pre>

Здесь мы обновляем образ контейнера с nginx:1.14.2 на nginx:1.16.1. После применения этого манифеста, Deployment автоматически начнет обновление подов с использованием стратегии роллинг-апдейта.

Изменение ресурсов:
<pre><code>
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        resources:
          limits:
            cpu: 200m
            memory: 256Mi
          requests:
            cpu: 100m
            memory: 128Mi
        ports:
        - containerPort: 80</code></pre>

В этом примере мы изменяем ресурсы, доступные для контейнера. Мы устанавливаем ограничения на использование CPU и памяти, а также указываем запрашиваемые ресурсы.
