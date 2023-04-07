<html>
<head>
<title>Урок 5: Автомасштабирование в Kubernetes (lesson-5-autoscaling)</title>
</head>
<body>
<h1>Урок 5: Автомасштабирование в Kubernetes (lesson-5-autoscaling)</h1>
<p>В этом уроке мы рассмотрим возможности автомасштабирования в Kubernetes с использованием Horizontal Pod Autoscaler (HPA).</p>
<h2>Horizontal Pod Autoscaler (HPA)</h2>
<p>Horizontal Pod Autoscaler (HPA) - это механизм, позволяющий автоматически масштабировать количество подов в репликасете или деплойменте на основе наблюдаемой нагрузки, такой как использование ЦПУ или памяти. HPA может увеличивать или уменьшать количество подов в зависимости от текущей нагрузки, что позволяет оптимизировать использование ресурсов в кластере.</p>
<p>Пример манифеста для Horizontal Pod Autoscaler:</p>
<pre><code>apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: my-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 2
  maxReplicas: 10
  targetCPUUtilizationPercentage: 50</code></pre>
<p>В этом примере создается HPA для Deployment с именем my-deployment. Количество подов будет масштабироваться в диапазоне от 2 до 10, если уровень использования ЦПУ превысит 50%.</p>
<h2>Метрики для автомасштабирования</h2>
<p>По умолчанию HPA использует метрики использования ЦПУ и памяти для принятия решений об автомасштабировании. Однако можно настроить HPA для использования пользовательских метрик или внешних метрик, таких как запросы в секунду, обработанные приложением. Это делается с помощью API Custom Metrics и External Metrics.</p>
<h2>Автомасштабирование на основе пользовательских метрик</h2>
<p>Пример манифеста для автомасштабирования на основе пользовательской метрики:</p>
<pre><code>apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: custom-metric-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Pods
    pods:
      metric:
        name: my_custom_metric
      target:
        type: AverageValue
        averageValue: 50</code></pre>
<p>В этом примере HPA будет масштабировать количество подов в зависимости от пользовательской метрики my_custom_metric, стремясь к среднему значению 50 для каждого пода в деплойменте.</p>

<h2>Автомасштабирование на основе внешних метрик</h2>
<p>Пример манифеста для автомасштабирования на основе внешней метрики:</p>
<pre><code>apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: external-metric-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: External
    external:
      metric:
        name: my_external_metric
      target:
        type: Value
        value: 100</code></pre>
<p>В этом примере HPA будет масштабировать количество подов в зависимости от внешней метрики my_external_metric, стремясь к значению 100 для всего деплоймента.</p>
<h2>Автомасштабирование на основе нескольких метрик</h2>
<p>HPA также может использовать несколько метрик для определения масштабирования. В следующем примере HPA будет использовать комбинацию метрик использования ЦПУ, памяти и пользовательской метрики:</p>
<pre><code>apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: multiple-metrics-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-deployment
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 50
  - type: Resource
    resource:
      name: memory
      target:
        type: AverageValue
        averageValue: 500Mi
  - type: Pods
    pods:
      metric:
        name: my_custom_metric
      target:
        type: AverageValue
        averageValue: 50</code></pre>
<p>HPA будет учитывать все указанные метрики для принятия решений об автомасштабировании.</p>
<h2>Полезные ссылки</h2>
<p>Дополнительные примеры, документацию и руководства по использованию автомасштабирования в Kubernetes можно найти в материалах курса, а также на официальном сайте Kubernetes: <a href="https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/">https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/</a></p>
</body>
</html>




