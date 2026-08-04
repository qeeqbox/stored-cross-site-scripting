<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/stored-cross-site-scripting/main/content/stored-cross-site-scripting.svg"></p>

## Stored Cross-Site Scripting (Stored XSS)
Stored Cross-Site Scripting (Stored XSS) is a type of vulnerability where an application saves data controlled by an attacker, which could be in a database, file, comment, or user profile. Later, when the application includes this data in a web page without proper context-aware output encoding, it becomes a security risk. When users visit the affected content, their browsers execute the malicious script as if it were a part of the trusted website.

## How Stored XSS Works
1. Identify a Vulnerable Input: The attacker finds an input field whose content is stored and later displayed to users.
2. Submit Malicious Content: The attacker submits malicious JavaScript or HTML through the vulnerable input.
3. Store the Content: The application stores the user-supplied content without proper validation or sanitization.
4. Display the Stored Content: The application retrieves the stored content and displays it without applying context-aware output encoding.
5. Script Execution: The victim's browser executes the malicious script whenever the affected content is viewed.

## Stored XSS Impact
- Persistent Attacks Affecting Multiple Users: Because the malicious payload is stored by the application, every user who accesses the affected content may execute the attacker's script.
- Session Hijacking: Attackers may steal session tokens or authentication information from multiple users who view the compromised content.
- Sensitive Data Theft: Attackers may collect confidential information from victims, including personal data, application data, or information accessible through their sessions.
- Website Defacement: Attackers may modify website content, display unauthorized messages, inject advertisements, or alter the appearance of pages.
- Unauthorized Actions Performed on Behalf of Victims: Attackers may use victims' browser sessions and permissions to perform actions within the application without their knowledge.

## Stored XSS Mitigation
- Perform Context-Aware Output Encoding: Always encode user-generated content before displaying it in a web page.
- Sanitize User-Generated HTML When HTML Is Allowed: If users are allowed to submit HTML, such as in rich text editors or forums, sanitize the content using a trusted HTML sanitization library.
- Validate User Input: Validate input against expected formats and reject malicious or unexpected content where appropriate.
- Implement Content Security Policy (CSP): Use CSP to reduce the impact of malicious scripts stored within application content.
- Monitor and Review User-Generated Content: Regularly review stored content for malicious payloads and remove identified XSS attacks.

## Stored XSS Example 
Clone this current repo recursively
```sh
git clone --recurse-submodules https://github.com/qeeqbox/stored-cross-site-scripting
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
If you examine the ticket section, you will see the payload there
<p align="center"> <img src="https://raw.githubusercontent.com/qeeqbox/stored-cross-site-scripting/main/content/5.png"></p>

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
