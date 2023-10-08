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

Data:
order_body = {    "firstName": "Anton",
    "lastName": "Zaitsev",    "address": "Chertaovskaya",
    "metroStation": 2,    "phone": "+7 999 999 99 99",
    "rentTime": 2,    "deliveryDate": "2023-01-01",
    "comment": "No phone calls",    "color": [
        "BLACK"    ]
}

Configuraion:
CREATE_ORDER_PATH = "/api/v1/orders"GET_ORDER_BY_TRACK_PATH = "/api/v1/orders/track?t="
URL = ""

sender:
import configuration 
import requests
#функция на создание заказа
def create_order(order_body):   return requests.post(configuration.URL + configuration.CREATE_ORDER_PATH,
                         json=order_body
#функция получения заказа по треку
def get_order_info_by_track(track):    
                                  return requests.get(configuration.URL + configuration.GET_ORDER_BY_TRACK_PATH + str(track))

create:
import sender_stand_requestimport data
#получение данных о заказе по его трекуdef test_create_order():
    track_number = sender_stand_request.get_order_info_by_track(sender_stand_request.create_order(data.order_body).json()["track"])    assert track_number.status_code == 200



