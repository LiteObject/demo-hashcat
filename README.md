# Hashcat Basics

Hashcat is a powerful password recovery tool that supports various hashing algorithms and cracking methods. Here are some essential **Hashcat commands** and concepts that can help you be successful:

---

### **1. Basic Syntax**
The general syntax for running Hashcat:
```bash
hashcat -m [hash_type] -a [attack_mode] -o [output_file] [hash_file] [wordlist_or_rules]
```
- **`-m`**: Hash type (e.g., MD5, SHA1, bcrypt).
- **`-a`**: Attack mode (e.g., dictionary, brute-force).
- **`-o`**: Output file for cracked hashes.
- **`[hash_file]`**: File containing the hashes.
- **`[wordlist_or_rules]`**: Wordlist or rules file (depends on the mode).

---

### **2. Important Hashcat Options**
- **`-m` (Hash Type)**: Defines the type of hash to crack.  
  Example:  
  - `0` for MD5  
  - `100` for SHA1  
  - `3200` for bcrypt  
  Full list: [Hashcat Hash Types](https://hashcat.net/wiki/doku.php?id=hashcat#hash_types)

- **`-a` (Attack Mode)**: Defines the cracking method:
  - `0`: Dictionary attack  
  - `1`: Combination attack  
  - `3`: Brute-force attack  
  - `6`: Hybrid dictionary + mask  
  - `7`: Hybrid mask + dictionary

- **`--force`**: Forces Hashcat to run on unsupported devices (use cautiously).

- **`--session [name]`**: Saves the current session so you can resume later.

- **`--restore`**: Resumes a saved session.

- **`--show`**: Displays cracked hashes from the output file.

---

### **3. Example Commands**
#### **Dictionary Attack**
```bash
hashcat -m 0 -a 0 -o cracked.txt hashes.txt rockyou.txt
```
- `-m 0`: MD5 hash type.
- `-a 0`: Dictionary attack.
- `hashes.txt`: File with hashes.
- `rockyou.txt`: Wordlist file.

#### **Brute-Force Attack**
```bash
hashcat -m 0 -a 3 -o cracked.txt hashes.txt ?a?a?a?a
```
- `?a`: Represents any printable ASCII character.
- You can adjust the length (e.g., `?a?a?a?a?a` for 5 characters).

#### **Combination Attack**
```bash
hashcat -m 0 -a 1 -o cracked.txt hashes.txt wordlist1.txt wordlist2.txt
```
- Combines words from `wordlist1.txt` and `wordlist2.txt`.

#### **Hybrid Dictionary + Mask Attack**
```bash
hashcat -m 100 -a 6 -o cracked.txt hashes.txt rockyou.txt ?d?d?d
```
- Appends three digits (`?d?d?d`) to each word in the dictionary.

#### **Hybrid Mask + Dictionary Attack**
```bash
hashcat -m 100 -a 7 -o cracked.txt hashes.txt ?d?d rockyou.txt
```
- Prepends two digits (`?d?d`) to each word in the dictionary.

---

### **4. Customizing Masks**
Mask characters allow you to specify the charset:
- `?l`: Lowercase letters (a-z).
- `?u`: Uppercase letters (A-Z).
- `?d`: Digits (0-9).
- `?s`: Special characters.
- `?a`: All of the above.
- Example: `?u?l?l?l?d?d` (e.g., `Aaaa11`).

---

### **5. Speed Optimization**
- **`--optimized-kernel-enable`**: Speeds up attacks on simpler hash algorithms.
- **`-w`**: Workload profile:
  - `-w 1`: Low power (used for multitasking).
  - `-w 4`: High power (max performance).

---

### **6. Rules**
Rules modify the words in your wordlist dynamically:
```bash
hashcat -m 0 -a 0 -o cracked.txt hashes.txt -r rules/best64.rule
```
- Popular rule files include `best64.rule` and `d3ad0ne.rule`.

---

### **7. Session Management**
- Save session:
  ```bash
  hashcat --session mysession -m 0 -a 3 hashes.txt ?a?a?a
  ```
- Restore session:
  ```bash
  hashcat --restore --session mysession
  ```

---

### **8. Benchmarking**
Test your system's performance with different hash types:
```bash
hashcat -b
```

---

### **9. GPU Configuration**
Ensure GPU acceleration is enabled:
```bash
hashcat -I
```
- Use `-D` to select the device type:
  - `-D 1`: Use CPU.
  - `-D 2`: Use GPU.

---

### Generate Hashes
Generate your own hashes using Python:
```python
import hashlib

# MD5 hash
print(hashlib.md5(b'test').hexdigest())

# SHA1 hash
print(hashlib.sha1(b'test').hexdigest())

```

### **10. Tips for Success**
1. Use the **right hash type** for your target.
2. Experiment with **attack modes** based on the scenario.
3. Leverage **wordlists** and **rules** for better results.
4. Use **session management** for long-running jobs.
5. Test your setup using the **benchmark** mode.
6. Keep your Hashcat updated for new features and improved performance.

---
### Resources
- [Hashcat Wiki](https://hashcat.net/wiki/)
- [Examples Hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)
- [RockYou Wordlist](https://github.com/danielmiessler/SecLists/blob/master/Passwords/Leaked-Databases/rockyou.txt.tar.gz)
- [Rule Files](https://github.com/hashcat/hashcat/tree/master/rules)
- [CrackStation](https://crackstation.net/)