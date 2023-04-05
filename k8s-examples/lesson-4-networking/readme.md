<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Урок 4: Сетевые возможности в Kubernetes</title>
</head>
<body>
    <h1>Урок 4: Сетевые возможности в Kubernetes (lesson-4-networking)</h1>
    <p>В этом уроке мы рассмотрим основные сетевые возможности в Kubernetes, такие как Ingress и Service.</p>
    <h2>Ingress</h2>
    <p>Ingress - это объект Kubernetes, который предоставляет средства для управления доступом к сервисам в кластере с помощью HTTP и HTTPS. Ingress работает как "входная точка" для внешнего трафика и позволяет определить правила маршрутизации на основе URI и хоста.</p>
    <p>Пример манифеста Ingress:</p>
    <pre><code>apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: example.com
    http:
      paths:
      - path: /app1
        pathType: Prefix
        backend:
          service:
            name: app1-service
            port:
              number: 80
  - host: example.com
    http:
      paths:
      - path: /app2
        pathType: Prefix
        backend:
          service:
            name: app2-service
            port:
              number: 80</code></pre>
              <h2>Service</h2>
<p>Service - это объект Kubernetes, который абстрагирует собой доступ к группе Pods и обеспечивает балансировку нагрузки между ними. Он может быть использован для обеспечения доступа к Pods как внутри кластера, так и снаружи.</p>
<p>Пример манифеста Service:</p>
<pre><code>apiVersion: v1
<h2>Service</h2>
<p>Service - это объект Kubernetes, который абстрагирует собой доступ к группе Pods и обеспечивает балансировку нагрузки между ними. Он может быть использован для обеспечения доступа к Pods как внутри кластера, так и снаружи.</p>
<p>Пример манифеста Service:</p>
<pre><code>apiVersion: v1
kind: Service
metadata:
name: app1-service
spec:
selector:
app: app1
ports:
- protocol: TCP
port: 80
targetPort: 8080
type: LoadBalancer</code></pre>
<p>В этом примере создается Service с типом LoadBalancer, который будет балансировать трафик между Pods с меткой app: app1 и предоставлять доступ к ним на порту 80 снаружи кластера.</p>
<h2>Network Policies</h2>
<p>Сетевые политики (Network Policies) позволяют ограничивать сетевое взаимодействие между различными Pods в кластере. Это может быть полезно для повышения безопасности и изоляции компонентов в кластере.</p>
<<p>Пример манифеста NetworkPolicy:</p>
<pre><code>apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: app1-network-policy
spec:
  podSelector:
    matchLabels:
      app: app1
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: app2
    ports:
    - protocol: TCP
      port: 8080</code></pre>
<p>В этом примере мы создаем сетевую политику для Pods с меткой app: app1, разрешая входящий трафик только от Pods с меткой app: app2 на порту 8080.</p>
<h2>Сервисы с типом NodePort</h2>
<p>Еще одним способом предоставления доступа к сервисам снаружи кластера является использование типа NodePort для сервисов. В этом случае, сервис будет доступен на определенном порту на всех узлах кластера.</p>
<p>Пример манифеста Service с типом NodePort:</p>
<pre><code>apiVersion: v1
kind: Service
metadata:
  name: app1-nodeport-service
spec:
  selector:
    app: app1
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30080
  type: NodePort</code></pre>
<p>В этом примере, сервис будет доступен на порту 30080 на всех узлах кластера.</p>
<h2>Управление трафиком с помощью Istio</h2>
<p>Istio представляет собой сервисный меш, который обеспечивает набор расширенных сетевых возможностей, таких как мониторинг, безопасность, маршрутизация трафика и многое другое. Используя Istio, вы можете управлять сетевым трафиком в вашем кластере Kubernetes на более высоком уровне, чем с использованием встроенных средств.</p>
<p>Дополнительные примеры, документацию и руководства по использованию сетевых возможностей в Kubernetes можно найти в материалах курса, а также на официальном сайте Kubernetes (https://kubernetes.io/docs/concepts/services-networking/).</p>
</body>
</html>
