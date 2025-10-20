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
