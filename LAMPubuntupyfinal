# Steps to install on ubuntu

# copy the code link, after posting the code as public on git repo

# download the code/file on ubuntu using, "git clone <code link>" command

# open the code directory, run it as "sudo python3 <file>"


import subprocess
import shlex
import sys
import socket

def run_command(command, description=None, shell=False):
    """Runs a shell command with error handling."""
    print(f"\n  {description or 'Running'}: {command}")
    try:
        if shell:
            subprocess.run(command, shell=True, check=True, text=True)
        else:
            subprocess.run(shlex.split(command), check=True, text=True)
        print(" Success")
    except subprocess.CalledProcessError as e:
        print(f" Error: Command failed with exit code {e.returncode}")
        print(f" Output: {e}")
        sys.exit(1)

def install_apache_mysql():
    run_command("sudo apt update -y", "Updating package lists")
    run_command("sudo apt install -y apache2", "Installing Apache2")
    run_command("sudo apt install -y mysql-server", "Installing MySQL Server")

def configure_mysql_users():
    print("\n  Configuring MySQL users (root/root and abhi/power)...")
    sql_commands = """
    ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
    FLUSH PRIVILEGES;
    CREATE USER IF NOT EXISTS 'abhi'@'localhost' IDENTIFIED BY 'power';
    GRANT ALL PRIVILEGES ON *.* TO 'abhi'@'localhost' WITH GRANT OPTION;
    FLUSH PRIVILEGES;
    """
    try:
        subprocess.run(['sudo', 'mysql', '-u', 'root', '-e', sql_commands], check=True)
        print(" MySQL root password set, and user 'abhi' created with full privileges.")
    except subprocess.CalledProcessError as e:
        print(f" Failed to configure MySQL users: {e}")
        sys.exit(1)

def install_php():
    run_command("sudo add-apt-repository -y ppa:ondrej/php", "Adding PHP 8.3 PPA")
    run_command("sudo apt update -y", "Updating package list after adding PHP repo")

    php_packages = (
        "php8.3 libapache2-mod-php8.3 php8.3-curl php8.3-intl php8.3-zip "
        "php8.3-soap php8.3-xml php8.3-gd php8.3-mbstring php8.3-bcmath "
        "php8.3-common php8.3-mysqli"
    )
    run_command(f"sudo apt install -y {php_packages}", "Installing PHP 8.3 and modules", shell=True)

def create_phpinfo():
    print("\n Creating check.php...")
    try:
        with open("/var/www/html/check.php", "w") as f:
            f.write("<?php phpinfo(); ?>\n")
        print(" PHP info file created at /var/www/html/check.php")
    except Exception as e:
        print(f" Failed to create PHP file: {e}")
        sys.exit(1)

def install_phpmyadmin():
    print("\n  Installing phpMyAdmin non-interactively...")

    debconf = """
phpmyadmin phpmyadmin/reconfigure-webserver multiselect apache2
phpmyadmin phpmyadmin/dbconfig-install boolean true
phpmyadmin phpmyadmin/mysql/admin-user string root
phpmyadmin phpmyadmin/mysql/admin-pass password root
phpmyadmin phpmyadmin/mysql/app-pass password root
phpmyadmin phpmyadmin/app-password-confirm password root
"""
    try:
        subprocess.run("sudo debconf-set-selections", input=debconf, text=True, shell=True, check=True)
        run_command("sudo apt install -y phpmyadmin", "Installing phpMyAdmin")
    except subprocess.CalledProcessError as e:
        print(f" Failed to install phpMyAdmin: {e}")
        sys.exit(1)

def print_summary():
    try:
        hostname = socket.gethostname()
        ip = subprocess.check_output("curl -s http://checkip.amazonaws.com", shell=True, text=True).strip()
    except Exception:
        ip = "<your-EC2-public-ip>"

    print("\n📋 ====== SETUP SUMMARY ======\n")
    print("🔐 MySQL Users:")
    print("   - root / root")
    print("   - abhi / power")
    print("\n🛠 phpMyAdmin:")
    print("   - URL     : http://{}/phpmyadmin".format(ip))
    print("   - Login   : root / root OR abhi / power")
    print("\n📄 PHP Info File:")
    print("   - http://{}/check.php".format(ip))
    print("\n🌐 Apache:")
    print("   - http://{}/".format(ip))
    print("\n✅ LAMP + phpMyAdmin setup is complete!\n")

def main():
    print("🚀 Starting LAMP + phpMyAdmin setup on AWS Ubuntu (with custom MySQL users)...\n")
    install_apache_mysql()
    configure_mysql_users()
    install_php()
    create_phpinfo()
    install_phpmyadmin()
    print_summary()

if __name__ == "__main__":
    main()



# 📋 ====== SETUP SUMMARY ======

# 🔐 MySQL Users:
#    - root / root
#    - abhi / power

# 🛠 phpMyAdmin:
#    - URL     : http://13.201.45.88/phpmyadmin
#    - Login   : root / root OR abhi / power

# 📄 PHP Info File:
#    - http://13.201.45.88/check.php

# 🌐 Apache:
#    - http://13.201.45.88/

# ✅ LAMP + phpMyAdmin setup is complete!
