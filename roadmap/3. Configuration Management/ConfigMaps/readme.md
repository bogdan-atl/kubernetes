ConfigMaps

ConfigMaps используются для хранения конфигурационных данных в виде пар ключ-значение. Они позволяют разделять конфигурацию приложения от образа контейнера, что делает приложение более гибким и удобным для развертывания в различных средах.

Создание ConfigMap

Давайте создадим простой ConfigMap, который содержит некоторые настройки для нашего приложения. Создайте файл app-config.yaml со следующим содержимым:
<pre><code>
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  db-host: "localhost"
  db-port: "5432"
  log-level: "info"</code></pre>

Здесь мы определяем ConfigMap с именем app-config, который содержит три ключа: db-host, db-port и log-level. Чтобы создать ConfigMap в кластере, выполните следующую команду:

kubectl apply -f app-config.yaml

Теперь у нас есть ConfigMap, который может быть использован нашим приложением.

Использование ConfigMap в подах

Для использования ConfigMap в наших подах, нам нужно сначала добавить его в спецификацию пода. Давайте создадим новый файл app-pod.yaml со следующим содержимым:
<pre><code>
apiVersion: v1
kind: Pod
metadata:
  name: app
spec:
  containers:
  - name: app-container
    image: my-app-image:latest
    env:
    - name: DB_HOST
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: db-host
    - name: DB_PORT
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: db-port
    - name: LOG_LEVEL
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: log-level</code></pre>

В этом файле мы определяем один контейнер с именем app-container, использующий образ my-app-image:latest. Затем мы добавляем три переменных среды, значения которых извлекаются из нашего ConfigMap. Чтобы развернуть этот под, выполните следующую команду:

kubectl apply -f app-pod.yaml

Теперь у нас есть под, который использует настройки из ConfigMap.

Обновление ConfigMap

Если вам нужно обновить значения в ConfigMap, вы можете просто изменить файл app-config.yaml и применить изменения с помощью команды kubectl apply. Однако, изменения не будут автоматически применены к работающим подам. Вам нужно будет либо пересоздать поды, либо использовать другие подходы, такие как "rolling update", для обновления подов с новыми настройками.

