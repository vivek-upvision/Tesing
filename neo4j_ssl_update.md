# **Neo4j SSL certificate update**

## Step 1 : SSH to *sysops-monitor-01*

1. SSH to *sysops-monitor-01* (64.227.99.230), which holds the certificate and key.

        ssh -i .ssh/fulcrum-do-20201214 root@64.227.99.230

2. Run the script to copy certificate and key from *sysops-monitor-01* (64.227.99.230) to the *target_machine*.
        
        /var/local/fulcrumsaas/certs/letsencrypt/cpremote.sh -t 159.89.191.33  -o net

## Step 2 : To update SSL certificate of Neo4j

1. SSH to *target_machine* (159.89.191.33)

        ssh -i .ssh/fulcrum-do-20201214 root@159.89.191.33

2. Check if certificate and key copied from *sysops-monitor-01* (64.227.99.230)

        ls -la /var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.net/

3. Change ownership of certificate and key

        chown -R neo4j:adm /var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.net/

4. Check if ownership of certificate and key changed        

        ls -la /var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.net/

5. Update confinguration file of Neo4j

        nano /etc/neo4j/neo4j.conf

    Update the following lines

        ..........
        ..........
        # Bolt SSL configuration
		dbms.ssl.policy.bolt.enabled=true		
		dbms.ssl.policy.bolt.base_directory=/var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.net/
		dbms.ssl.policy.bolt.private_key=privkey.pem		
		dbms.ssl.policy.bolt.public_certificate=fullchain.pem		
		dbms.ssl.policy.bolt.client_auth=NONE		
				
	    # HTTPS SSL configuration		
		dbms.ssl.policy.https.enabled=true		
		dbms.ssl.policy.https.base_directory=/var/local/fulcrumsaas/certs/letsencrypt/STAR.fulcrumsaas.net/		
		dbms.ssl.policy.https.private_key=privkey.pem		
		dbms.ssl.policy.https.public_certificate=fullchain.pem		
		dbms.ssl.policy.https.client_auth=NONE	

6. Restart Neo4j service:
    
    Check status of service 

        service neo4j status

    Restart service
    
        service neo4j restart

    Check status of service

        service neo4j status
