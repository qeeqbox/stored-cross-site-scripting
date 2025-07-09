<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/stored-cross-site-scripting/main/content/stored-cross-site-scripting.svg"></p>

An application enables users to control the Document Object Model (DOM) environment. A threat actor can exploit this feature by injecting a malicious payload into the trusted web application database. When users interact and request resources from the database, their browsers retrieve the payload and then execute it. This vulnerability is reflected in the HTTP(s) response and occurs on the client side. However, the payload is saved on the server-side

Clone this current repo recursively
```sh
git clone --recursive https://github.com/qeeqbox/stored-cross-site-scripting
```
Run the webapp using Python
```sh
python3 stored-cross-site-scripting/vulnerable-web-app/webapp.py
```
Open the webapp in your browser 127.0.0.1:5142
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/reflected-cross-site-scripting/main/content/1.png"></p>
Open the network tab from the developer tools to examine the requests and responses
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/reflected-cross-site-scripting/main/content/2.png"></p>
If you type the URL + test, it will take you to the test resourse (page), it does not exist but the test keyword gets embedded in the page
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/reflected-cross-site-scripting/main/content/3.png"></p>
A threat actor could embed a malicious payload and send it to a victim using social engineering attacks. If the victim falls for it, their browser will send the request to the webapp
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/reflected-cross-site-scripting/main/content/4.png"></p>
Then, the browser will execute a malicious payload
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/reflected-cross-site-scripting/main/content/5.png"></p>

## Code
This logic will check if the requested page has a route or exists, if it does not, then it will pass the requested page value to the msg_page() function
```py
def do_GET(self):
    ...
    self.send_content(404, [('Content-type', 'text/html')], self.msg_page(f"Error: The requested URL {urllib_parse.unquote(parsed_url.path)} was not found".encode("utf-8")))
    ...
```
The msg_page() function will embed the user value in the webpage
```py
def msg_page(self, msg, prev=None):
    with open(path.join(TEMPLATE_FOLDER,"msg.html"),"rb") as fi:
        if prev:
            return fi.read().replace(b"{{msg-result}}",msg).replace(b"{{msg-prev}}",prev).replace(b"{{msg-page}}",b"Return")
        else:
            return fi.read().replace(b"{{msg-result}}",msg).replace(b"{{msg-prev}}",b"/").replace(b"{{msg-page}}",b"Home")
```
 
## Impact
Vary

## Risk
- Session Hijacking
- Credential Theft

## Redemption
- Server input validation

## ID
cb251c97-067d-4f13-8195-4f918273f41b
