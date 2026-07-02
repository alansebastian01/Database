Absolutely! Here's a **Windows Command Prompt + Flask Cheat Sheet** designed for someone who's just starting. These are the commands you'll use over and over.

---

# 🖥️ Windows Command Prompt Cheat Sheet

## 📂 Navigation

### Show your current location

```cmd
cd
```

Example:

```text
C:\Users\Alan Sebastian\Downloads\Demo\practice2
```

---

### List files and folders

```cmd
dir
```

Example:

```text
app.py
templates
static
```

---

### Go into a folder

```cmd
cd practice2
```

---

### Go back one folder

```cmd
cd ..
```

Example:

```text
Before:
C:\Demo\practice2

After:
C:\Demo
```

---

### Go back two folders

```cmd
cd ..\..
```

---

### Clear the screen

```cmd
cls
```

---

# 📁 Create Files and Folders

Create a folder:

```cmd
mkdir templates
```

or

```cmd
md templates
```

Create another folder:

```cmd
mkdir static
```

---

# 🐍 Python

Check Python version:

```cmd
python --version
```

Run a Python program:

```cmd
python app.py
```

Install Flask:

```cmd
pip install flask
```

See installed packages:

```cmd
pip list
```

---

# 🌐 Flask

Tell Flask which file contains your app:

```cmd
set FLASK_APP=app.py
```

Turn on debug mode:

```cmd
set FLASK_DEBUG=1
```

Start the server:

```cmd
flask run
```

Stop the server:

```text
Ctrl + C
```

Open your browser:

```text
http://127.0.0.1:5000
```

---

# 📂 Typical Flask Project

```text
practice2/
│
├── app.py
│
├── templates/
│   ├── index.html
│   └── edit.html
│
└── static/
    ├── css/
    ├── js/
    └── images/
```

---

# ⭐ VS Code Shortcuts

Save file:

```text
Ctrl + S
```

Open Terminal:

```text
Ctrl + `
```

Find text:

```text
Ctrl + F
```

Copy:

```text
Ctrl + C
```

Paste:

```text
Ctrl + V
```

Undo:

```text
Ctrl + Z
```

Redo:

```text
Ctrl + Y
```

---

# 🧠 Common Workflow

Every time you start working on your Flask app:

```cmd
cd C:\Users\Alan Sebastian\Downloads\Demo\practice2
dir
set FLASK_APP=app.py
set FLASK_DEBUG=1
flask run
```

When you're done:

```text
Ctrl + C
```

---

# 💡 What to memorize

You don't need to memorize every command. Focus on these first:

| Command                | Purpose                     |
| ---------------------- | --------------------------- |
| `dir`                  | Show files and folders      |
| `cd foldername`        | Go into a folder            |
| `cd ..`                | Go up one folder            |
| `cls`                  | Clear the terminal          |
| `python app.py`        | Run a Python program        |
| `set FLASK_APP=app.py` | Tell Flask which app to run |
| `flask run`            | Start the Flask server      |
| `Ctrl + C`             | Stop the Flask server       |

---

One last tip: **don't try to memorize this sheet in one sitting**. Keep it beside you while you work. After a week or two of using these commands, you'll find yourself typing them from memory because you've used them repeatedly, not because you forced yourself to memorize them.
