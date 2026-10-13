
# TROUBLESHOOTING.md

## 🧭 Project Setup & Common Issues

This document summarizes the configuration fixes and issues resolved during the initial project setup.

---

## 1. Notebook Fixes — `troubleshooting.ipynb`

### 🔗 Links & Resources
Replaced personal/instructor references with project-specific links:
- GitHub Repository: `https://github.com/gtaltou/ai-engineering-apps`
- Issues: `https://github.com/gtaltou/ai-engineering-apps/issues`
- Discussions: `https://github.com/gtaltou/ai-engineering-apps/discussions`
- Wiki: `https://github.com/gtaltou/ai-engineering-apps/wiki`

> **Note:** If you get 404 errors on Discussions or Wiki links, go to your repository **Settings > General > Features** and enable **Discussions**. Also make sure the Wiki feature is enabled.

### 🤖 Model Name
- ❌ Invalid: `gpt-5-nano`
- ✅ Correct: `gpt-4.1-nano` (aligned with `README.md`)

### 🧪 OpenAI SDK v3 Compatibility
Make sure to use the new client syntax:

```python
from openai import OpenAI
client = OpenAI()
```

---

## 2. Python Version Harmonization

The project had **inconsistent Python versions** across files. All were unified to **Python 3.12**.

| File | Old Version | New Version |
| :--- | :---------- | :---------- |
| `.python-version` | 3.12.12 | 3.12 |
| `environment.yml` | 3.11 | 3.12 |
| `pyproject.toml`  | >=3.11 | >=3.12 |

### Quick fix (if you need to do it again):

```bash
# Update .python-version
echo "3.12" > .python-version

# Update environment.yml
# Change: python=3.11 → python=3.12

# Update pyproject.toml
# Change: requires-python = ">=3.11" → requires-python = ">=3.12"

# Amend the last commit (if not yet pushed)
git add .
git commit --amend --no-edit
git push --force-with-lease
```

> ⚠️ Use `--force-with-lease` carefully. Only do this if you haven't shared the branch with collaborators.

---

## 3. Jupyter Kernel Issues

### 🐍 Problem: Wrong Python interpreter

If you get `ModuleNotFoundError` for packages you know are installed, the notebook kernel is likely pointing to the **system Python** instead of your virtual environment.

#### 🔍 Diagnosis

In a notebook cell, run:

```python
import sys
print(sys.executable)
```

If the output is something like `c:\Python310\python.exe`, you're using the wrong kernel.

#### ✅ Fix — Register the correct kernel

```bash
# Step 1: Navigate to the project directory
cd C:\ai-engineering\ai-engineering-apps

# Step 2: Activate the virtual environment
# Windows:
.venv\Scripts\activate
# macOS / Linux:
source .venv/bin/activate

# Step 3: Install ipykernel
pip install ipykernel

# Step 4: Register the kernel
python -m ipykernel install --user --name=ai-engineering-apps --display-name="Python (ai-engineering-apps)"

# Step 5: Restart Jupyter and select "Python (ai-engineering-apps)" from the Kernel menu
```

---

## 4. Missing Dependencies

### 📦 `ModuleNotFoundError: No module named 'dotenv'`

```bash
pip install python-dotenv
```

### 📦 `ModuleNotFoundError: No module named 'bs4'`

```bash
pip install beautifulsoup4 requests
```

### 📦 Install everything at once

```bash
# From root of project
pip install -e .
# OR
pip install -r requirements.txt
```

> After installing, **restart the kernel** (Kernel → Restart & Run All).

---

## 5. Environment Validation

### ✅ Check your OpenAI API key

Run this in a notebook cell:

```python
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv("OPENAI_API_KEY")

if api_key and api_key.startswith("sk-"):
    print("✅ API key loaded successfully!")
else:
    print("❌ API key missing or invalid")
```

### ✅ Check your virtual environment is active

```python
import sys
print(sys.executable)
# Expected: ...\.venv\Scripts\python.exe  (Windows)
# Expected: .../.venv/bin/python           (macOS/Linux)
```

---

## 6. Testing the `fetch_website_contents` Utility

If you want to test the web scraper without hitting copyright issues, use any of these **royalty-free / open-access** URLs:

```python
# Option 1: Wikipedia (CC-licensed, rich content)
page = fetch_website_contents("https://en.wikipedia.org/wiki/Artificial_intelligence")
print(page[:500])

# Option 2: Your own GitHub repo
page = fetch_website_contents("https://github.com/gtaltou/ai-engineering-apps")
print(page[:500])

# Option 3: Python.org
page = fetch_website_contents("https://www.python.org")
print(page[:500])

# Option 4: Project Gutenberg (public domain books)
page = fetch_website_contents("https://www.gutenberg.org")
print(page[:500])
```

---

## 🚀 Quick Start — From Scratch

```bash
# 1. Clone the repo
git clone https://github.com/gtaltou/ai-engineering-apps
cd ai-engineering-apps

# 2. Create and activate virtual environment
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS / Linux:
source .venv/bin/activate

# 3. Install dependencies
pip install -e .

# 4. Set up environment variables
cp .env.example .env
# Edit .env and add your OPENAI_API_KEY

# 5. Register Jupyter kernel
python -m ipykernel install --user --name=ai-engineering-apps --display-name="Python (ai-engineering-apps)"

# 6. Launch Jupyter
jupyter notebook
```

---

## 🔗 Useful Links

| Resource | URL |
| :------- | :-- |
| Repository | `https://github.com/gtaltou/ai-engineering-apps` |
| Issues | `https://github.com/gtaltou/ai-engineering-apps/issues` |
| Discussions | `https://github.com/gtaltou/ai-engineering-apps/discussions` |
| Wiki | `https://github.com/gtaltou/ai-engineering-apps/wiki` |

---