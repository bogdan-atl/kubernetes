<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>Инструкция по использованию PVC и StorageClass в Kubernetes</title>
</head>
<body>
	<h1>Инструкция по использованию PVC и StorageClass в Kubernetes</h1>
	<p>В этой инструкции мы рассмотрим, как использовать PersistentVolumeClaim (PVC) и StorageClass в Kubernetes для управления хранилищем.</p>
	<h2>Определение StorageClass:</h2>
	<p>StorageClass - это абстракция, которая определяет, какое хранилище должно быть использовано для PersistentVolume (PV). В манифесте StorageClass мы можем определить различные параметры, такие как тип хранилища, доступность и пропускную способность.</p>
	<pre>
apiVersion: storage.k8s.io/v1  # Определяем версию API для работы с хранилищами
kind: StorageClass  # Тип объекта - StorageClass
metadata:
  name: slow  # Указываем имя для StorageClass
provisioner: kubernetes.io/no-provisioner  # Определяем провайдера хранилища. Здесь используется no-provisioner, что означает, что этот StorageClass не будет создавать PV автоматически.
reclaimPolicy: Retain  # Определяем политику восстановления PV, если PVC был удален. Здесь используется Retain, что означает, что PV и данные на нем будут сохранены после удаления PVC.
volumeBindingMode: Immediate  # Определяем режим привязки объемов - Immediate. Это означает, что PVC будет немедленно привязан к PV.
    </pre>
	<p>В этом манифесте мы определяем StorageClass с именем "slow", который использует хранилище типа "slow" и доступен в зоне "us-west1". Мы также указываем, что данный StorageClass не предоставляет динамическое выделение хранилища, то есть не будет создавать PV автоматически.</p>
	<h2>Определение PersistentVolume:</h2>
<p>PersistentVolume (PV) - это физический ресурс, который используется для хранения данных в Kubernetes. PV может быть создан заранее, или динамически с помощью StorageClass.</p>
<pre>
apiVersion: v1  # Определяем версию API для работы с Kubernetes-объектами
kind: PersistentVolume  # Тип объекта - PersistentVolume
metadata:
  name: pv-demo  # Указываем имя для PersistentVolume
spec:
  capacity:
    storage: 1Gi  # Определяем ёмкость хранилища - 1 гигабайт
  accessModes:
    - ReadWriteOnce  # Указываем доступность хранилища - чтение и запись
  persistentVolumeReclaimPolicy: Retain  # Определяем политику восстановления PV, если PVC был удален. Здесь используется Retain, что означает, что PV и данные на нем будут сохранены после удаления PVC.
  storageClassName: slow  # Определяем класс хранилища, который будет использоваться для PV
  mountOptions:
    - hard  # Определяем тип монтирования - жесткое монтирование
    - nfsvers=4.1  # Определяем версию протокола NFS
  nfs:
    path: /data  # Определяем путь до директории на NFS-сервере
    server: nfs.example.com  # Определяем адрес NFS-сервера

</pre>
<p>В этом манифесте мы определяем PersistentVolume с именем "pv-demo" и ёмкостью 1 гигабайт, с доступом в режиме чтения и записи, и с классом хранилища slow. Мы также указываем, что данный PV может быть монтирован только в одном Pod одновременно, и что данные на PV должны быть сохранены после удаления PVC.</p>
<h2>Определение PersistentVolumeClaim:</h2>
<p>PersistentVolumeClaim (PVC) - это запрос на ресурс PV. PVC определяет, какой PV должен быть использован и как много места нужно для хранения данных.</p>
<pre>
apiVersion: v1  # Определяем версию API для работы с Kubernetes-объектами
kind: PersistentVolumeClaim  # Тип объекта - PersistentVolumeClaim
metadata:
  name: pvc-demo  # Указываем имя для PersistentVolumeClaim
spec:
  accessModes:
    - ReadWriteOnce  # Указываем доступность хранилища - чтение и запись
  resources:
    requests:
      storage: 500Mi  # Определяем требуемую ёмкость хранилища - 500 мегабайт
  storageClassName: slow  # Определяем класс хранилища, который будет использоваться для PVC

</pre>
<p>В этом манифесте мы определяем PersistentVolumeClaim с именем "pvc-demo" и запросом на 500 мегабайт хранилища с классом slow. Мы также указываем, что PV может быть монтирован только в одном Pod одновременно.</p>

<h2>Применение манифестов:</h2>
<p>Чтобы применить манифесты, мы можем использовать команду kubectl apply:</p>
<pre>
kubectl apply -f storageclass.yaml
kubectl apply -f persistentvolume.yaml
kubectl apply -f persistentvolumeclaim.yaml
</pre>
<p>После применения манифестов, мы можем проверить статус PVC и PV с помощью команды kubectl get:</p>
<pre>
kubectl get pvc
kubectl get pv
</pre>
<p>Также мы можем использовать PVC в манифесте Pod, чтобы монтировать хранилище в контейнер:</p>
<pre>
apiVersion: v1  # Определяем версию API для работы с Kubernetes-объектами
kind: Pod  # Тип объекта - Pod
metadata:
  name: pod-demo  # Указываем имя для Pod
spec:
  containers:
    - name: app  # Указываем имя контейнера
      image: myapp  # Указываем образ контейнера
      volumeMounts:
        - name: data  # Указываем имя Volume, которое будет монтироваться в контейнер
          mountPath: /data  # Определяем путь для монтирования Volume в контейнер
  volumes:
    - name: data  # Указываем имя Volume, которое будет использоваться в Pod
      persistentVolumeClaim:
        claimName: pvc-demo  # Указываем имя PVC, которое будет использоваться для монтирования Volume в Pod

</pre>
<p>В этом манифесте мы определяем Pod с именем "pod-demo", который использует образ "myapp". Мы также определяем монтирование PVC "pvc-demo" в каталог "/data" внутри контейнера.</p>
<p>Таким образом, мы изучили, как использовать PVC и StorageClass в Kubernetes для управления хранилищем. Это позволяет нам создавать устойчивые к сбоям приложения, которые могут хранить данные даже при
перезапуске Pod.</p>
<p>В манифесте StorageClass мы указываем, что данный класс не предоставляет динамическое выделение хранилища. Это значит, что PV должен быть создан заранее.
В манифесте PersistentVolume мы определяем, что данный PV может быть монтирован только в одном Pod одновременно, и что данные на PV должны быть сохранены после удаления PVC.
В манифесте PersistentVolumeClaim мы указываем, что PV может быть монтирован только в одном Pod одновременно, и запрос на 500 мегабайт хранилища.
При применении манифестов, мы применяем сначала манифесты StorageClass и PersistentVolume, а затем PersistentVolumeClaim.
В манифесте Pod мы монтируем PVC в каталог "/data" внутри контейнера. Использование PVC позволяет создавать устойчивые к сбоям приложения, которые могут хранить данные даже при перезапуске Pod.
StorageClass позволяет определять различные параметры хранилища, такие как тип, доступность и пропускную способность.
PersistentVolume может быть создан заранее, или динамически с помощью StorageClass.
При использовании манифестов для PVC и PV, необходимо убедиться, что классы хранилища и ёмкость хранилища соответствуют друг другу.
При использовании PVC в манифесте Pod, необходимо убедиться, что имя PVC и объём монтируемого хранилища совпадают.</p>
</body>
</html>
