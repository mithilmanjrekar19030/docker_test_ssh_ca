##SSH Testing with Host Client and Remote Docker Client & SSH Management Server.

###Inputs:- 
Enter Environment UUID & Id Token & given by CA Authentication

###Step 1:
Hit the SSH Management Server and get the environment public key and the environment name using below url.
> https://${host}/v1/environments?uuid=${uuid}

###Step 2:
This details will be used to fetch user certificate from the SSH Management Server using below url.
> https://${host}/v1/user_certs/${environment_name}/${id_token}

###Step 3:
Put the public key that is recieved from the environment into the docker container folder which will put the key in the docker container we plan to ssh for the test.
> sudo echo "{environment_public_key_contents}" > ~/.ssh/id_rsa-cert.pub

###Step 4:
Put the signed_certificate that is recieved into the id_rsa-cert.pub with the id_rsa.pub bieng the machine public key we send to get the certifivcate signed.
> sudo echo "{signed_certificate}" > ~/.ssh/id_rsa-cert.pub

###Step 5:
Build and run the docker image
'sudo docker build --rm -f "Dockerfile" -t ssh-ca-cert:latest .'
'sudo docker stop ssh-ca-cert-run-1'
'sudo docker run --rm -d -p 2201:22/tcp --name ssh-ca-cert-run-1 ssh-ca-cert:latest'
'sudo docker ps'

###Step 6:
Try to ssh 
'ssh -v root@127.0.0.1 -p 2201'
