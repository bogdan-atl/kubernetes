Внешний доступ к сервисам важен для предоставления доступа к вашим приложениям пользователям или другим системам, которые находятся вне кластера Kubernetes.

Сначала рассмотрим основные типы сервисов в Kubernetes, которые можно использовать для предоставления внешнего доступа:

ClusterIP (тип по умолчанию) - сервис доступен только внутри кластера через внутренний IP-адрес.
NodePort - сервис доступен на статическом порту, назначенном на каждом узле кластера.
LoadBalancer - сервис доступен через внешний балансировщик нагрузки, предоставляемый облачным провайдером.
ExternalName - сервис представляет собой ссылку на внешний ресурс вне кластера.
NodePort
Для начала рассмотрим сервис NodePort. Вот пример манифеста сервиса NodePort:
<pre><code>
apiVersion: v1
kind: Service
metadata:
  name: my-nodeport-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
      nodePort: 30080
  type: NodePort</code></pre>


  В этом примере сервис my-nodeport-service направляет трафик на поды с меткой app: my-app. Трафик с порта 80 сервиса перенаправляется на порт 8080 подов. NodePort назначен порт 30080. Теперь ваш сервис доступен на всех узлах кластера на порту 30080.

LoadBalancer
Теперь давайте рассмотрим сервис LoadBalancer. Вот пример манифеста сервиса LoadBalancer:
<pre><code>
apiVersion: v1
kind: Service
metadata:
  name: my-loadbalancer-service
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
  type: LoadBalancer</code></pre>


В этом примере сервис my-loadbalancer-service аналогичен сервису NodePort, но с типом LoadBalancer. Облачный провайдер автоматически создаст внешний балансировщик нагрузки и направит трафик на поды с меткой app: my-app. Трафик с порта 80 сервиса будет перенаправляться на порт 8080 подов.

Ingress
Для более сложных сценариев, таких как настройка SSL/TLS, маршрутизация на основе пути или поддоменов, вы можетеиспользовать Ingress. Ingress - это объект Kubernetes, который позволяет управлять внешним доступом к сервисам в кластере, обычно через HTTP. Ingress может предоставлять более сложную маршрутизацию и тонкую настройку, чем NodePort или LoadBalancer.

Прежде чем использовать Ingress, вам нужно установить и настроить Ingress контроллер, такой как Nginx Ingress Controller или другие сторонние варианты.

Вот пример манифеста Ingress:
<pre><code>
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: my-app.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-app-service
            port:
              number: 80</code></pre>


В этом примере мы определили Ingress с именем my-ingress, который направляет весь трафик для домена my-app.example.com на сервис my-app-service на порт 80. Вы можете определить дополнительные правила маршрутизации для других доменов или путей, если это необходимо.

Важно отметить, что Ingress контроллеры могут иметь разные реализации и возможности, так что убедитесь, что вы ознакомились с документацией для вашего выбранного Ingress контроллера, чтобы использовать все его функции.

Вот краткое изложение того, что мы рассмотрели в этом уроке:

NodePort - предоставляет доступ к сервисам на статическом порту на всех узлах кластера.
LoadBalancer - предоставляет доступ к сервисам через внешний балансировщик нагрузки, предоставляемый облачным провайдером.
Ingress - предоставляет более сложные сценарии маршрутизации и доступа к сервисам, такие как SSL/TLS, маршрутизация на основе пути или поддоменов.