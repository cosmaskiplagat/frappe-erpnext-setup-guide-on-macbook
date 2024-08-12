Setting up Frappe/ERPNext on a MacBook



### Comprehensive Guide to Setting Up Frappe/ERPNext on macOS (MacBook Pro)

#### Prerequisites
- **Python**: ≥ 3.10
- **Node.js**: 18
- **MariaDB**: ≥ 10.6.6
- **Redis**: For caching and real-time updates
- **Yarn**: JS dependency manager
- **wkhtmltopdf**: For PDF generation
- **Cron**: For scheduled jobs (macOS uses `launchd` for scheduling tasks)
- **NGINX**: Optional for production setup

#### 1. Setting Up the Environment

**1.1 Install Xcode Command Line Tools**
```bash
xcode-select --install
```

**1.2 Install Homebrew**
Homebrew is a package manager for macOS:
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**1.3 Install Required Packages**
Use Homebrew to install essential packages:
```bash
brew install python@3.11 git redis mariadb@10.6 node wkhtmltopdf pipx
```
Add MariaDB and Redis to your PATH:
```bash
echo 'export PATH="/opt/homebrew/opt/mariadb@10.6/bin:$PATH"' >> ~/.zshrc
echo 'export PATH="/opt/homebrew/opt/redis@6.2/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**1.4 Set MariaDB Root Password**
Secure your MariaDB installation:
```bash
sudo /opt/homebrew/opt/mariadb@10.6/bin/mysqladmin -u root password 1234
```

**1.5 Configure MariaDB**
Edit the MariaDB configuration file:
```bash
nano /opt/homebrew/etc/my.cnf.d/frappe.cnf
```
Add the following configuration:
```ini
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
bind-address = 127.0.0.1

[mysql]
default-character-set = utf8mb4
```
Restart MariaDB to apply the changes:
```bash
brew services restart mariadb@10.6
```

**1.6 Install Node Version Manager (NVM)**
Manage multiple Node.js versions:
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
source ~/.zshrc
nvm install 18
nvm use 18
```

**1.7 Install Essential Tools**
Install Frappe Bench, Node.js package manager (Yarn), and process manager (Honcho):
```bash
pipx install frappe-bench
pipx install honcho
npm install -g yarn
```
Run the following to ensure paths are set:
```bash
pipx ensurepath
source ~/.zshrc
```

#### 2. Initialize Frappe/ERPNext

**2.1 Create and Set Up Bench Instance**
Create a new bench directory and initialize Frappe:
```bash
cd ~/Code/Bench  # Navigate to your preferred directory
bench init --python python3.11 --no-backups develop
cd develop/
bench get-app erpnext
bench new-site kigen.local --install-app erpnext
```

**2.2 Start the Frappe Server**
```bash
bench start
```

**2.3 Access Your ERPNext Site**
After setup, access your site via:
- [http://127.0.0.1:8000](http://127.0.0.1:8000)
- [http://kigen.local:8000](http://kigen.local:8000)

#### 3. Special Considerations

**3.1 Avoid "Python Ended Unexpectedly" Errors**
Add this line to your shell configuration file (e.g., `~/.zshrc`):
```bash
export OBJC_DISABLE_INITIALIZE_FORK_SAFETY=YES
```

**3.2 Ensure Correct Node Version**
If using multiple Node.js versions:
```bash
nvm use 18
ln -s $(which node) env/bin/node
ln -s $(which yarn) env/bin/yarn
```

**3.3 Initialize Bench with Older Versions**
For older versions of Frappe or ERPNext:
```bash
brew install python@3.10
nvm install 16
nvm use 16
npm install -g yarn
bench init --python python3.10 --no-backups --frappe-branch version-14-hotfix version-14
cd version-14
bench get-app erpnext --branch version-14-hotfix
```



This guide provides a detailed process to set up Frappe/ERPNext on your MacBook M1 Pro, tailored to your preferences for passwords and site names. 


Great guide! It’s thorough and covers all the necessary steps for setting up Frappe/ERPNext on macOS. Here’s a quick checklist based on your guide to ensure everything is set up correctly:

### Checklist

1. **Xcode Command Line Tools**
   - [ ] Installed with `xcode-select --install`

2. **Homebrew**
   - [ ] Installed with the provided script

3. **Packages Installation**
   - [ ] Python, Git, Redis, MariaDB, Node.js, wkhtmltopdf, pipx installed via Homebrew
   - [ ] MariaDB and Redis paths added to `~/.zshrc`

4. **MariaDB Configuration**
   - [ ] Root password set with `mysqladmin`
   - [ ] Configuration file updated
   - [ ] MariaDB restarted

5. **Node Version Manager (NVM)**
   - [ ] NVM installed
   - [ ] Node.js version 18 installed and in use

6. **Essential Tools**
   - [ ] Frappe Bench, Honcho, and Yarn installed
   - [ ] Paths ensured with `pipx ensurepath`

7. **Frappe/ERPNext Initialization**
   - [ ] Bench instance created
   - [ ] ERPNext app installed
   - [ ] Site created and started

8. **Accessing the Site**
   - [ ] Site accessible via `http://127.0.0.1:8000` and `http://kigen.local:8000`

9. **Special Considerations**
   - [ ] `OBJC_DISABLE_INITIALIZE_FORK_SAFETY` set if needed
   - [ ] Correct Node.js version in use and linked properly

10. **Older Versions (if applicable)**
    - [ ] Python 3.10 and Node.js 16 setup
    - [ ] Bench initialized with older versions as specified

If you need any tweaks or additional details, let me know!
