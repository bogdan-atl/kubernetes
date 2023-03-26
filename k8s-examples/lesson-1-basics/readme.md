<!DOCTYPE html>
<html>
<head>
	<title>README.md</title>
</head>
<body>
	<h1>Введение в Kubernetes (k8s):</h1>
	<p>Kubernetes - это система оркестрации контейнеров с открытым исходным кодом, предназначенная для автоматизации развертывания, масштабирования и управления приложениями, запущенными в контейнерах.</p>
	<h2>Основные понятия:</h2>
	<ul>
		<li>Node (узел): физический или виртуальный сервер, на котором работают контейнеры.</li>
		<li>Pod (под): наименьшая и простейшая единица в Kubernetes, состоящая из одного или нескольких контейнеров.</li>
		<li>Service (сервис): абстракция, представляющая стабильный IP-адрес и порт, по которым можно обратиться к подам.</li>
	</ul>
	<h2>Установка и настройка kubectl:</h2>
	<p>Kubectl - это командная строка Kubernetes, которая позволяет выполнять команды и управлять кластером Kubernetes. Вам необходимо установить kubectl на вашем локальном компьютере для взаимодействия с кластером.</p>
	<h3>Установка kubectl (Linux):</h3>
	<pre>
		<code>
			curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
			chmod +x kubectl
			sudo mv kubectl /usr/local/bin/
		</code>
	</pre>
	<h3>Установка kubectl (macOS):</h3>
	<pre>
		<code>
			brew install kubectl
		</code>
	</pre>
	<h3>Установка kubectl (Windows):</h3>
	<pre>
		<code>
			choco install kubernetes-cli
		</code>
	</pre>
	<h2>Подключение к кластеру Kubernetes:</h2>
	<p>Чтобы использовать kubectl, вам необходимо подключиться к кластеру Kubernetes. Если вы используете облачный провайдер, например, Google Kubernetes Engine (GKE) или Amazon Elastic Kubernetes Service (EKS), следуйте инструкциям по настройке доступа к вашему кластеру.</p>
	<h2>Развертывание простого приложения:</h2>
	<p>Для развертывания приложения в Kubernetes, создайте файл my-app-deployment.yaml со следующим содержимым:</p>
	<pre>
		<code>
			apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 2
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
        image: nginx:latest
        ports:
        - containerPort: 80
	</code>
</pre>
<p>Этот файл определяет развертывание, которое создает два пода с контейнерами, запускающими образ Nginx.</p>
<p>Примените файл развертывания, выполнив следующую команду:</p>
<pre>
	<code>
		kubectl apply -f my-app-deployment.yaml
	</code>
</pre>
<p>Создание сервиса:</p>
<p>Чтобы предоставить доступ к вашему приложению извне, вам нужно создать сервис. Создайте файл my-app-service.yaml со следующим содержимым:</p>
<pre>
	<code>
		apiVersion: v1
		kind: Service
		metadata:
		  name: my-app-service
		spec:
		  selector:
		    app: my-app
		  ports:
		    - protocol: TCP
		      port: 80
		      targetPort: 80
		  type: LoadBalancer
	</code>
</pre>
<p>Примените файл сервиса, выполнив следующую команду:</p>
<pre>
	<code>
		kubectl apply -f my-app-service.yaml
	</code>
</pre>
<p>Проверка статуса развертывания и сервиса:</p>
<p>Чтобы проверить статус развертывания и сервиса, выполните следующие команды:</p>
<pre>
	<code>
		kubectl get deployments
		kubectl get services
	</code>
</pre>
<p>Это покажет информацию о вашем развертывании и сервисе, включая количество реплик и IP-адреса.</p>
<h2>Обновление приложения:</h2>
<p>Если вы хотите обновить образ контейнера или изменить количество реплик, вы можете обновить файл my-app-deployment.yaml и повторно применить его:</p>
<pre>
	<code>
		kubectl apply -f my-app-deployment.yaml
	</code>
</pre>
<p>Kubernetes автоматически обновит развертывание с новыми параметрами.</p>
<h2>Удаление развертывания и сервиса:</h2>
<p>Если вам необходимо удалить развертывание и сервис, выполните следующие команды:</p>
<pre>
	<code>
		kubectl delete -f my-app-deployment.yaml
		kubectl delete -f my-app-service.yaml
	</code>
</pre>
<h2>Работа с kubectl:</h2>
<p>Вот несколько основных команд kubectl, которые могут быть полезны при работе с Kubernetes:</p>
<ul>
	<li>Получение информации о ресурсах:</li>
	<pre>
  kubectl get nodes
  kubectl get pods
  kubectl get services
  kubectl get deployments
  </pre>
	<li>Получение дополнительной информации о ресурсе:</li>
	<pre>
		<code>
			kubectl describe pod &lt;pod_name&gt;
			kubectl describe service &lt;service_name&gt;
			kubectl describe deployment &lt;deployment_name&gt;
		</code>
	</pre>
	<li>Просмотр логов контейнера:</li>
	<pre>
		<code>
			kubectl logs &lt;pod_name&gt; -c &lt;container_name&gt;
		</code>
	</pre>
	<li>Запуск команды внутри контейнера:</li>
	<pre>
		<code>
			kubectl exec -it &lt;pod_name&gt; -c &lt;container_name&gt; -- &lt;command&gt;
		</code>
	</pre>
	<li>Масштабирование развертывания:</li>
	<pre>
		<code>
			kubectl scale deployment &lt;deployment_name&gt; --replicas=&lt;number_of_replicas&gt;
		</code>
	</pre>
	<li>Обновление образа контейнера в развертывании:</li>
	<pre>
		<code>
			kubectl set image deployment/&lt;deployment_name&gt; &lt;container_name&gt;=&lt;new_image&gt;
		</code>
	</pre>
	<li>Получение yaml-конфигурации ресурса:</li>
	<pre>
		<code>
			kubectl get deployment &lt;deployment_name&gt; -o yaml
		</code>
	</pre>
</ul>
</body>
</html>

	
