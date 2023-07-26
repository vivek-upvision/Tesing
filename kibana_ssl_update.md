# **Kibana SSL certificate update**

## Step 1 : SSH to *sysops-monitor-01*

1. SSH to *sysops-monitor-01* (64.227.99.230), which holds the certificate and key.

        ssh -i .ssh/fulcrum-do-20201214 root@64.227.99.230

2. Run the script to copy certificate and key from *sysops-monitor-01* (64.227.99.230) to the *target_machine*.
        
        /var/local/fulcrumsaas/certs/letsencrypt/cpremote.sh -t 161.35.186.145  -o net

## Step 2 : To update SSL certificate of Kibana

1. SSH to *target_machine* (161.35.186.145)

        ssh -i .ssh/fulcrum-do-20201214 root@161.35.186.145

2. Check if certificate and key copied from *sysops-monitor-01* (64.227.99.230)

        ls -la /var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.net/

3. Update configuration file of Kibana

        nano /etc/kibana/kibana.yml

    Update the following lines

        ......
        ..........
        server.ssl.certificate: /var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.net/fullchain.pem
        server.ssl.key: /var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.net/privkey.pem
        .........

4. Restart Kibana service

    Check status of service

       service kibana status

    Restart service

       service kibana restart
       

    Check status of service

        service kibana status