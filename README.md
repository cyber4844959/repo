# repo
malware
# Creating a ready-to-upload GitHub repository as a zip file with safe, educational content for a "white hat" profile.
# This will create a folder /mnt/data/ethical-hacking-white-hat and populate it with files,
# then compress into /mnt/data/ethical-hacking-white-hat.zip for you to download and upload to GitHub.

from pathlib import Path
import zipfile
import json
import os

repo_root = Path("/mnt/data/ethical-hacking-white-hat")
if repo_root.exists():
    # remove existing for clean recreate
    import shutil
    shutil.rmtree(repo_root)
repo_root.mkdir(parents=True)

# Files and contents (safe, educational, non-malicious)
files = {
    "README.md": """# Ethical Hacking — White Hat Starter Kit

This repository is a safe, educational starter kit for people who want to learn **ethical hacking** and **cybersecurity** responsibly.

**What's inside**
- `cheatsheets/` — quick command references (Linux, networking, tools).
- `guides/` — beginner-friendly guides, lab setup, and career advice.
- `scripts/` — *safe* automation examples (backups, log parsing). No malware.
- `assets/` — profile assets you can use on your social media.
- `pdf/` — beginner PDF guide (markdown included).
- `CONTRIBUTING.md`, `CODE_OF_CONDUCT.md`, `LICENSE` — project governance.

> ⚠️ **Important**: All content is for educational, defensive, and ethical use only. Do **not** use these techniques to access systems you do not own or have explicit permission to test.

---
""",
    "LICENSE": """MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, subject to the following conditions:
...
""",
    "CONTRIBUTING.md": """# Contributing

Thanks for your interest! Keep contributions:
- Focused on education, documentation, and defensive tooling.
- Free of malware, exploits, or instructions for unauthorized access.
- Clear and well-documented.

Please open issues or PRs describing the change.
""",
    "CODE_OF_CONDUCT.md": """# Code of Conduct

Be respectful. This project is for learning and building a welcoming security community.
""",
    ".gitignore": """# Python
__pycache__/
*.pyc

# macOS
.DS_Store

# VSCode
.vscode/
""",
    "cheatsheets/linux-cheatsheet.md": """# Linux & Terminal Cheatsheet

- List files: `ls -la`
- Check listening ports: `ss -tuln` (or `netstat -tuln`)
- Disk usage: `df -h`
- Tail logs: `tail -f /var/log/syslog`
- Find files: `find / -name 'config.yaml' 2>/dev/null`
- Basic text search: `grep -R "TODO" .`
""",
    "cheatsheets/networking-cheatsheet.md": """# Networking Cheatsheet

- Show routing: `ip route`
- DNS lookup: `dig example.com` or `nslookup example.com`
- Trace path: `traceroute example.com`
- Check open ports (example command): `nmap -sS -p 1-1024 example.com`  
  *Only run nmap against systems you own or have permission to test.*
""",
    "guides/lab-setup.md": """# Lab Setup (safe, local)

1. Use virtualization (VirtualBox, VMware, or QEMU) or containers.
2. Create an isolated network for testing (no internet or with NAT as appropriate).
3. Use intentionally vulnerable VM images for practice (e.g., OWASP Juice Shop, Metasploitable) — only in lab.
4. Snapshot VMs before experiments so you can revert.
5. Learn and follow legal/ethical rules: always have permission.

Recommended tools: Wireshark, Burp Suite (Community), nmap, docker, git.
""",
    "guides/ethical-guidelines.md": """# Ethical Guidelines for White Hat Practitioners

- Always obtain written authorization before testing a system.
- Report findings responsibly to the owner.
- Avoid publishing exploit code for active, unpatched vulnerabilities.
- Respect user privacy and data protection laws.
""",
    "scripts/backup_example.sh": """#!/bin/bash
# Safe example: backup a directory locally
SRC="$HOME/my-notes"
DST="$HOME/backups/my-notes-$(date +%Y%m%d_%H%M%S).tar.gz"
mkdir -p "$(dirname "$DST")"
tar -czf "$DST" -C "$SRC" .
echo "Backup created at: $DST"
""",
    "scripts/log_parse_example.py": """#!/usr/bin/env python3
# Safe example: simple log parser that counts occurrences of a keyword in a log file.
import sys
from collections import Counter

def count_keyword(path, keyword):
    c = Counter()
    with open(path, 'r', encoding='utf-8', errors='ignore') as f:
        for line in f:
            if keyword in line:
                c[keyword] += 1
    return c[keyword]

if __name__ == "__main__":
    if len(sys.argv) < 3:
        print("Usage: log_parse_example.py /path/to/log KEYWORD")
    else:
        print(count_keyword(sys.argv[1], sys.argv[2]))
""",
    "assets/profile_bio_examples.txt": """Security Researcher | White Hat
Open-source enthusiast | Defensive security
Pentester in training (ethics first)
""",
    "pdf/introducao_ethical_hacking.md": """# Introdução ao Ethical Hacking

Este é um guia introdutório seguro sobre conceitos básicos de segurança ofensiva **ética**.

## Conteúdo
- O que é white hat vs black hat
- Preparando um laboratório seguro
- Comandos e ferramentas comuns (ex.: nmap, traceroute) com notas éticas
- Próximos passos na carreira (cursos, certificações)

> Lembre-se: só realize testes em sistemas com autorização.
""",
    "README_REPO_STRUCTURE.md": """# Repo Structure

- /cheatsheets: quick references
- /guides: learning guides and ethics
- /scripts: safe, non-malicious scripts
- /assets: images and bio text
- /pdf: markdown that you can convert to PDF for download or hosting
"""
}

# Create files and directories
for path, content in files.items():
    file_path = repo_root / path
    file_path.parent.mkdir(parents=True, exist_ok=True)
    with open(file_path, "w", encoding="utf-8") as f:
        f.write(content)

# Make scripts executable
(os.chmod(repo_root / "scripts" / "backup_example.sh", 0o755))

# Create a simple HTML profile snippet
profile_html = """<!doctype html>
<html>
<head><meta charset="utf-8"><title>White Hat Profile</title></head>
<body style="font-family: Arial, sans-serif; max-width:700px;margin:auto;">
<h1>White Hat Security — [Seu Nome]</h1>
<p>Security researcher • Ethical hacker in training • Open-source</p>
<ul>
<li>GitHub: https://github.com/SEU_USUARIO</li>
<li>Contact: email@example.com</li>
</ul>
</body>
</html>
"""
with open(repo_root / "assets" / "profile_snippet.html", "w", encoding="utf-8") as f:
    f.write(profile_html)

# Create a zip archive
zip_path = Path("/mnt/data/ethical-hacking-white-hat.zip")
with zipfile.ZipFile(zip_path, 'w', zipfile.ZIP_DEFLATED) as zipf:
    for file in repo_root.rglob('*'):
        zipf.write(file, file.relative_to(repo_root.parent))

# List files created
created = [str(p.relative_to(repo_root)) for p in repo_root.rglob('*') if p.is_file()]

print("Repository prepared at:", repo_root)
print("Zipfile created at:", zip_path)
print("\nFiles included:")
for c in created:
    print("-", c)

# Provide a short JSON with paths to help the user
result = {
    "repo_root": str(repo_root),
    "zip": str(zip_path),
    "files": created
}
print("\n\nJSON-INFO-BEGIN")
print(json.dumps(result))
print("JSON-INFO-END")

