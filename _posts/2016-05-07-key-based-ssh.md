
~~~ bash
1st generate ssh signed key , either dsa or rsa(preferred/default)

ssh-keygen -t rsa
ssh-keygen    ----will default to rsa

copy the public key inside the .ssh directory

cd .ssh
you will see a id_rsa --the private key and id_rsa.pub the public key

ssh-copy-id user@ipaddress..you are copying the public key to the remote server that you need to have keybased authentication

prevent passwd everytime on session

ssh-agent bash
ssh-add
permissions on private key should be 600
permissions on public key should be 644
~~~
