# Creating and Installing a Private Python Package from GitHub (Step‑by‑Step)

This guide shows **the clean, repeatable way** to create a **private Python package**, push it to GitHub, and install it via `pip` (including from Jupyter).

## What you will need
1. GitHub Desktop
2. Terminal Command Line
3. A function or two you want to work with (examples provided here)
4. VS Code
5. IPythonNotebook (Jupyter Notebook)

---

## Overview of What You’ll Do

1. Create an empty GitHub repo
2. Clone it locally
3. Create a Python package structure
4. Add package code
5. Configure `pyproject.toml`
6. Build and test the package locally
7. Push to GitHub
8. Install the package via `pip` from GitHub

---

## 1. Create the GitHub Repository (First)

On GitHub:

- Create a **new repository**:
- Give it a succinct and meaningful name (no more than 2 words)
- Private or public is fine, **but perhaps this time keep it private**.
- Add a ```README```, `.gitignore`, and a license

I'm calling this repo:
  ```
  my-installs
  ```
- you can call it anything you want, but ** note that I am using `my-install` and will keep it consistent throughout.



---

## 2. Clone the Repo Locally

### Clone using GitHub Desktop

Clone the repo via GitHub Desktop. Make sure to clone it into your dataProjects folder.

---

## 3. Create and Activate a Virtual Environment

Through your Mac terminal, navigate to `dataProjects`. If you have it in your root directory, simply copy and paste the following:

```bash
cd dataProject/my-installs
```

If you have have `dataProject` is some other location, do the following:
- Hold down the Option (⌥) key
- Right-click on the file or folder
- Select "Copy [filename] as Pathname"
- In your terminal, write `cd ` and paste what your path.

In either case, you should be inside your `my-installs` directory.
Run the following:

```bash
python3 -m venv venv
source venv/bin/activate
```

Confirm:

```bash
which python
```

Expected output:

```
.../my-installs/venv/bin/python
```

If you see an alias like ```python: aliased to /usr/bin/python3```

Type: 

```bash
unalias python
```

Again, try:

```bash
which python
```

and you should see the expected output.

---

## 4. Create Package Folder Structure

** Careful here: I often mess it up by ignoring this step. Note the **underscores** I am making for a new directory called ```my_installs``` inside the project folder called ```my-installs```.

- Navigate into your `my-install` folder in `dataProjects` and create a new folder called ```my_installs```.
- Open VS Code/Sublime or any code editor and create a new file and save it as ```__init__.py``` NOTE: these are two sets of 2 underscores that surround `init` INSIDE `my_installs`. Again please note that it is being saved inside of my_installs (with the underscore) which is inside my-installs (with the hyphen).

---

## 5. Add Package Code into your __init__.py file


Paste the following into `my_installs/__init__.py`:

```python
import time
from random import uniform


def addNumbers(number1, number2):
    """
    Provide two numbers and I'll add them together
    """
    return number1 + number2


def timer(start_time, end_time):
    """
    Timer to snooze while scraping
    Para_1: start_time: integer for minimum seconds snooze
    Para_2: end_time: integer for maximum seconds snooze
    """
    snoozer = uniform(start_time, end_time)
    print(f"Snoozing for {snoozer} seconds!")
    time.sleep(snoozer)
```

---

## 6. Create `pyproject.toml`

- Using VS code or Sublime or any code editor, create a new file and save it as `pyproject.toml` into **`my-installs`** NOT into `my_installs`. **Again note the underscore v. the hyphen!**
- 

Paste this code into `pyproject.toml` and hit save again.

```toml
[build-system]
requires = ["setuptools>=61"]
build-backend = "setuptools.build_meta"

[project]
name = "my-installs"
version = "0.1.0"
description = "Small helper utilities"
requires-python = ">=3.8"

authors = [
  { name = "Your Name" } # REMEMBER TO PUT YOUR OWN NAME
]

[tool.setuptools]
packages = ["my_installs"]
```
When you paste the above into the file, make sure to update
- line 6 (name ="my-installs")
line 12: with your name
line 16 with the name of your package **(note the underscore v. the hyphen)**.

⚠️ **This section is required** or the package will install but not import correctly.

---

## 7. Add `.gitignore`

- Recall that `.gitignore` is a hidden file. While inside your `my-install` folder, click on **Command + Shift + Period (.)** to reveal it.
- Double click on it to open it. It doesn't matter what it opens into.


Copy and paste the following into by replacing all the text in the file:

```gitignore
venv/
dist/
build/
*.egg-info/
__pycache__/
.DS_Store
```

- Save it.
---

## 8. Build Your Package
**In all the following steps, if you throw an error, try `pip3` instead of `pip`.**

Install build tools:

```bash
pip install --upgrade pip build
```

If that throws an error try:

```bash
pip3 install --upgrade pip build
```

Build package:

```bash
python -m build
```

Then install (note you will have your own package name with the **underscore**)

```bash
pip install --force-reinstall dist/my_installs-0.1.0-py3-none-any.whl
```

If you get an error saying the file does not exist, look into the `dist`` director to make sure the numbers in the pip install above match the ones in the file. If they don't then run to force it to read the correct file number.

```bash
rm -rf dist build my_installs.egg-info
python3 -m pip install --upgrade pip build
python3 -m build
ls dist
```
Then run this again:
```bash
pip install --force-reinstall dist/my_installs-0.1.0-py3-none-any.whl
```


## 9. Test:
Back in terminal, copy and paste the following. This both imports one of the functions and runs it.

```bash
python -c "from my_installs import addNumbers; print(addNumbers(2,3))"
```

Expected output:

```
5
```

---

## 10. Commit and Push to GitHub

Use GitHub Desktop to commit and push your repo to GitHub.com.

---

## 11. Using from Jupyter Notebook

**Important:** The notebook kernel **must use the same Python environment**.

Inside a notebook cell (remember to update `YOUR_USERNAME` to your GitHub username!)

```python
!pip install git+ssh://git@github.com/YOUR_USERNAME/my-installs.git
```

Then:

```python
from my_installs import addNumbers
addNumbers(10, 20)
```

---

## Common Failure Causes (and Fixes)

### ❌ `ModuleNotFoundError: my_installs`

✔ Missing this in `pyproject.toml`:

```toml
[tool.setuptools]
packages = ["my_installs"]
```

---

### ❌ Package installs but won’t import in Jupyter

✔ Kernel is using a different Python:

```python
import sys
sys.executable
```

Ensure it matches the environment where you ran `pip install`.

---

## Final Mental Model

- **Repo name** → `my-installs`
- **Import name** → `my_installs`
- **pip name** → `my-installs`
- `pyproject.toml` controls everything

