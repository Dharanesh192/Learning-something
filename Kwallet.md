# KWallet Management and VS Code Integration

This document explains how **KWallet** (KDE's credential manager) works and how **Visual Studio Code** utilizes it to securely store your passwords, tokens, and session keys on Linux.

---

## 1. What is KWallet?
KWallet is the central credential management system for the KDE Plasma desktop environment. It provides a secure way to store sensitive information such as:
*   Wi-Fi passwords
*   Web form passwords
*   API tokens (like GitHub or Azure)
*   SSH key passphrases
*   Apps (Tokens/password)

### How it works:
*   **Encryption:** All data in a wallet is encrypted using **Blowfish** or **GPG**.
*   **Access Control:** Applications must request access to the wallet. The first time an app requests a secret, KWallet will ask for your permission.
*   **Wallets:** You can have multiple wallets (e.g., one for work, one for personal), but most users use the default `kdewallet`.

---

## 2. How VS Code Uses KWallet
VS Code uses a feature called **Secret Storage** to handle sensitive data like GitHub authentication tokens, Settings Sync credentials, and extension-specific keys.

### The Connection:
VS Code doesn't talk to KWallet directly. Instead, it uses a standardized bridge:
1.  **VS Code** calls the **Secret Service API** (a standard Linux D-Bus interface).
2.  **libsecret** is the library VS Code uses to communicate with this API.
3.  **KWallet** (specifically modern versions using KDE Frameworks 5.97+) implements this Secret Service interface.
```Simply it uses System API to call ask the K manager.```

### Visual Workflow Tree:
```text
VS Code Secret Storage
├── 1. Action: VS Code needs to save/load a secret (Token/password) (e.g. GitHub Token)
|
├── 2. Internal Engine: Electron requests the "Safe Storage Key" (Perpare the API call)
|
├── 3. Bridge: VS Code calls `libsecret` library 
│   └── libsecret sends D-Bus message to `org.freedesktop.secrets` (Send the message to Kmanager to store the data in the KWallet)
|
├── 4. Provider (KWallet): Receives the D-Bus request (VS code request the data from the Kmanager)
│   ├── If Wallet Locked: System prompts user for Master Password (Show a window and ask the user to enter the password)
│   └── If Wallet Unlocked: KWallet accesses "Code Safe Storage" entry (directly give the code)
|
├── 5. Physical Storage: Key is encrypted & saved to disk (~/.local/share/kwalletd/)
|
└── 6. Result: VS Code uses the key to encrypt/decrypt local session data
```

### What is actually stored?
VS Code typically stores a **Safe Storage Key**. This key is used by VS Code's internal encryption engine (derived from Chromium/Electron) to encrypt the actual data before it is saved in your user profile. 
*   **KWallet Entry Name:** Usually found under a folder named `Secret Service` or `Chrome Safe Storage`.
*   **Key Label:** Often labeled as `Code Safe Storage`.

---

## 3. Why is it necessary?
If KWallet is not available or locked:
*   **Repetitive Logins:** VS Code will ask you to sign in to GitHub or your Microsoft account every time you restart the application.
*   **In-Memory Fallback:** Without a persistent "keyring" (like KWallet or GNOME Keyring), VS Code falls back to storing secrets in memory only, which are lost when the app closes.

---

## 4. Managing KWallet
You can manage your secrets using the **KWalletManager** application.

*   **View Secrets:** Open KWalletManager -> Open your wallet (`kdewallet`) -> Look for `Secret Service`.
*   **Auto-Unlock:** To avoid entering your password every time, ensure your KWallet password matches your Linux login password. Most distributions use `pam_kwallet` to unlock the wallet automatically when you log in.

---

## 5. Troubleshooting VS Code & KWallet
If VS Code fails to save your credentials, try these steps:

### 1. Install `libsecret`
Ensure the necessary communication library is installed:
```bash
# Arch Linux
sudo pacman -S libsecret

# Ubuntu/Debian
sudo apt install libsecret-1-0
```

### 2. Force the Backend
You can tell VS Code which password store to use. 
1. Open VS Code.
2. Open the Command Palette (`Ctrl+Shift+P`).
3. Search for **"Preferences: Configure Runtime Arguments"**.
4. Add the following line to the `argv.json` file:
   ```json
   "password-store": "gnome-libsecret"
   ```
   *(Note: Even on KDE, `gnome-libsecret` is used because it refers to the standard library interface.)*

### 3. Check Wallet Status
Make sure the wallet is "Enabled" in **System Settings** -> **Security & Privacy** -> **KDE Wallet**.

---
