# atividade-godoi-semana5

Código referente ao seguinte vídeo: https://drive.google.com/file/d/1Y4Nzf7L1g56uTiXrQqUPlUirlVPAVl2t/view?usp=drivesdk

```

import machine
import time
import dht
from umqtt.simple import MQTTClient
import network


wifi = 'Felipe_teste'

password = 'felipe1908'


endpoint = b'/v1.6/devices/Demo'


wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(wifi,password)

pessoa = MQTTClient(
    client_id='umqtt_client',
    port=1883,
    user='BBUS-ZZgfjNk3TmNzUznf4801GI5VwMIrzY',
    server = 'industrial.api.ubidots.com',
    password=''
    )



time.sleep(5)
print(wlan.isconnected())


sensor = dht.DHT11(machine.Pin(4))

while True:
    try:
        sensor.measure()
        temp = sensor.temperature() 
        print("Temperatura em graus Celsius: {:.1f} °C".format(temp))
        pessoa.connect()
        print('foi')
        pessoa.publish(endpoint, '{"Demo: 1"}')
        print('foi2')
        pessoa.disconnect()
        print('foi3')

    except OSError as e:
        print("Falha ao ler o sensor.")
        

    time.sleep(2)



```
