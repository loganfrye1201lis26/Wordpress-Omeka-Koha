## Overview
For this assignment, I installed the Koha integrated library system and configured it so the OPAC is publicly accessible. I also linked the Koha catalog from my WordPress site and documented issues and resolutions during the process.

## Server network Setup
- **Platform:** Google Cloud VM (Compute Engine)
- **OS:** Ubuntu 22.04 LTS
- **Machine type:** e2-medium (2 vCPU, 4 GB RAM)
- **Disk:** 20 GB
- **Network tag:** `koha-8080`
- **Firewall rule:** `koha-opac` (tcp:8080 open from `0.0.0.0/0`)

# Installation Steps
1. **Update and prepare the system**
   [sudo apt update
   sudo apt upgrade
   sudo apt autoremove -y && sudo apt clean
   sudo apt install gnupg2 apt-transport-https]
2. **Add Koha repository and key**
    [echo 'deb http://debian.koha-community.org/koha stable main' | sudo tee /etc/apt/sources.list.d/koha.list
   wget -qO- https://debian.koha-community.org/koha/gpg.asc | gpg --dearmor | sudo tee /etc/apt/trusted.gpg.d/koha.gpg > /dev/null
   gpg --show-keys /etc/apt/trusted.gpg.d/koha.gpg
   sudo apt update]
3. **Install Koha and prerequisites**
  [sudo apt show koha-common
   sudo apt install koha-common
   sudo apt install mysql-server
   sudo mysqladmin -u root password]
4. **Configure Koha ports**
   [sudo cp /etc/koha/koha-sites.conf /etc/koha/koha-sites.conf.backup
   sudo nano /etc/koha/koha-sites.conf]   # change INTRAPORT from 80 to 8080
5. **Enable Apache modules and create Koha instance**
   [ sudo a2enmod rewrite
   sudo a2enmod cgi
   sudo systemctl restart apache2]

   [sudo koha-create --create-db bibliolib
   sudo apachectl configtest]
6. **Enable the Koha site**
   [sudo a2dissite 000-default
   sudo a2ensite bibliolib
   sudo systemctl reload apache2
   sudo systemctl restart apache2]
7. **Verify instance**
   [sudo koha-list]
   - Should show "bibliolib"

# Web Installer and Onboarding
1. **Access staff web installer**
- Navigate to: `http://34.46.254.243:8080`
   - Log in with the database username/password from `/etc/koha/sites/bibliolib/koha-conf.xml`.
2. **Installer steps**
- Choose language.
- Confirm Perl modules.
- Accept database settings and let Koha create tables.
- Select **MARC21** and install recommended sample data.
- Proceed to the onboarding tool.
3. **Onboarding**
- Create first library (e.g., code `MAIN`).
- Add at least one patron category.
- Create Koha administrator (staff) user.
- Log into the staff client with this admin account at `http://34.46.254.243:8080`.

# OPAC Configuration and Wordpress Link
1. **Set public OPAC URL in Koha**
- In the staff client: More → Administration → System Preferences → **OPAC**.
- Set `OPACBaseURL` to: `http://34.46.254.243` (no trailing slash).
- Ensure `OpacPublic` is enabled so anyone can access the catalog.
2. **Add link from WordPress**
- In the WordPress dashboard, go to Appearance → Menus (or the equivalent).
   - Add a **Custom Link** with:
     - URL: `http://34.46.254.243`
   - Save the menu.

## Issues Encounters & Resolutions
- **Symptom:** Visiting `http://34.46.254.243:8080` did not initially show the Koha web installer.
- **Checks/Actions:**
  - Verified instance exists with `sudo koha-list` (`bibliolib` present).
  - Confirmed `Listen 8080` was added to `/etc/apache2/ports.conf` and `apachectl configtest` returned `Syntax OK`.
  - Ensured `bibliolib` site was enabled and `000-default` disabled, then restarted Apache.
- **Resolution:** After correcting configuration and restarting services, the staff interface/installer became available at the expected URL.

## Reflection
Following only the written documentation to install Koha forced me to pay close attention to each step and to the relationships among the services (Apache, MySQL, Koha). Without a video walkthrough, I relied on error messages, config tests, and the official Koha and course documentation to debug issues, such as the staff interface not appearing on the expected port. This reinforced the value of tools like `apachectl configtest` and `koha-list` as quick sanity checks.

The process also highlighted how small configuration details matter—for example, changing the intranet port to 8080, adding `Listen 8080` in Apache, and setting `OPACBaseURL` correctly so the public catalog is reachable. Working through these steps made me more comfortable reading configuration files and tracing how a web application is exposed through the server stack. Overall, doing this with text-only documentation was slower than following a video, but it gave me a deeper, more transferable understanding of Koha’s architecture and the underlying Linux and web server tools.
