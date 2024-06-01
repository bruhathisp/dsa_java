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

6. **End the Lab**:
   - Click "End Lab" to complete and submit feedback.

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

### Complete Example of `network.py`

Here’s the complete code for the `network.py` module:

```python
#!/usr/bin/env python3

import requests
import socket

def check_localhost():
    localhost = socket.gethostbyname('localhost')
    return localhost == '127.0.0.1'

def check_connectivity():
    request = requests.get("http://www.google.com")
    return request.status_code == 200
```

### Using the `network` Module in Another Script

1. **Open `health_checks.py`**:

   ```bash
   nano health_checks.py
   ```

2. **Import the Network Module**:

   At the beginning of the `health_checks.py` file, import the network module:

   ```python
   from network import *
   ```

3. **Modify the Script to Use Network Checks**:

   Update the script to call the functions from the `network` module. For example:

   ```python
   if check_disk_usage('/') and check_cpu_usage():
       print("Everything ok")
   elif check_localhost() and check_connectivity():
       print("Everything ok")
   else:
       print("Network checks failed")
   ```

### Complete Example of Modified `health_checks.py`

Here’s the complete code for the modified `health_checks.py`:

```python
#!/usr/bin/env python3

import shutil
import psutil
from network import *

def check_disk_usage(disk):
    du = shutil.disk_usage(disk)
    free = du.free / du.total * 100
    return free > 20

def check_cpu_usage():
    usage = psutil.cpu_percent(1)
    return usage < 75

if check_disk_usage('/') and check_cpu_usage():
    print("Everything ok")
elif check_localhost() and check_connectivity():
    print("Everything ok")
else:
    print("Network checks failed")
```

### Running the Script

1. **Ensure `health_checks.py` Has Execute Permissions**:

   ```bash
   sudo chmod +x health_checks.py
   ```

2. **Run the Script**:

   ```bash
   ./health_checks.py
   ```

If everything is set up correctly, the script should output "Everything ok" if the checks pass.
