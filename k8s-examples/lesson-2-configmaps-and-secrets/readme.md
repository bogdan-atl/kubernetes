<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Урок 2: ConfigMaps и Secrets</title>
</head>
<body>
	<h1>Цель урока: научиться работать с ConfigMaps и Secrets для хранения конфигураций и секретных данных.</h1>
	<h2>Создание ConfigMap</h2>
	<p>Создайте файл <code>configmap.yaml</code> с содержимым:</p>
	<pre><code>apiVersion: v1
kind: ConfigMap
metadata:
  name: my-app-config
data:
  app-config.json: |
    {
      "apiUrl": "https://api.example.com",
      "logLevel": "info"
    }</code></pre>
	<p>Запустите команду для создания ConfigMap:</p>
	<pre><code>kubectl apply -f configmap.yaml</code></pre>
	<h2>Создание Secret</h2>
	<p>Создайте файл <code>secret.yaml</code> с содержимым:</p>
	<pre><code>apiVersion: v1
kind: Secret
metadata:
  name: my-app-secret
type: Opaque
data:
  api-key: c2VjcmV0LWFwaS1rZXkK</code></pre>
	<p>Запустите команду для создания Secret:</p>
	<pre><code>kubectl apply -f secret.yaml</code></pre>
	<h2>Использование ConfigMap и Secret в Deployment</h2>
	<p>Обновите файл <code>my-app-deployment.yaml</code> из урока 1, добавив <code>envFrom</code> и <code>env</code> в контейнер:</p>
	<pre><code>apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 1 # количество реплик пода, которые должны быть созданы
  selector:
    matchLabels:
      app: my-app # метки для выбора подов, управляемых этим развертыванием
  template:
    metadata:
      labels:
        app: my-app # метки, применяемые к поду
    spec:
      containers:
        - name: my-app # имя контейнера
          image: my-app-image:latest # образ, который должен быть запущен в контейнере
          ports:
            - containerPort: 3000 # порт, который должен быть открыт в контейнере
          envFrom:
            - configMapRef:
                name: my-app-config # ссылка на ConfigMap, из которого будут получены переменные окружения
          env:
            - name: API_KEY # имя переменной окружения
              valueFrom:
                secretKeyRef:
                  name: my-app-secret # ссылка на Secret, из которого будет получено значение переменной окружения
                  key: api-key # имя ключа в Secret, соответствующего переменной окружения</code></pre>
	<p>Запустите команду для обновления Deployment:</p>
	<pre><code>kubectl apply -f my-app-deployment.yaml</code></pre>
	<p>Теперь ваше приложение будет использовать конфигурации из ConfigMap и Secret.</p>
<h2>Обновление ConfigMap и Secret</h2>
<p>Чтобы обновить ConfigMap или Secret, измените соответствующий файл и выполните команду <code>kubectl apply</code>. Затем выполните команду для обновления Deployment, чтобы применить изменения:</p>
<pre><code>kubectl rollout restart deployment my-app</code></pre>
<h2>Удаление ConfigMap и Secret</h2>
<p>Чтобы удалить ConfigMap или Secret, выполните следующие команды:</p>
<pre><code>kubectl delete configmap my-app-config
kubectl delete secret my-app-secret</code></pre>
<p>Обратите внимание, что при удалении ConfigMap или Secret, приложение, использующее их, может столкнуться с проблемами, поскольку ожидаемые конфигурации и секретные данные будут отсутствовать. Важно следить за зависимостями между ресурсами и избегать удаления тех, которые активно используются.</p>
<p>В этом уроке вы научились создавать, использовать и управлять ConfigMaps и Secrets в Kubernetes. Эти механизмы позволяют хранить конфигурационные данные и секретные ключи безопасно и эффективно. На следующем уроке мы рассмотрим работу с хранилищами в Kubernetes и создание Persistent Volumes для сохранения данных между перезапусками контейнеров.</p>

</body>
</html>
