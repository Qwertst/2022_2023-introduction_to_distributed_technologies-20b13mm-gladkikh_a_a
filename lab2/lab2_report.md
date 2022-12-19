University: [SPBU](https://spbu.ru/)
Faculty: [MM](https://math.spbu.ru/rus/)
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
Year: 2022/2023
Group: 20.b13-mm
Author: Gladkikh Andrey Andreevich
Lab: Lab2
Date of create: 19.12.2022
Date of finished: 19.12.2022


## Цель работы

Ознакомиться с типами "контроллеров" развертывания контейнеров, ознакомится с сетевыми сервисами и развернуть свое веб приложение.

## Ход работы

1. Создал манифест для deployment с 2 репликами контейнера ifilyaninitmo/itdt-contained-frontend:master и передаk переменные в эти реплики.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: itdt-front
  labels:
    app: itdt
spec:
  replicas: 2
  selector:
    matchLabels:
      app: itdt
  template:
    metadata:
      labels:
        app: itdt
    spec:
      containers:
      - name: itdt-front
        image: ifilyaninitmo/itdt-contained-frontend:master 
        ports:
        - containerPort: 3000          
        env:
        - name: REACT_APP_USERNAME
          value: "Qwerty"
        - name: REACT_APP_COMPANY_NAME
          value: "ICANT"
```
2. Создал деплоймент из манифеста
```
> kubectl apply -f itdt.yaml 
```
3. Создал сервис для доступа к подам
```
> kubectl expose deploy itdt-front --type=ClusterIP --port=3000
```
4. Запустил режим проброса портов и подключился к контейнерам через браузер.
```
> kubectl port-forward service/itdt-front 3000:3000
```
![image](https://user-images.githubusercontent.com/74876495/208455799-1fe06dcc-3a17-47eb-884a-681be7b29cbc.png)
Переменные на странице не изменяются.

5. Проверил логи контейнеров.
![image](https://user-images.githubusercontent.com/74876495/208456639-d3c3d289-b024-4dfc-a93c-d5030ea2629c.png)
