# AWS Lightsail WordPress Multisite POC

## Overview
Setup of a WordPress multisite instance on AWS Lightsail. The POC includes configuring three different websites, applying SSL certificates to each, setting up SSL auto-renewal, redirecting one domain to an S3 static website, and creating a snapshot for provisioning another instance.

## Steps

1. **Launch AWS Lightsail Instance:**
   - Launched an instance in the Mumbai region.
   - Chose WordPress Multisite blueprint.
   - Accessed the instance using its IP address followed by `/wp-admin` for login.

2. **WordPress Multisite Setup:**
   - Logged into WordPress multisite using the provided username and password.
   - Launched two websites with the domains `freelyy.zapto.org` and `freelyy.hopto.org`.

3. **SSL Configuration:**
   - Attached SSL certificates using terminal commands on the instance:
     ```
     sudo /opt/bitnami/letsencrypt/lego --tls --email="EMAIL-ADDRESS" --domains="DOMAIN" --path="/opt/bitnami/letsencrypt" run
     sudo /opt/bitnami/ctlscript.sh start
     ```

4. **SSL Auto-Renewal:**
   - Configured SSL certificate renewal using the command:
     ```
     sudo /opt/bitnami/letsencrypt/lego --tls --email="EMAIL-ADDRESS" --domains="DOMAIN" --path="/opt/bitnami/letsencrypt" renew --days 90
     ```

## Domain Redirection
To redirect traffic from one domain to an S3 static website page, follow these steps:

1. **Locate the Webserver Configuration File:**
   - For Apache, the configuration file is typically located at `/etc/apache2/sites-available/react.conf`.

2. **Edit the Configuration File:**
   - Open the configuration file using a text editor.
   - Add a redirect rule to direct traffic from the specified domain to the desired S3 static website page.
     ```
     <VirtualHost *:80>
         ServerName freelyy.zapto.org
         Redirect permanent / http://your_s3_static_page_url](https://neelofar.s3.amazonaws.com/index.html
     </VirtualHost>
     ```

3. **Restart the Webserver:**
   
     ```
     sudo systemctl restart apache2
     ```

4. **Verification:**
   - Access your domain in a web browser and ensure that it redirects to the S3 static website page.
   - Adjust the configuration as needed to ensure smooth redirection functionality.

## Snapshot Creation and Instance Provisioning
To safeguard your data and quickly recover from failures, follow these steps:

1. **Take a Snapshot:**
   - Navigate to the AWS Lightsail console.
   - Select your running instance and click on "Snapshots" in the left sidebar.
   - Click on "Create Snapshot" and follow the prompts.
   - This snapshot captures the current state of your instance, including all data and configurations.

2. **Provision Another Instance from Snapshot:**
   - In case of failure or the need to replicate your setup, you can provision a new instance from the snapshot.
   - Navigate to the "Snapshots" section in the Lightsail console.
   - Choose the snapshot you created and click on "Create Instance."
   - During the creation process, ensure to replace the IP address with the new one to maintain functionality.
   - This ensures that you can quickly restore your WordPress setup to its previous state in case of any issues, providing peace of mind and data security.


## Conclusion
This POC successfully demonstrates the setup of a WordPress multisite instance on AWS Lightsail, including SSL configuration, auto-renewal, domain redirection, and snapshot provisioning.

For any queries or assistance, feel free to reach out.

## 🌟 Thank you ! 🌟
