### Complete MariaDB/MySQL Removal on Fedora

#### 1\. Stop MariaDB Service
```bash
sudo systemctl stop mariadb
```

- Stops the running MariaDB service to prevent conflicts during removal.
#### 2\. Remove MariaDB Packages
```bash
sudo dnf remove mariadb mariadb-server mariadb-libs
```
- Removes the main MariaDB packages (`mariadb`, `mariadb-server`, and `mariadb-libs`).
- Also removes dependent packages (e.g., `akonadi-server-mysql`, `qt6-qtbase-mysql`) and unused dependencies.
#### 3\. Delete Data and Configuration Files
```bash
sudo rm -rf /var/lib/mysql
sudo rm -rf /etc/my.cnf /etc/my.cnf.d
```
- Removes the MariaDB data directory (`/var/lib/mysql`) and configuration files (`/etc/my.cnf`, `/etc/my.cnf.d`).
#### 4\. Clean Up Logs and Temporary Files
```bash
sudo rm -rf /var/log/mysql*
sudo rm -rf /var/log/mariadb /var/lib/mysql-files /var/lib/mysql-keyring
```

- Deletes log files (`/var/log/mysql*`, `/var/log/mariadb`) and auxiliary directories (`mysql-files`, `mysql-keyring`).
#### 5\. Remove History File
```bash
sudo rm -f /root/.mysql_history
```
- Deletes the MySQL command history file for the root user.
#### 6\. Remove Firewall and Documentation Files
- Removes the firewall rule for MySQL and leftover documentation/license files.
#### 7\. Remove Miscellaneous Files
- Cleans up sample scripts, SELinux policies, and shared library files.
#### 8\. Run Autoremove (Optional)
```bash
sudo dnf autoremove
```
- Removes any leftover dependencies no longer needed by the system.
---
### Notes

- This process **completely removes** MariaDB/MySQL, including all data and configurations.
- Backup your databases before running these commands if you need to preserve data.
- If you plan to reinstall MariaDB later, use:
```bash
sudo dnf install mariadb mariadb-server
sudo systemctl enable --now mariadb
```
Save this note for future reference! Let me know if you'd like to add anything else.
