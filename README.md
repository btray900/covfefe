# covfefe
Trump-based VM MAGA walkthrough


## netdiscover

Ths VM picks up the IP 192.168.56.108 from DHCP on the host-only adapter.

![Alt text](./netdiscover.png?raw=true)


## nmap

A nmap scan identified ports running services to be enumerated.

![Alt text](./nmap.png?raw=true)


## dirb

Using dirb on port 31337 identifes a 'taxes' directory with flag 1.

![Alt text](./dirb.png?raw=true)

![Alt text](./taxes.png?raw=true)


Checking the other findings from dirb we see a binary having been run from the .bash_history file.

![Alt text](./bash_history.png?raw=true)


We can also ID SSH keys if we enumerate the identified .ssh directory and download the public and private keys. 

![Alt text](./ssh_dir.png?raw=true)

The private key is encrypted as can be seen in the file contents.

![Alt text](./id_rsa.png?raw=true)


## ssh2jon

Using ssh2john we can brute force the SSH key passphrase.

![Alt text](./ssh2john.png?raw=true)

The passphrase is cracked. 

![Alt text](./john.png?raw=true)


## Low-privilege Shell

Using SSH and the private keyt with the compromised passphrase, we can SSH to the VM.

![Alt text](./lowshell.png?raw=true)


## Enumeration

Checking the file system shows we can read a C file in the root directory. We get flag 2. This binary was identified earlier in our .bash_history finding.

![Alt text](./source.png?raw=true)

We see a vulnerable C function in gets() in the C file

![Alt text](./gets.png?raw=true)


## Malicious payload

The C file is vulnerable to arbitrary file execution via a buffer flow. We upload a simlpe bash script to elevate if successful.

![Alt text](./venom.png?raw=true)

Upload by any available means to a writeale directory.

![Alt text](./shellbin.png?raw=true)


## root
<pre>
\(*_*)
  ( (>
  /  \
</pre>

Appending As to the message allows out command to begin at char 21 and execute. We recieve a reverse shell with root privileges

![Alt text](./root.png?raw=true)



## ctf

Cat the flag contents.

![Alt text](./ctf.png?raw=true)


## thanks

Thanks out to Tim Kent for the VM.
