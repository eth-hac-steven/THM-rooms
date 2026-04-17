

This is a machine that allows you to practise web app hacking and privilege escalation.

Task 1 Web Application Testing and privilege Escalation
    In these set of tasks you'll learn the following:

- brute forcing 
- hash cracking 
- service enumeration
- Linux Enumeration

The main goal here is to learn as much as possible. Make sure you are connected to our network using your [OpenVPN configuration file](https://tryhackme.com/access)/ use the attack box.

Answer the questions below

Question 1 Find the Service exposed by the machine 
  To find the services exposed use nmap to scan for open  port
```
   `nmap -sT -sV <target ip>`
```
    -sT  - For full tcp scan
    -sV - For service Version scan
    -Target ip -  Target ip
![[Pasted image 20260415184709.png]]

Question 3 ; What is the name of the hidden directory on the web server(enter name without /)?

Ans
```
   dirb http://target-ip/
```
  dirb - dirbuster (kali cli tool)
reveals **development**  to be the hidden directory
![[Pasted image 20260415185537.png]]

Question 4 ; Use brute-forcing to find the username & password
using the command 
```
 enum4linux -a <target-ip>
```

question 5 
What is the username?  
Ans :  Jan
![[Pasted image 20260417090124.png]]

question 6 
What is the password?
```
hydra -l jan -P /usr/share/wordlists/rockyou.txt ssh:<Target-ip>
```
Hydra is a password cracking tool
-l - used to  specific the username i.e jan
-P - used to specify the path to the wordlists to use 
ssh - the target protocol 
Target ip  - Target ip

ANS
 passwd = armando 
![[Pasted image 20260417140643.png]]

question 7
What service do you use to access the server(answer in abbreviation in all caps)?
 ANS : SSH
 
 Question 9
 What is the name of the other user you found(all lower case)?
 
 ANS : kay
 
 Question 10
 If you have found another user, what can you do with this information?
 
 No Ans needed

Question 11
What is the final password you obtain? 
  use the obtained credentials to SSH into the system.
  ![[Pasted image 20260417141324.png]]
   
now we are logged in as Jan on your target machine. 
We continue the search 
```
cd home 
ls 
```
 ![[Pasted image 20260417161725.png]]
Now let’s move into kay’s directory and list all the contents.

```
cd kay  
ls -lsa
```

The hidden directory ‘.ssh’ is what we need, let’s cd into that.

```
cd .ssh  
ls -lsa
```
![[Pasted image 20260417162136.png]]
Here, in /home/kay/.ssh, we can see three files:

- **authorized_keys:** a list of public keys allowed to authenticate via SSH
- **id_rsa:** kay’s private SSH key
- **id_rsa.pub:** kay’s public SSH key

We are interested in id_rsa, which is Kay’s private key used for SSH authentication. This file is world-readable (-rw-r — r — ), meaning any local user on the system (including us) can read it. This is a serious misconfiguration, as private keys should never be readable by other users.

We need to copy this private key to our local machine so it can be analyzed and cracked. First, we display the contents of the key:

```
cat id_rsa
```

This outputs a long, encrypted private key:

![[Pasted image 20260417162659.png]]

When copying the private key, it is critical to include the entire key, including the — — -BEGIN RSA PRIVATE KEY — — — and — — -END RSA PRIVATE KEY — — — lines, otherwise the key will not be usable.

Next, in a terminal on your local machine open nano and paste the entire key into a file. I kept the name ’id-rsa' but you can name it whatever you like.

```
nano id_rsa
```
![[Pasted image 20260417163606.png]]

After pasting kay’s key into the new id_rsa file we need to make sure we set the correct permissions. Under normal circumstances, private keys should have permissions set to 600, restricting access to the owner only.

```
chmod 600 id_rsa
```

Next, we need to convert the key into a format John the Ripper will understand. To do that we can use a helper utility that comes with John; ssh2john.

```
ssh2john id_rsa > id_rsa.hash
```

This converts the encrypted SSH private key (id_rsa) into a John-compatible hash format and saves it to a file (id_rsa.hash), allowing the key’s passphrase to be cracked using John the Ripper.

This does not crack the key itself, it only prepares the key so that John the Ripper can attempt to crack the passphrase protecting it.

```
john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa.hash
```

After John finishes cracking the hash, it will report that there are no password hashes left to crack. 
![[Pasted image 20260417164006.png]]
and this reveals that the password.
Now we can authenticate into SSH on the target machine as kay using the id_rsa.

```
ssh -i id_rsa kay@10.128.137.142
```

This command tells SSH to use the specified private key for authentication instead of password-based login. When prompted enter the passphrase you just discovered using John the Ripper.

![[Pasted image 20260417164353.png]]

Once you enter the passphrase and hit enter you will be logged in as kay.  
![[Pasted image 20260417164438.png]]
From here just list the contents of your directory. The flag can be found in the **_pass.bak_** file.

```
ls -la  
cat pass.bak
```

![[Pasted image 20260417164603.png]]
