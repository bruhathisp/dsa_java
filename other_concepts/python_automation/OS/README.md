### Summary of the Lab

**Lab Title:** Work with Python Scripts


#### Objectives:
1. Fix an incorrect Python script.
2. Write a new Python module and use it within the script.

#### Lab Instructions:

1. **Fix File Permissions**:
   - Navigate to the scripts directory using `cd scripts`.
   - List the files using `ls`.
   - View the script contents with `cat health_checks.py`.
   - Update the file permissions with `sudo chmod +x health_checks.py`.
   - Run the script using `./health_checks.py`.
   - If you encounter "Permission denied," ensure the script has execute permissions.

3. **Debug the Script**:
   - Open the script with `nano health_checks.py`.
   - Fix the function `check_cpu_usage` to return `True` if CPU usage is less than 75%.
   - Save and exit the editor (Ctrl-o, Enter, Ctrl-x).
   - Run the script again with `./health_checks.py` to check for "Everything ok."

4. **Create a New Python Module**:
   - Install the requests module using `sudo apt install python3-requests`.
   - Navigate to the scripts directory (`cd ~/scripts`).
   - Create a new file `network.py` using `nano network.py`.
   - Add shebang line `#!/usr/bin/env python3`.
   - Import necessary modules: `requests` and `socket`.
   - Define `check_localhost()` to verify local host configuration (127.0.0.1).
   - Define `check_connectivity()` to verify internet connectivity (HTTP status 200).
   - Save and exit the editor.

5. **Use the Python Module**:
   - Open `health_checks.py` with `nano health_checks.py`.
   - Import the network module (`from network import *`).
   - Update the script to include checks from the network module.
   - Save and exit the editor.
   - Run the script to verify it returns "Everything ok."


### Key Points:

1. **How do you create a new Python module and what should it include?**
   - Create a file with a `.py` extension.
   - Include necessary imports (e.g., `requests`, `socket`).
   - Define functions to perform specific tasks (e.g., `check_localhost`, `check_connectivity`).

2. **How do you use a newly created module in another script?**
   Import the module using `from module_name import *` and call its functions within the script.



### Adding the Shebang Line

The shebang (`#!`) line is used at the beginning of a script to indicate which interpreter should be used to run the script. This is particularly useful in Unix-like operating systems (Linux, macOS).

1. **Purpose of the Shebang Line:**
   - It tells the operating system how to execute the file.
   - Ensures the script runs with the specified interpreter, regardless of the user's default shell.

2. **Format:**
   ```python
   #!/usr/bin/env python3
   ```
   - `#!/usr/bin/env python3`: This uses the `env` command to find the Python interpreter in the user's `PATH`, making it more flexible and portable across different systems.

### Importing Necessary Modules

Modules in Python are libraries of code that you can include in your scripts to add functionality without writing it from scratch.

1. **Requests Module:**
   - The `requests` module is used for making HTTP requests in Python.
   - Install the module (if not already installed) with the command:
     ```bash
     sudo apt install python3-requests
     ```

2. **Socket Module:**
   - The `socket` module provides access to the BSD socket interface. It is used for network-related tasks.
   - This module is included in Python's standard library, so no additional installation is required.


   ```

   - This function makes a GET request to `http://www.google.com` using the `requests` module. If the status code of the response is `200` (which means the request was successful), it returns `True`.

Certainly! Let's break down the `health_checks.py` script line by line:

### Script Breakdown

```python
#!/usr/bin/env python3
```
- **Shebang Line**: This tells the operating system to use the Python 3 interpreter to run this script. The `env` command ensures that the correct interpreter is found in the user's `PATH`.

```python
import shutil
import psutil
from network import *
```
- **Import Statements**:
  - `import shutil`: Imports the `shutil` module, which provides high-level file operations such as copying and removal.
  - `import psutil` Python system and process utilities: Imports the `psutil` module, which provides an interface for retrieving information on system utilization (CPU, memory, disks, network, etc.).
  - `from network import *`: Imports all functions from the `network` module, which should include `check_localhost` and `check_connectivity`.

```python
def check_disk_usage(disk):
    """Verifies that there's enough free space on disk"""
    du = shutil.disk_usage(disk)
    free = du.free / du.total * 100
    return free > 20
```
- **Function `check_disk_usage(disk)`**:
  - `du = shutil.disk_usage(disk)`: Uses the `disk_usage` function from the `shutil` module to get the total, used, and free disk space.
  - `free = du.free / du.total * 100`: Calculates the percentage of free disk space.
  - `return free > 20`: Returns `True` if the free disk space is greater than 20%, otherwise returns `False`.

