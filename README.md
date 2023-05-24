### pip install python-binance

## Для работы с API Binance мы будем использовать класс Client, который позволяет создавать и отправлять запросы к API.
Функция для создания ордеров на основе данных от фронтенда:


<div class="code-block">
<pre>
<code class="python">

from binance.client import Client
import random

def create_orders(api_key, api_secret, symbol, order_data):
    client = Client(api_key, api_secret)

    volume = order_data["volume"]
    number = order_data["number"]
    amount_dif = order_data["amountDif"]
    side = order_data["side"]
    price_min = order_data["priceMin"]
    price_max = order_data["priceMax"]

    # Вычисляем объем каждого ордера
    volume_per_order = volume / number

    # Создаем заданное количество ордеров
    for i in range(number):
        # Выбираем случайную цену в диапазоне от price_min до price_max
        price = round(random.uniform(price_min, price_max), 2)

        # Выбираем случайный объем в верхней или нижней частях диапазона
        volume_offset = round(random.uniform(-amount_dif, amount_dif), 2)
        if volume_offset < 0:
            volume_offset = max(volume_offset, -volume_per_order)
        else:
            volume_offset = min(volume_offset, volume_per_order)

        # Вычисляем итоговый объем для данного ордера
        order_volume = round(volume_per_order + volume_offset, 2)

        # Отправляем запрос на создание ордера
        order = client.create_order(
            symbol=symbol,
            side=side,
            type="LIMIT",
            timeInForce="GTC",
            quantity=order_volume,
            price=price,
        )

        print(
            f"Created order {i + 1}/{number} with volume: {order_volume}, price: {price}"
        )
</code>
</pre>
</div>

Эта функция принимает API ключ и секретный ключ, используемые для аутентификации в API, символ торговли (например, BTCUSDT), а также данные от фронтенда, необходимые для создания ордеров.

Функция вычисляет объем каждого ордера (volume_per_order) на основе общего объема volume и количества ордеров number. Затем она создает заданное количество ордеров, каждый из которых имеет случайную цену в диапазоне от price_min до price_max, а также случайный объем в пределах разброса amount_dif.

Для отправки запросов на создание ордеров используется метод create_order объекта класса Client. Запрос отправляется для каждого ордера, и после успешного создания ордера в консоль выводится сообщение с номером и объемом ордера.

# Test

<div class="code-block">
<pre>
<code class="python">
 
api_key = 'ваш_api_ключ'
api_secret = 'ваш_секретный_ключ'
symbol = 'BTCUSDT'
order_data = {
   "volume": 10000.0,
   "number": 5,
   "amountDif": 50.0,
   "side": "SELL",
   "priceMin": 200.0,
   "priceMax":
  
  </code>
</pre>
</div>
