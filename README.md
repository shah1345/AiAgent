# AiAgent


# Personal AI Assistant MVP - Guide

## 1. Overview
This guide helps you build a **personal AI assistant** on Ubuntu that can:
- Remember tasks
- Add tasks via voice commands
- Take notes
- Manage financial data
- Track progress

The MVP focuses on **task management + voice command + local AI reasoning**.

---

## 2. Prerequisites

- Ubuntu Server (20.04+)
- Python 3.12+
- 46 GB RAM, 16 CPU threads (CPU-only setup)
- Optional: internet connection for downloading models

### Install essentials
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install python3-venv python3-pip git -y

```
### 3. Set Up Python Virtual Environment
```bash
python3 -m venv ai_env
source ai_env/bin/activate
```


### 4. Install Required Python Packages
```bash
pip install --upgrade pip
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
pip install transformers
pip install SpeechRecognition vosk pyttsx3
pip install sqlite3


```


- torch/transformers → AI model for reasoning

- SpeechRecognition / vosk → Offline voice command recognition

- pyttsx3 → Text-to-Speech

- sqlite3 → Task / Notes / Finance storage

### 5. Project Structure


AI_Assistant/
├─ ai_env/                  # Python virtual environment
├─ main.py                  # Main assistant script
├─ models/                  # AI models (local Hugging Face)
├─ data/
│   ├─ tasks.db             # SQLite database
│   └─ notes.db
├─ README.md



### 6. Sample Python Script (main.py)

```bash
import sqlite3
import pyttsx3
import speech_recognition as sr
from datetime import datetime

# Initialize TTS
engine = pyttsx3.init()

# Initialize database
conn = sqlite3.connect('data/tasks.db')
c = conn.cursor()
c.execute('''CREATE TABLE IF NOT EXISTS tasks
             (id INTEGER PRIMARY KEY, task TEXT, created_at TEXT)''')
conn.commit()

# Speech Recognizer
recognizer = sr.Recognizer()

def speak(text):
    engine.say(text)
    engine.runAndWait()

def listen():
    with sr.Microphone() as source:
        print("Listening...")
        audio = recognizer.listen(source)
        try:
            text = recognizer.recognize_vosk(audio)
            print(f"You said: {text}")
            return text.lower()
        except:
            print("Sorry, could not understand.")
            return ""

def add_task(task_text):
    c.execute("INSERT INTO tasks (task, created_at) VALUES (?, ?)", 
              (task_text, datetime.now().strftime("%Y-%m-%d %H:%M:%S")))
    conn.commit()
    speak(f"Task added: {task_text}")

def list_tasks():
    c.execute("SELECT id, task, created_at FROM tasks")
    tasks = c.fetchall()
    if tasks:
        for t in tasks:
            print(f"{t[0]}: {t[1]} (created {t[2]})")
    else:
        print("No tasks found.")

# Main loop
while True:
    speak("Say a command: add task or list tasks or exit")
    command = listen()
    if "add" in command:
        speak("What is the task?")
        task_text = listen()
        if task_text:
            add_task(task_text)
    elif "list" in command:
        list_tasks()
    elif "exit" in command:
        speak("Goodbye!")
        break
    else:
        speak("Command not recognized.")
```



### 7. Running the Assistant

```bash
source ai_env/bin/activate
python3 main.py
```



Say commands like:

"add task" → assistant will ask task details

"list tasks" → assistant lists all tasks

"exit" → exit program

8. Next Steps / Extensions

Add financial database:

Track income, expenses, and balances

Integrate AI reasoning using Hugging Face small LLMs:

Summarize tasks

Analyze financial progress

Expand voice commands for more actions

Add text/voice summaries of progress

Add encryption for sensitive data

9. Security Considerations

Encrypt databases storing financial info

Limit server access (localhost or VPN)

Use virtual environment to isolate Python packages

10. References

Vosk Speech Recognition

Hugging Face Transformers

pyttsx3 Text-to-Speech

SQLite Python
