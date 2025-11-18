### TASK 1: CREATE GROUPS ====="
~~~
sudo groupadd developers
sudo groupadd testers
sudo groupadd admins
~~~

 ### 2: CREATE USERS AND ASSIGN GROUPS ====="
~~~
sudo useradd -m -G developers alice
sudo useradd -m -G testers bob
sudo useradd -m -G admins charlie
~~~
### 3: SET PASSWORDS 
~~~
 "sudo passwd alice"
 "sudo passwd bob"
 "sudo passwd charlie"
~~~
### 4 : CREATE PROJECT DIRECTORY
~~~
sudo mkdir /projectX
sudo chown root:developers /projectX
sudo chmod 770 /projectX
~~~
### 5: VERIFY GROUP MEMBERSHIP 
~~~
id alice
id bob
id charlie
~~~
### 6: PASSWORD POLICIES
~~~
echo "Step 1: Set password aging policy for alice"
sudo chage -M 60 -m 5 -W 10 alice

echo "Step 2: Force bob password expire"
sudo passwd --expire bob

echo "Step 3: Set account expiration date for charlie"
sudo chage -E 2025-12-31 charlie

echo "Step 4: Lock and Unlock alice"
sudo passwd -l alice  
sudo passwd -u alice  
~~~
### 7: Disable login after inactivity for bob"
~~~
sudo chage -I 30 bob

echo "Verify password policies:"
sudo chage -l alice
sudo chage -l bob
sudo chage -l charlie
~~~
### 8: CONFIGURE SUDO PRIVILEGES
~~~
"NOTE: The next steps require editing visudo manually."
 "Add the below lines inside visudo:"

 "### ALLOW ALICE TO RESTART SSH ONLY"
 "alice ALL=(ALL) /bin/systemctl restart ssh, /bin/systemctl status ssh"

 "### ALLOW BOB TO RUN ONLY NETWORK COMMANDS"
 "bob ALL=(ALL) /usr/sbin/ifconfig, /usr/sbin/ip"

 "### ALLOW CHARLIE PASSWORDLESS df"
"charlie ALL=(ALL) NOPASSWD: /bin/df"

"To edit sudoers safely:"
"sudo visudo"
~~~
### 9. TEST COMMANDS
~~~
sudo df
sudo systemctl restart ssh
sudo ip addr
~~~
