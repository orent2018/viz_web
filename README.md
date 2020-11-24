Private web page update
***********************

We have a webserver in a private subnet on AWS

It is accessible only from a public subnet from the same VPC

It runs an nginx container with a mountable root web page

We have a jenkins server in the public subnet in the same VPC which 

receives webhooks from this github repo and trigers a job to update

the root webpage in the webserver with an updated index.html file
