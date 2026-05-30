# Lab 3 - Troubleshooting & Debugging Exercise

![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge&logo=python&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-yellow?style=for-the-badge&logo=javascript&logoColor=black)
![Bash](https://img.shields.io/badge/Shell-Bash_Script-green?style=for-the-badge&logo=gnu-bash&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-success?style=for-the-badge)

---

## **Scenario**

Imagine you are a junior engineer who has just been handed a repository of broken code left behind by a previous developer. None of the four files run correctly out of the box - they contain a mix of syntax errors, logic errors, and runtime traps deliberately seeded throughout the codebase. Your job is not to rewrite anything from scratch, but to read each error message carefully, diagnose the root cause, make the smallest targeted fix, and document every step of your process so that a teammate could reproduce your reasoning.

---

## What the Lab Does

This lab covers four files across three languages, each with a defined intended output and multiple embedded bugs. The workflow runs through 6 steps:

1. Fork the provided GitHub repository and clone it locally into the `cpg_lab3_group` working directory
2. Attempts to run each of the four files and records the first error encountered
3. Reads error messages and traces them back to the responsible line of code
4. Applies targeted, incremental fixes - one error at a time - without rewriting files entirely
5. Re-run each file after each fix to confirm the change had the intended effect
6. Documents every initial error, diagnosis step, code change, and final successful execution in `DEBUGGING.md`

---

## Repository

| Field              | Detail                                                       |
|--------------------|--------------------------------------------------------------|
| Original repo      | `https://github.com/solnarcisse/SingleA_Armageddon_VersionB` |
| Cloned into        | `cpg_lab3_group/`                                            |
| Lab files location | `cpg_lab3_group/`                                            |

---

## File Structure

```
CPG_LAB3_GROUP/
├── deliverables/
|   ├── 01_deliverable_python.tx
|   └── 02_deliverable_python.txt
├── images/
|   ├── 01_git_clone_proof.jpg
|   ├── 02_script_js_output.png
|   ├── 03_script_js_output.png
|   ├── 04_script_js_output.png
|   ├── 05_html_output.jpg
│   └── 06_python_output.jpg
├── scripts
│   ├── index.html       ← Age calculator web page
│   ├── python_app.py    ← Prime number generator
│   ├── script.js        ← Bubble sort & statistics
│   └── script.sh        ← Sum/average calculator
├── .gitignore
├── DEBUGGING.md             ← Full error log, diagnosis steps, and all fixes applied
└── README.md
```

---

## Intended Outputs

### `python_app.py`
Generates all prime numbers up to a user-entered limit, displays them, counts them, and calculates their average.

### `script.js`
Sorts an array of numbers using bubble sort, prints ascending and descending orders, then calculates the median and mean.

### `script.sh`
Accepts numbers as command-line arguments, calculates their sum and average, and identifies the maximum and minimum values.

### `index.html`
Presents a form where the user enters a birth year; the page computes their age and displays whether they are an adult or a minor.

---

## **Steps to Complete the Lab 3**

### Step 1: Prepare the Project Repository

Create or open the GitHub repository that will be used to submit Lab 3.

```bash
git clone <forked-repository-url>
cd <repository-name>
```

### Step 2: Add the Project Files

Add the required project files to the repository. (if necessary)

```bash
touch index.html python_app.py script.js script.sh
```

This generates a blank version of `index.html`, `python_app.py`, `script.js`, and `script.sh` that can be filled in with the required code for Lab 3.

> Confirm the content from GitHub is the same as the perspective files; if not, you will have to copy in their files.
---

## **How to Run (After Fixes Applied)**

```bash
# Navigate to the lab files directory
cd cpg_lab3_group/

# Python - prime number generator
python python_app.py
# Prompt: Enter an upper limit to generate primes: 20

# JavaScript - bubble sort and statistics (requires Node.js)
node script.js 
# OR Run from a console on a browser in the Developer tools section, as shown in the images.

# Bash - sum/average/min/max calculator
./scripts/script.sh 3 7 2 9 5

# HTML - age calculator
# Open index.html via Live Server in VS Code, then view in browser
```

---

## Deliverables

| Deliverable                   | Location                                                     |
|-------------------------------|--------------------------------------------------------------|
| Original repository           | `https://github.com/solnarcisse/SingleA_Armageddon_VersionB` |
| Fixed, runnable code          | All four files in `cpg_lab3_group/`                          |
| Troubleshooting documentation | `DEBUGGING.md`                                               |

---

## Screenshots

| Screenshot               | Description                                                              |
|--------------------------|--------------------------------------------------------------------------|
| `01_git_clone_proof.jpg` | VS Code terminal showing `git clone` of original GitHub into local folder|
| `02_script_js_output.png`| Results of script file execution on browser                              |
| `03_script_js_output.png`| Results of script file execution on browser                              |
| `04_script_js_output.png`| Results of script file execution on browser                              |
| `05_html_output.jpg`     | Results of HTML file execution on browser                                |
| `06_python_output.jpg`   | Results of python file execution version                                 |

---

## **Git Workflow Used**

```bash
# Step 1: Navigate to working directory and clone the repository
cd ~/Documents/TheoWAF/SingleA/cpg_lab3_group
git clone https://github.com/solnarcisse/SingleA_Armageddon_VersionB.git

# Step 2: Navigate into the cloned repository
cd cpg_lab3_group

# Step 3: Make targeted fixes to each file

# Step 4: Stage, commit, and push
git status
git add .
git commit -m "Fix syntax and logic errors in python_app.py, script.js, script.sh, index.html"
git push
```
---

## **Collaborators/Authors**

1. **Vince Woods**
2. **Ali Zachary**
3. **Cassiem Davids**
4. **Mister_Alex = Aleki Mdogo (fb)**
5. **Ernie**
6. **Dante Dorsey**
7. **Tyler Tedson**
