
# 📦 Data Versioning with Git, DVC, and S3

This project demonstrates how to integrate Git and DVC (Data Version Control) to manage and version control dataset files while storing them remotely (e.g., S3). The project includes a simple script that generates and updates a CSV file stored in a `data/` directory.

---

## 🛠️ Project Setup

### 1. Initialize a Git Repository
```bash
git init
git remote add origin <your-repo-url>
git clone <your-repo-url>
```

### 2. Create Python Script
Create a file `mycode.py` that generates and saves a `.csv` file into the `data/` directory.

```python
# mycode.py
import pandas as pd
import os

os.makedirs("data", exist_ok=True)
df = pd.DataFrame({"Name": ["John", "Alice"], "Age": [28, 24]})
df.to_csv("data/sample.csv", index=False)
```

### 3. Git Commit (Before DVC)
```bash
git add .
git commit -m "Initial commit with Python script and data"
git push
```

---

## 🔄 DVC Setup & Remote Configuration

### 4. Install DVC
```bash
pip install dvc
```

### 5. Initialize DVC
```bash
dvc init
```

### 6. Setup Remote (e.g., S3)
```bash
mkdir S3
dvc remote add -d myremote S3
```

---

## 🧠 Start Tracking Data with DVC

### 7. First Time DVC Tracking
```bash
dvc add data/
```

If prompted:
```bash
git rm -r --cached data/
git commit -m "Stop tracking data directory in Git"
```

### 8. Add .dvc Files to Git
```bash
dvc add data/
git add .gitignore data.dvc
git commit -m "Track data with DVC"
```

### 9. Push Data to Remote Storage
```bash
dvc commit
dvc push
git add .
git commit -m "First version of data (v1)"
git push
```

---

## 🔁 Versioning Your Data

### 10. Make Changes to Data
Modify `mycode.py` to append a new row and re-run it:
```python
df = pd.read_csv("data/sample.csv")
df.loc[len(df.index)] = ["Bob", 32]
df.to_csv("data/sample.csv", index=False)
```

### 11. Track & Push New Version (v2)
```bash
dvc status
dvc commit
dvc push
git add .
git commit -m "Second version of data (v2)"
git push
```

---

## 🔂 Repeat for v3, v4, etc.

Each time you update your data:
- Modify and run your script
- Check with `dvc status`
- Run:
  ```bash
  dvc commit
  dvc push
  git add .
  git commit -m "New data version"
  git push
  ```

---

## ✅ Verify Status

At any point, verify everything is synced:
```bash
dvc status
git status
```

---

## 📂 Files in This Project

- `mycode.py` – Python script to create/modify dataset
- `data/` – Directory containing dataset (tracked by DVC)
- `data.dvc` – Metadata file for data directory
- `.gitignore` – Ensures `data/` is not tracked by Git
- `.dvc/` – DVC configuration directory
- `.dvcignore` – Similar to `.gitignore`, but for DVC
- `S3/` – Remote directory configured for DVC

