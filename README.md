# Securing Prometheus and Node Exporter with TLS

This README file describes how to secure the communication between Prometheus and Node Exporter by enabling Transport Layer Security (TLS) and basic authentication.

## Generate TLS Certificate and Key

1. Create a private key for the Node Exporter:

   ```bash
   openssl genrsa -out node_exporter.key 2048
   ```

2. Generate a certificate signing request (CSR) using the private key:

   ```bash 
   openssl req -new -key node_exporter.key -out node_exporter.csr
   ```

   Fill out the requested fields when prompted.

3. Self-sign the certificate to be valid for 365 days:

   ```bash
   openssl x509 -req -days 365 -in node_exporter.csr -signkey node_exporter.key -out node_exporter.crt
   ```

## Configure Node Exporter for TLS

4. Create a config.yml file:

   ```bash
   nano config.yml
   ```

5. Add the certificate and key file paths to config.yml:

   ```yaml
   tls_server_config:
     cert_file: node_exporter.crt
     key_file: node_exporter.key
   ```

6. Edit the Node Exporter systemd service unit file: 

   ```bash
   nano /etc/systemd/system/node_exporter.service
   ```

   Add the config file path parameter.

7. Reload daemon and restart Node Exporter:

   ```bash 
   systemctl daemon-reload
   systemctl restart node_exporter
