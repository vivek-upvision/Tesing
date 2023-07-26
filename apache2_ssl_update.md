# **Apache2 SSL certificate update**

## Step 1 : SSH to *sysops-monitor-01*

1. SSH to *sysops-monitor-01* (64.227.99.230), which holds the certificate and key.

        ssh -i .ssh/fulcrum-do-20201214 root@64.227.99.230

2. Run the script to copy certificate and key from *sysops-monitor-01* (64.227.99.230) to the *target_machine*.
        
        /var/local/fulcrumsaas/certs/letsencrypt/cpremote.sh -t 45.55.60.214  -o com

## Step 2 : To update SSL certificate of Apache2

1. SSH to *target_machine* (45.55.60.214)

        ssh -i .ssh/fulcrum-do-20201214 root@45.55.60.214

2. Check if certificate and key copied from *sysops-monitor-01* (64.227.99.230)

        ls -la /var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.com/

3. Update configuration file of Apache2

        nano /etc/apache2/sites-enabled/000-default.conf

    Update the following lines

        ......
        ..........
        SSLCertificateFile      /var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.com/fullchain.pem
        SSLCertificateKeyFile   /var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.com/privkey.pem
        .........

4. Reload Apache2 service

        service apache2 reload

    Check status of service

        service apache2 status