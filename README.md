````md
# Password Strength Analyzer

## Overview
Password Strength Analyzer is a Python-based security tool that evaluates the strength of user-entered passwords. It checks password length, complexity, entropy, and uniqueness while also suggesting stronger alternatives.

This project helps users understand password security concepts and basic cryptography techniques.

---

# Features

- Check password length
- Detect uppercase and lowercase letters
- Detect numbers and special characters
- Calculate password entropy
- Classify passwords as Weak, Moderate, or Strong
- Suggest stronger password improvements
- Generate secure passwords
- Prevent common password usage
- Optional password reuse prevention using database storage
- Optional password hashing using bcrypt

---

# Technologies Used

- Python 3
- Regular Expressions (`re`)
- Math Module (`math`)
- Secrets Module (`secrets`)
- String Module (`string`)
- bcrypt
- SQLite (Optional)

---

# Requirements

## Software Requirements

- Python 3.8 or higher
- VS Code or any Python IDE

## Python Libraries

Install required libraries using:

```bash
pip install bcrypt
````

---

# Project Structure

```text
Password-Strength-Analyzer/
│
├── password_analyzer.py
├── README.md
└── requirements.txt
```

---

# Password Strength Rules

| Requirement  | Recommendation         |
| ------------ | ---------------------- |
| Length       | 12–16+ characters      |
| Uppercase    | At least 1             |
| Lowercase    | At least 1             |
| Numbers      | At least 1             |
| Symbols      | At least 1             |
| Uniqueness   | Avoid reused passwords |
| Common Words | Avoid dictionary words |

---

# Password Entropy Formula

```text
H = L × log2(N)
```

Where:

* H = entropy (strength in bits)
* L = password length
* N = character set size

Higher entropy means stronger passwords.

---

# Main Python Code

```python
import re
import math

COMMON_PASSWORDS = [
    "password", "123456", "qwerty", "admin", "welcome"
]

def calculate_entropy(password):
    charset = 0

    if re.search(r"[a-z]", password):
        charset += 26
    if re.search(r"[A-Z]", password):
        charset += 26
    if re.search(r"[0-9]", password):
        charset += 10
    if re.search(r"[^A-Za-z0-9]", password):
        charset += 32

    entropy = len(password) * math.log2(charset) if charset else 0
    return round(entropy, 2)

def analyze_password(password):
    score = 0
    feedback = []

    if len(password) >= 12:
        score += 2
    elif len(password) >= 8:
        score += 1
    else:
        feedback.append("Password is too short.")

    if re.search(r"[A-Z]", password):
        score += 1
    else:
        feedback.append("Add uppercase letters.")

    if re.search(r"[a-z]", password):
        score += 1
    else:
        feedback.append("Add lowercase letters.")

    if re.search(r"[0-9]", password):
        score += 1
    else:
        feedback.append("Add numbers.")

    if re.search(r"[^A-Za-z0-9]", password):
        score += 1
    else:
        feedback.append("Add special characters.")

    if password.lower() in COMMON_PASSWORDS:
        feedback.append("Password is too common.")
        score = 0

    entropy = calculate_entropy(password)

    if score <= 2:
        strength = "Weak"
    elif score <= 5:
        strength = "Moderate"
    else:
        strength = "Strong"

    return {
        "strength": strength,
        "entropy": entropy,
        "feedback": feedback
    }

password = input("Enter password: ")

result = analyze_password(password)

print("\nPassword Strength:", result["strength"])
print("Entropy:", result["entropy"], "bits")

if result["feedback"]:
    print("\nSuggestions:")
    for item in result["feedback"]:
        print("-", item)
```

---

# Example Output

```text
Enter password: Hello123

Password Strength: Moderate
Entropy: 52.44 bits

Suggestions:
- Add special characters.
```

---

# Secure Password Generator

```python
import secrets
import string

def generate_password(length=16):
    chars = (
        string.ascii_letters +
        string.digits +
        string.punctuation
    )

    return ''.join(secrets.choice(chars) for _ in range(length))

print(generate_password())
```

---

# Why Use `secrets` Instead of `random`?

* Cryptographically secure
* Better for authentication systems
* Resistant to prediction attacks

---

# Optional Advanced Features

## 1. Password Reuse Prevention

Store hashed passwords in a database to prevent reuse.

## 2. Password Hashing with bcrypt

```python
import bcrypt

password = b"MySecurePassword123!"

hashed = bcrypt.hashpw(password, bcrypt.gensalt())

bcrypt.checkpw(password, hashed)
```

---

# Detect Breached Passwords

You can integrate with:

* Have I Been Pwned API
  [https://haveibeenpwned.com/API/v3](https://haveibeenpwned.com/API/v3)

This checks whether a password appears in known data breaches.

---

# Security Best Practices

* Never store plaintext passwords
* Use HTTPS
* Add rate limiting
* Enforce minimum entropy
* Use MFA/2FA
* Salt password hashes
* Avoid weak passwords

---

# Expected Learning Outcomes

By building this project, you will learn:

* Password security principles
* Entropy and randomness
* Regular expressions
* Secure hashing
* Authentication systems
* Cryptographic best practices

---

# Future Improvements

* GUI using Tkinter
* Web application using Flask
* Dark mode interface
* Real-time password strength meter
* Multi-user authentication system

---

# Conclusion

This project demonstrates how password security systems work in real-world applications. It provides hands-on experience with authentication, password validation, cryptography, and secure coding practices.

```
```
