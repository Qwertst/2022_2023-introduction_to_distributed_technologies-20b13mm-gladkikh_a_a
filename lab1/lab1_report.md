University: [SPBU](https://spbu.ru/)
Faculty: [MM](https://math.spbu.ru/rus/)
Course: [Introduction to distributed technologies](https://github.com/itmo-ict-faculty/introduction-to-distributed-technologies)
Year: 2022/2023
Group: 20.b13-mm
Author: Gladkikh Andrey Andreevich
Lab: Lab1
Date of create: 19.12.2022
Date of finished: 19.12.2022


## Цель работы

Ознакомиться с инструментами Minikube и Docker, развернуть свой первый "под".


## Ход работы

1. Установил Docker и Minikube.
2. Развернул minikube cluster.
3. Написал манифест для развертывания пода HashiCorp Vault.
```
apiVersion: v1
kind: Pod
metadata:
  name: vault
  labels:
    app: vault
spec:
  containers:
  - name: valut
    image: vault
    ports:
    - containerPort: 8200
 ```   
4. Создал под из манифеста.
```
> kubectl  apply -f vault.yaml
```
5. Создал сервис для доступа к контейнеру
```
> kubectl expose pod vault --type=NodePort --port=8200
```
6. Для доступа к сервису воспользовался командой, переводящей minikube в режим проброса портов.
```
> kubectl port-forward service/vault 8200:8200
```
Теперь сервис доступен по адресу http://localhost:8200
![image](https://user-images.githubusercontent.com/74876495/208449283-57482cd6-869d-4de6-b241-37bc3faa1bdc.png)

7. Для получения токена воспользовался командой
```
> kubectl logs vault
```
![image](https://user-images.githubusercontent.com/74876495/208449837-33b57dcb-000b-48f0-be61-a8a0e8e0b2f4.png)
