# Macine Overview

![image](https://github.com/user-attachments/assets/1e0a23e6-0657-4f70-a344-5b22e4da3e47)

# Nmap

First i checked for only open ports no DNS resolution, or host discovery. Then based on this results i did further scanning on ports 22 and 80

Command used: nmap -Pn -n 10.10.11.64

Command used: nmap -sC -sV -p 22,80 -oN Nocturnal_nmap.txt 10.10.11.64

![image](https://github.com/user-attachments/assets/6909e55a-4324-4f5a-9904-e6962ae61c10)

Port 80 redirects to http://nocturnal.htb, added it to /etc/hosts

# Website

Looking at the site there is register and login funcionality, i tried to register and login and see there is a file upload with only allowed uploads of PDF, DOC, DOCX, XLS, XLSX, and ODT files. After viewing uploaded file i can see that there appears username field, trying to fuzz it

```bash
ffuf -u 'http://nocturnal.htb/view.php?username=FUZZ&file=*.pdf' -w /usr/share/seclists/Usernames/xato-net-10-million-usernames.txt -mc 200 -fr "User not found." -H "Cookie: PHPSESSID=4d9uqe8ifkefg1pkgu5ksi06qs"
```

![image](https://github.com/user-attachments/assets/de51d47f-9175-4936-b407-093d150b7394)

Found 3 users tobias, amanda and admin

After further enumeration found that user amanda has privacy.odt file with following content

![image](https://github.com/user-attachments/assets/3d132e00-4069-4a16-a0fb-6b0724da7ca2)

There is a password arHkG7HAI68X8s1J, trying to login as amanda with following credentials amanda:arHkG7HAI68X8s1J. When logged in as amanda i can see there is admin panel with backup interface
