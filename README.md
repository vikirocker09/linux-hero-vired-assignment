# DevOps Environment Setup Report

**Objective:** Secure, monitored, and automated development environment for Sarah and Mike

---

## Task 1: System Monitoring Setup

### Implementation Steps
- Installed monitoring tools:
  ```bash
  sudo yum install epel-release -y
  sudo yum install htop -y
  sudo yum install nmon -y

### Challenges
    - htop not found in default CentOS repo → fixed by enabling EPEL repository.
    - Permission denied writing to /var/log/system_metrics.log → fixed by using root crontab or redirecting logs to user-owned directory.

### Output
<img width="940" height="568" alt="image" src="https://github.com/user-attachments/assets/56906e92-f846-4124-8a91-eddb21491e46" />

<img width="940" height="614" alt="image" src="https://github.com/user-attachments/assets/b241b477-afb6-4afa-b953-cf573b25bfaa" />

<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/192fedce-8e71-4ab5-9bcf-cd4f365d0043" />

<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/b81522ec-3992-4175-9fdd-033613488936" />


## Task 2: User Management and Access Control
### Implementation Steps
- Created user accounts:
  ```bash
  sudo adduser sarah
  sudo adduser mike 
  sudo passwd sarah 
  sudo passwd mike

- Created isolated directories:
  ```bash
  sudo mkdir /home/sarah_dev /home/mike_dev 
  sudo chown sarah:sarah /home/sarah_dev 
  sudo chown mike:mike /home/mike_dev 
  sudo chmod 700 /home/sarah_dev /home/mike_dev

### Challenges
    - Editing /etc/login.defs required root privileges → solved using sudo nano.
    - Policies applied only to new users → enforced for Sarah and Mike using chage.

### Output

<img width="1308" height="153" alt="image" src="https://github.com/user-attachments/assets/5ea86568-f8ea-4baf-8b0f-764562aacd18" />


<img width="940" height="392" alt="image" src="https://github.com/user-attachments/assets/cbff4bd3-0d00-4cac-82e1-cf84fda0d29f" />

<img width="940" height="605" alt="image" src="https://github.com/user-attachments/assets/ce507110-a098-4e8c-aa64-8426afb2eec7" />


## Task 3: Backup Configuration for Web Servers
### Implementation Steps
- Created user accounts:
  ```
  #!/bin/bash
  TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
  BACKUP_DIR="/backups"
  mkdir -p $BACKUP_DIR
  tar -czf $BACKUP_DIR/apache_backup_$TIMESTAMP.tar.gz /etc/httpd/ /var/www/html/
  tar -czf $BACKUP_DIR/nginx_backup_$TIMESTAMP.tar.gz /etc/nginx/ /usr/share/nginx/html/
  tar -tzf $BACKUP_DIR/apache_backup_$TIMESTAMP.tar.gz > $BACKUP_DIR/apache_verify_$TIMESTAMP.log
  tar -tzf $BACKUP_DIR/nginx_backup_$TIMESTAMP.tar.gz > $BACKUP_DIR/nginx_verify_$TIMESTAMP.log
  ```
  
  ```bash
  sudo chmod +x /usr/local/bin/web_backup.sh
  sudo crontab -e
  0 0 * * 2 /usr/local/bin/web_backup.sh
  ls /backups
  cat /backups/apache_verify_*.log
  cat /backups/nginx_verify_*.log

### Challenges
    - Cron job editing failed due to unfamiliar editor → fixed by setting EDITOR=nano.
    - Empty verify logs → resolved by ensuring Apache/Nginx directories exist before backup.

### Output
<img width="940" height="602" alt="image" src="https://github.com/user-attachments/assets/3d666b55-aace-471e-bb4a-d4e8ede2971d" />

<img width="940" height="614" alt="image" src="https://github.com/user-attachments/assets/cd3ed206-d6a9-4c79-86bb-0aa5b50a2876" />

<img width="940" height="615" alt="image" src="https://github.com/user-attachments/assets/57ee7580-c787-4da6-ad56-e8c617814690" />

<img width="940" height="56" alt="image" src="https://github.com/user-attachments/assets/1b6018d8-9e83-46d0-b605-6ea646c7ee5e" />



