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
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/stored-cross-site-scripting/main/content/1.png"></p>
Use the default credentials (username: admin and password: admin) to login
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/stored-cross-site-scripting/main/content/2.png"></p>
A threat actor could embed a malicious payload instead of a ticket
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/stored-cross-site-scripting/main/content/3.png"></p>
When the victim logs in (The admin user), the payload will be executed by the broswer 
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/stored-cross-site-scripting/main/content/4.png"></p>

## Code
When the user adds a ticket to the webapp, the ticket is sent from the user to the webapp using a POST request, the add route is used, and the data is passed to the add_ticket() function
```py
def do_POST(self):
    ...
    elif parsed_url.path == "/add":
        self.add_ticket(post_request_data["ticket"][0])
        self.redirect(URL)
    ...
```
The add_ticket() function will embed the user value in an SQLite database
```py
@logged_in
@check_access(access="ticket")
def add_ticket(self, ticket):
    with connect(DATABASE, isolation_level=None) as connection:
        cursor = connection.cursor()
        cursor.execute("INSERT into ticket(username, ticket) values(?,?)", (self.session["username"], ticket))
        return True
    return False
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
