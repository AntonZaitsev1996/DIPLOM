# DIPLOM

# Работа с базой данных

# Задание №1

SELECT "Couriers".login,        COUNT ("Orders"."courierId")      
       FROM "Couriers"JOIN "Orders" ON "Orders"."courierId"="Couriers".id
WHERE "Orders"."inDelivery"=trueGROUP BY "Couriers".login

# Задание №2

SELECT track,     CASE WHEN finished = true THEN 2
           WHEN cancelled = true THEN -1           WHEN "inDelivery" = true THEN 1
           ELSE 0           END AS status
FROM "Orders";

# Автоматизация теста к API

data:
order_body = {
    "firstName": "Naruto",
    "lastName": "Uchiha",
    "address": "Konoha, 142 apt.",
    "metroStation": 4,
    "phone": "+7 800 355 35 35",
    "rentTime": 5,
    "deliveryDate": "2020-06-06",
    "comment": "Saske, come back to Konoha",
    "color": [
        "BLACK"
    ]
}


configuraion:
CREATE_ORDER_PATH = "/api/v1/orders"
GET_ORDER_BY_TRACK_PATH = "/api/v1/orders/track?t="
URL = "https://ed5be1ac-c862-4588-87a9-9a099b27a19b.serverhub.praktikum-services.ru"

sender_stand_request:
import configuration
import requests

#функция на создание заказа
def create_order(order_body):
    return requests.post(configuration.URL + configuration.CREATE_ORDER_PATH, json=order_body)

#функция получения заказа по треку
def get_order_info_by_track(track):
    return requests.get(configuration.URL + configuration.GET_ORDER_BY_TRACK_PATH + str(track))

create:
import sender_stand_request
import data
import requests

#получение данных о заказе по его треку
def test_create_order():
    track_number = sender_stand_request.get_order_info_by_track(sender_stand_request.create_order(data.order_body).json()["track"])
    assert track_number.status_code == 200


# Скриншоты 
![image](https://github.com/AntonZaitsev1996/DIPLOM/assets/143004894/639f00dc-a792-49ce-8032-7e5d7dfbe015)
![image](https://github.com/AntonZaitsev1996/DIPLOM/assets/143004894/f3c2878b-e9ac-4850-bd56-cb56de328115)
![image](https://github.com/AntonZaitsev1996/DIPLOM/assets/143004894/aadac795-4441-43c7-95b9-9c3cdd08a2ab
![image](https://github.com/AntonZaitsev1996/DIPLOM/assets/143004894/3633afbf-4e0f-41f8-becc-7bd431685318)








