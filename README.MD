HW #7 - Security
----------------------------------------------------------------------
Design and build a PKI infrastructure that includes Root CA, Signing CA, and TLS Certificate for an organisation named Simple Inc,
which has control over the domain simple.org.


Steps:
----------
1. Clone git and create directories. 

   ```
   git clone https://bitbucket.org/stefanholek/pki-example-1
   cd pki-example-1
   ```
2. Create Root CA:

   2.1 Create directories¶: 
   
   ```
   mkdir -p ca/root-ca/private ca/root-ca/db crl certs
   chmod 700 ca/root-ca/private
   ```
   
   2.2 Create database: 

   ```
   openssl req -new \
    -config etc/root-ca.conf \
    -out ca/root-ca.csr \
    -keyout ca/root-ca/private/root-ca.key
    ```
    ![image](https://user-images.githubusercontent.com/89236239/142757169-1a0eb8e8-9dd0-4227-bf29-e9fee6a03b89.png)
    
    2.3 Create root CA request: 
     
    ```
    openssl req -new \
    -config etc/root-ca.conf \
    -out ca/root-ca.csr \
    -keyout ca/root-ca/private/root-ca.key
    ```
    
    2.4 Create the root CA certificate: 
    
    ```
    openssl ca -selfsign \
    -config etc/root-ca.conf \
    -in ca/root-ca.csr \
    -out ca/root-ca.crt \
    -extensions root_ca_ext
    ```
    
 3. Create Signing CA:
 
     3.1 Create Directories
     ```
     mkdir -p ca/signing-ca/private ca/signing-ca/db crl certs
     chmod 700 ca/signing-ca/private
     ```
     3.2 Create Database
     ```
     cp /dev/null ca/signing-ca/db/signing-ca.db
     cp /dev/null ca/signing-ca/db/signing-ca.db.attr
     echo 01 > ca/signing-ca/db/signing-ca.crt.srl
     echo 01 > ca/signing-ca/db/signing-ca.crl.srl
     ```
     3.3 Create the signing CA request:
     ```
     openssl req -new \
    -config etc/signing-ca.conf \
    -out ca/signing-ca.csr \
    -keyout ca/signing-ca/private/signing-ca.key
    ```
    
    3.4 Create the signing CA certificate:
    
    ```
    openssl ca \
    -config etc/root-ca.conf \
    -in ca/signing-ca.csr \
    -out ca/signing-ca.crt \
    -extensions signing_ca_ext
    ```
    
  4. Create TLS server certificate:
     
     This will create a private key and a certificate signing request for the TLS server certificate.
    
     
     ```
     SAN=DNS:www.simple.org \
     openssl req -new \
     -config etc/server.conf \
     -out certs/simple.org.csr \
     -keyout certs/simple.org.key
     ```
    
    
      Using the signing CA and the CSR created from the previous step, a server certificate can be issued with the following command.
    
    
    
      ```
      openssl ca \
      -config etc/signing-ca.conf \
      -in certs/simple.org.csr \
      -out certs/simple.org.crt \
      -extensions server_ext
      ```
    
    
 TLS certificate can be viewed in certs folder
 ----------------------------------------------------------------------------
 
 ![image](https://user-images.githubusercontent.com/89236239/142757490-accf323c-659f-4025-a3b9-2aa8ba41bc60.png)
![image](https://user-images.githubusercontent.com/89236239/142757500-e83b121d-147f-43d8-a1e3-dbfbe51a9851.png)


    
    
   





