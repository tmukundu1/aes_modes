# AES Encryption & Decryption System: A Ground-Up Implementation

## 📋 Executive Summary
This project was all about moving past just importing a library and calling it a day. I wanted to understand the "cryptographic contract"; how different AES modes (ECB, CBC, CFB, CTR) actually talk to the file system, how they manage initialization vectors (IVs), and why they fail so spectacularly if the environment isn't set up exactly right.


## 💡 Technical Breakthroughs & Engineering Insights

### 1. The "File Contract" (IV Management)
I spent a lot of time stressing over how to pass IVs to the decryption function without manually typing them in every time. I finally settled on a "File Contract" protocol:
* **The Insight:** I decided to just prepend the 16-byte IV to the raw ciphertext during the encryption process. 
* **Why it matters:** It means the decryption function is smart enough to "self-discover" the IV. It just peels off the first 16 bytes and knows exactly how to proceed. It feels like a much cleaner way to handle data.


### 2. Mode-Agnostic Architecture
I started by writing four separate, rigid scripts, but that was a nightmare to maintain. I refactored everything into a single, modular "controller" function. By abstracting the `mode_choice`, the system now handles the messy stuff; like padding, file I/O, and binary handling; in one place, while letting the AES logic do the heavy lifting.


### 3. The "Ground-Up" Philosophy
Building these from scratch for ECB, CBC, CFB, and CTR forced me to finally "get" the difference between block and stream ciphers. It wasn't until I had to manually handle PKCS7 padding for the block modes that I understood why stream-like modes (CFB/CTR) are so much easier to work with in production.

## 🌟 Final Reflection
The highlight of this project was the "Universal Decryption" logic. Being able to run a single command and have the script correctly identify the IV and decrypt the file; no matter which mode I’d used; felt like a real win.

This project turned AES from a "black-box" library call into a manageable system I can actually audit. If there's one thing I’m taking away from this, it’s that **systemic debugging**; checking your paths, validating your data structures, and auditing your assumptions; is the real work. The coding is just the final step.

## 🚀 Technical Summary
* **Library:** `PyCryptodome` (for core AES operations).
* **Environment:** Managed via Conda.
* **Design Pattern:** Unified file-header protocol (IV + Ciphertext).
* **Environment Fixes:** Resolved namespace shadowing and pathing issues via local verification.

---

## 🛠 How to Run
1. Activate your environment.
2. Make sure the dependencies are there: `pip install pycryptodome`
3. Run the script:
   ```bash
   python aes_enc_dec.py [enc|dec] <filename>

---

[View AES Story Notebook](secret_images.ipynb)
