# Final-Project
Password strength analyzer
IN MY PROJECT I HAVE MADE PASSWORD STRENGTH ANALYZER WITH PASSWORD GENERATOR WITH THE GUI

# THE CODE FOR THE PASSWORD STRENGTH ANALYZE :

import math
import re
from datetime import datetime

def calculate_entropy(password):
    charset = 0
    if re.search(r'[a-z]', password): charset += 26
    if re.search(r'[A-Z]', password): charset += 26
    if re.search(r'\d', password): charset += 10
    if re.search(r'\W', password): charset += 32
    entropy = math.log2(charset) * len(password)
    return entropy

def analyze_password(password):
    entropy = calculate_entropy(password)
    timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

  strength = ""
    if entropy > 80:
        strength = " Very Strong"
    elif entropy > 60:
        strength = " Strong"
    elif entropy > 40:
        strength = " Moderate"
    else:
        strength = " Weak"

  print(f"\nPassword: {password}")
    print(f"Entropy: {entropy:.2f} bits")
    print("Strength:", strength)

   with open("summary.log", "a") as log:
        log.write(f"[{timestamp}] Password: {password} | Entropy: {entropy:.2f} | Strength: {strength}\n")

Run
pwd = input("Enter password to analyze: ")
analyze_password(pwd)


# THE CODE FOR THE PASSWORD GENERATOR :

from utils import leetspeak_variants, add_suffixes, combine_inputs
from datetime import datetime

name = input("Enter your name: ").lower().strip()
dob = input("Enter your date of birth (YYYYMMDD): ").strip()
pet = input("Enter your pet name: ").lower().strip()

base = [name, dob, pet]
variants = set(base)

for word in base:
    variants.update(leetspeak_variants(word))
    variants.update(add_suffixes(word))

variants.update(combine_inputs(name, dob, pet))

with open("wordlist.txt", "w") as f:
    for word in sorted(variants):
        f.write(word + "\n")

count = len(variants)
timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

print(f"\n✅ Generated {count} advanced wordlist entries and saved to wordlist.txt")

 Save stats
with open("summary.log", "a") as log:
    log.write(f"[{timestamp}] Wordlist generated with {count} entries using inputs: {name}, {dob}, {pet}\n")


# THE CODE FOR THE GUI LAUNCHER :

import tkinter as tk
import subprocess

def run_analyzer():
    subprocess.call(["python3", "analyzer.py"])

def run_wordlist():
    subprocess.call(["python3", "wordlist_gen.py"])

root = tk.Tk()
root.title("Password Tool Launcher")

tk.Label(root, text="Advanced Password Tool", font=("Arial", 14)).pack(pady=10)
tk.Button(root, text="Analyze Password", command=run_analyzer, width=25).pack(pady=5)
tk.Button(root, text="Generate Wordlist", command=run_wordlist, width=25).pack(pady=5)

root.mainloop()

THANKYOU