```python
def check_cpu_usage():
    """Verifies that there's enough unused CPU"""
    usage = psutil.cpu_percent(1)
    return usage < 75
```
- **Function `check_cpu_usage()`**:
  - `usage = psutil.cpu_percent(1)`: Uses the `cpu_percent` function from the `psutil` module to get the CPU usage percentage over a 1-second interval.
  - `return usage < 75`: Returns `True` if the CPU usage is less than 75%, otherwise returns `False`.

```python
# If there's not enough disk, or not enough CPU, print an error
if not check_disk_usage('/') or not check_cpu_usage():
    print("ERROR!")
```
- **Main Logic**:
  - `if not check_disk_usage('/') or not check_cpu_usage()`: Checks if either the disk usage is too high or the CPU usage is too high.
  - `print("ERROR!")`: If either check fails (i.e., returns `False`), it prints "ERROR!".

```python
elif check_localhost() and check_connectivity():
    print("Everything ok")
```
- **Secondary Check**:
  - `elif check_localhost() and check_connectivity()`: If the disk and CPU checks pass, it checks network connectivity.
    - `check_localhost()`: Checks if the local host is correctly configured (should return `True` if the localhost address is 127.0.0.1).
    - `check_connectivity()`: Checks if the computer can successfully connect to the internet (should return `True` if it can reach http://www.google.com and gets a 200 status code).
  - `print("Everything ok")`: If both network checks pass, it prints "Everything ok".

```python
else:
    print("Network checks failed")
```
- **Fallback**:
  - `else`: If the network checks fail (i.e., one of them returns `False`).
  - `print("Network checks failed")`: It prints "Network checks failed".

### Full Script for Reference

```python
#!/usr/bin/env python3

import shutil
import psutil
from network import *

def check_disk_usage(disk):
    """Verifies that there's enough free space on disk"""
    du = shutil.disk_usage(disk)
    free = du.free / du.total * 100
    return free > 20

def check_cpu_usage():
    """Verifies that there's enough unused CPU"""
    usage = psutil.cpu_percent(1)
    return usage < 75

# If there's not enough disk, or not enough CPU, print an error
if not check_disk_usage('/') or not check_cpu_usage():
    print("ERROR!")
elif check_localhost() and check_connectivity():
    print("Everything ok")
else:
    print("Network checks failed")
```

Here’s a detailed line-by-line explanation of the `network.py` script:


- **Import Statements**:
  - `import requests`: Imports the `requests` module, which allows you to send HTTP requests (e.g., GET, POST) in a simple and user-friendly way.
  - `import socket`: Imports the `socket` module, which provides low-level networking interfaces. It can be used to create and manage network connections.

```python
def check_localhost():
    localhost = socket.gethostbyname('localhost')
    return localhost == '127.0.0.1'
```
- **Function `check_localhost()`**:
  - **Function Definition**: Defines a function named `check_localhost`.
  - `localhost = socket.gethostbyname('localhost')`:
    - `socket.gethostbyname('localhost')`: Calls the `gethostbyname` function from the `socket` module to resolve the hostname 'localhost' to its corresponding IP address.
    - The result is assigned to the variable `localhost`.
  - `return localhost == '127.0.0.1'`:
    - Compares the resolved IP address stored in `localhost` to the string `'127.0.0.1'`.  (Resolving the hostname 'localhost' to its corresponding IP address means converting the hostname (a human-readable name) to an IP address (a numerical label).)
    - `127.0.0.1` is the standard loopback address for IPv4, commonly used to refer to the local machine.
    - If the resolved IP address is `127.0.0.1`, the function returns `True`, indicating that the local host is correctly configured. Otherwise, it returns `False`.

```python
def check_connectivity():
    request = requests.get("http://www.google.com")
    return request.status_code == 200
```
- **Function `check_connectivity()`**:
  - **Function Definition**: Defines a function named `check_connectivity`.
  - `request = requests.get("http://www.google.com")`:
    - `requests.get("http://www.google.com")`: Uses the `requests` module to send an HTTP GET request to `http://www.google.com`.
    - The result of the GET request is assigned to the variable `request`. This result is an HTTP response object.
  - `return request.status_code == 200`:
    - `request.status_code`: Accesses the `status_code` attribute of the response object. This attribute contains the HTTP status code returned by the server.
    - `200` is the standard HTTP status code for a successful request (OK).
    - If the status code is `200`, the function returns `True`, indicating successful connectivity to the internet. Otherwise, it returns `False`.

### Summary


- **`check_localhost()` Function**:
  - Resolves 'localhost' to its IP address.
  - Returns `True` if the IP address is `127.0.0.1`, otherwise `False`.
- **`check_connectivity()` Function**:
  - Sends an HTTP GET request to `http://www.google.com`.
  - Returns `True` if the HTTP status code is `200`, otherwise `False`.

These functions help verify the local host configuration and internet connectivity.

