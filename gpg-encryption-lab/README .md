# GPG Encryption Lab

## Overview

After completing the **Public Key Cryptography** room on TryHackMe, 
I ran this hands-on lab to practice GPG (GNU Privacy Guard) on my 
Kali Linux machine.

GPG is an open-source encryption tool that implements public key 
cryptography. This lab covers the full lifecycle — key generation, 
encryption, decryption, backup, and key recovery.

---

## Tools Used

- Kali Linux
- GPG 2.4.8 (GnuPG)
- TryHackMe — Public Key Cryptography room

---

## Lab Objectives

- Generate an ECC key pair
- Encrypt a file using a public key
- Decrypt a file using a private key
- Back up a private key
- Simulate key loss and recover from backup
- Confirm decryption still works after key restore

---

## Steps

### Step 1 — Generate Key Pair

```bash
gpg --full-generate-key
```

Selected ECC (sign and encrypt) — default in GPG 2.4+  
Curve: Curve 25519  
Expiry: 0 (no expiry)  
Name: Test User  
Email: test@lab.com  

![Key Generation](screenshots/1-key-generation.png)

---

### Step 2 — Export Keys

```bash
# Public key
gpg --export --armor test@lab.com > public.key

# Private key (backup)
gpg --export-secret-keys --armor test@lab.com > backup.key
```

---

### Step 3 — Create and Encrypt a Message

```bash
echo "This is my encrypted test message" > messages.txt
gpg --encrypt --recipient test@lab.com messages.txt
```

Output: `messages.txt.gpg`

![Encryption](screenshots/2-encryption.png)

---

### Step 4 — Decrypt the File

```bash
gpg --decrypt messages.txt.gpg
```

Enter passphrase → original message appears.

![Decryption](screenshots/3-decryption.png)

---

### Step 5 — Delete Keys (Simulate New Machine)

```bash
gpg --delete-secret-keys test@lab.com
gpg --delete-keys test@lab.com
```

![Delete Keys](screenshots/4-delete-keys.png)

---

### Step 6 — Restore from Backup

```bash
gpg --import backup.key
```

---

### Step 7 — Decrypt Again to Confirm Restore

```bash
gpg --decrypt messages.txt.gpg
```

Same message appears — backup and restore confirmed working.

---

## What the Encrypted File Looks Like Without the Key

Running `cat messages.txt.gpg` without the private key returns 
pure gibberish — unreadable binary data. This is what encryption 
is supposed to look like.

![Encrypted File Contents](screenshots/5-encrypted-file-contents.png)

---

## Key Takeaway

The backup and restore step is the one most people skip in labs. 
In a real environment, if your private key is gone and you have 
no backup, every file ever encrypted to you is permanently 
unreadable. No recovery exists for that.

---

## Reference

- [TryHackMe — Public Key Cryptography](https://tryhackme.com/room/publickeycrypto)
- [GnuPG Official Documentation](https://gnupg.org/documentation/)
