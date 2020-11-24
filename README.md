VPN access to a Private web server
***************************

We have a webserver in a private subnet on AWS

- It runs an nginx container with a mountable root web page

- It is accessible only from a public subnet in the same VPC

- We have a job on a jenkins server in the public subnet in the same VPC which 

  is triggered by a webhooks from this github repo.

- The job (pipeline) updates the root webpage in the webserver with an updated index.html file

- A rule to redirect traffic from port 80 to 8080 was created on the jenkins server using iptables:

  sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080

  
Access
------

- An AWS Client VPN was configured in the VPC 

   -  The Client VPN endpoint was associated with the public subnet.

   -  A server certificate was created and 2 client certificates (using easyrsa).

   -  All certificates were added to ASM and were used in the creation of the
 
      Client VPN endpoint.

   - The Client Configuration for the VPN Endpoint was downloaded and used to
  
     create 2 configurations:

        1) Using the first client certificate and key

        2) Using the second client certificate and key

     The configurations were:

        1) client-config1

        2) client-config2

     This allows providing access to 2 different users and revoking 1 if needed by creating
  
     and importing a Client Certificate CRL (certifcate revoking list) to the Client VPN

     Endpoint.

      - Use: easyrsa gen-crl to generate the CRL

-  Client Access  (Windows)

     - install the OpenVPN client

     - Open the GUI and choose Import Profile in the menu

     - Choose File

     - Browser opens to find the file

     - Choose the client config file and Press Open

     - You will see that the profile was imported

     - You can connect immediately or later

     - After connecting, access to the web server should be possible using its private ip

       from the browser as follows:

              http://172.31.45.36

AWS Configuration used:
----------------------

VPC
---

1) One VPC (not a default one)

2) One public subnet: pub-subnet1

3) One private subnet:  priv-subnet1

4) One routing table for the public subnet including an internet gateway

5) One routing table for the private subnet allowing only traffic from within the VPC

6) One Security group for the jenkins server in the public subnet allowing traffic from port 80 for the 

   github webhook - in production should use https and restrict input traffic.   

7) One Security group for the private web server that is found in the private subnet

   - Which opens port 22 for scp from the jenkins server and port 80 for http to the webserver

   - Both are limited to source from within the VPC.

8) One ElasticIP that was associcated with the jenkins EC2 instance

   - 54.228.162.105

EC2
---

   - 1 ubuntu server with a jenkins server on it

   - 1 ubnutu server with docker sw hosting the nginx container comprising the webserver
