<!DOCTYPE html>
<html lang="ru">
<head>
<meta charset="UTF-8">
<title>Урок 6: Обновления с минимальными прерываниями (Rolling Updates) в Kubernetes</title>
</head>
<body>
<h1>Урок 6: Обновления с минимальными прерываниями (Rolling Updates) в Kubernetes (lesson-6-rolling-updates)</h1>
<p>В этом уроке мы рассмотрим обновления с минимальными прерываниями (Rolling Updates) и Deployment в Kubernetes.</p>
<h2>Deployment</h2>
<p>Deployment - это объект Kubernetes, который предоставляет декларативное обновление и масштабирование для приложений. Deployment управляет ReplicaSet, который в свою очередь управляет Pods, гарантируя, что определенное количество их экземпляров всегда доступно и работает.</p>
<p>Пример манифеста Deployment:</p>
<pre><code>apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image:v1
        ports:
        - containerPort: 8080</code></pre>
<p>В этом примере создается Deployment с 3 репликами, каждая из которых имеет контейнер с образом my-image:v1.</p>
<h2>Rolling Updates</h2>
<p>Rolling Update - это процесс пошагового обновления приложения с минимальными прерываниями работы. Вместо остановки и обновления всех экземпляров одновременно, Kubernetes заменяет их по одному, гарантируя, что определенное количество реплик всегда доступно и работает. Rolling Update является стратегией обновления по умолчанию для Deployment в Kubernetes.</p>
<p>Пример обновления Deployment с использованием Rolling Update:</p>
<pre><code>kubectl set image deployment/my-deployment my-container=my-image:v2</code></pre>
<p>В этом примере мы обновляем Deployment с образом my-image:v1 на my-image:v2. Kubernetes выполнит пошаговое обновление всех реплик, заменяя их на новую версию.</p>
<h2>Управление стратегией Rolling Update</h2>
<p>Можно управлять параметрами Rolling Update в манифесте Deployment. Вот пример с настройками для стратегии обновления:</p>
<pre><code>apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image:v1
        ports:
        - containerPort: 8080
strategy:
type: RollingUpdate
rollingUpdate:
maxSurge: 1
maxUnavailable: 0
</code></pre>

<p>В этом примере мы указали стратегию обновления <code>RollingUpdate</code> и установили параметры <code>maxSurge</code> и <code>maxUnavailable</code>.</p>
<ul>
  <li><code>maxSurge</code> - максимальное количество реплик, которые могут быть добавлены к общему числу реплик во время обновления. Может быть указано в процентах или абсолютными значениями (например, "1" или "25%").</li>
  <li><code>maxUnavailable</code> - максимальное количество реплик, которые могут быть недоступны во время обновления. Может быть указано в процентах или абсолютными значениями (например, "0" или "25%").</li>
</ul>
<p>В нашем примере <code>maxSurge</code> равно 1, что означает, что во время обновления может быть добавлена только одна реплика сверх общего числа реплик. <code>maxUnavailable</code> равно 0, что означает, что все реплики должны быть доступны во время обновления.</p>
<p>Более подробную информацию о Rolling Updates, Deployment и других аспектах работы с Kubernetes можно найти в материалах курса, а также на официальном сайте Kubernetes (https://kubernetes.io/docs/concepts/workloads/controllers/deployment/).</p>
</body>
</html>
