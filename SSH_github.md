## Genarating an ssh key for github use

### 1. <u>step one</u>
- Open a terminal on your computer.
- Type the following command: 
  ```bash
    ssh-keygen -t ed25519 -C "your_email@example.com"
  ```
- Replace `"your_email@example.com"` with your own email address. This command will generate a new SSH key using the Ed25519 algorithm.
  - You will be prompted to enter a file path to save the new key. You can either accept the default path or specify a custom one. Press Enter to accept the default path.
  - You will be prompted to enter a passphrase for the key. This is optional, but adding a passphrase provides an extra layer of security. If you choose to use a passphrase, enter it and confirm it when prompted. If you don't want to use a passphrase, simply press Enter.
- Your new SSH key has been generated and saved to the specified file path. The file name should be **id_ed25519** (`private key`) and **id_ed25519.pub** (`public key`).

### 2. <u>step two</u>
- Now you need to add the public key to your GitHub account. Copy the contents of the id_ed25519.pub file by running the following command: 
    ```bash
      cat ~/.ssh/id_ed25519.pub | pbcopy.
    ``` 
- This will copy the public key to your clipboard.
### 3. <u>step three</u>
- Go to your GitHub account settings, click on "SSH and GPG keys", and click on "New SSH key". Paste the contents of the clipboard into the "Key" field and give the key a descriptive title.
- Click "Add SSH key" and enter your GitHub account password if prompted.
- You can now use your SSH key with the GitHub CLI by running 
    ```bash
    gh auth login
    ```
  and choosing the "SSH" option when prompted.