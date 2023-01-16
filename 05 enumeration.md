# port scanning
```
└─$ nmap -sC -sV -oA nmap/initial 10.10.11.177
Starting Nmap 7.93 ( https://nmap.org ) at 2023-01-03 22:21 IST
Stats: 0:01:40 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 50.00% done; ETC: 22:23 (0:00:01 remaining)
Nmap scan report for 10.10.11.177
Host is up (0.30s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 9e1f98d7c8ba61dbf149669d701702e7 (RSA)
|   256 c21cfe1152e3d7e5f759186b68453f62 (ECDSA)
|_  256 5f6e12670a66e8e2b761bec4143ad38e (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Is my Website up ?
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 122.84 seconds
```

# port 80(http)
![[Pasted image 20230103222939.png]]

# sub-domain enumeration
```
└─$ wfuzz -c -w /usr/share/wordlists/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -H "Host: FUZZ.siteisup.htb" -u 'http://siteisup.htb' --hw 93
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://siteisup.htb/
Total requests: 4989

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                         
=====================================================================

000000019:   403        9 L      28 W       281 Ch      "dev"                                                                           

Total time: 240.7250
Processed Requests: 4989
Filtered Requests: 4988
Requests/sec.: 20.72488
```


# directory enumeration (siteisup.htb)

```
└─$ gobuster dir -u http://siteisup.htb/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt -o dir.txt
===============================================================
Gobuster v3.4
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://siteisup.htb/
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.4
[+] Timeout:                 10s
===============================================================
2023/01/03 22:36:10 Starting gobuster in directory enumeration mode
===============================================================
/dev                  (Status: 301) [Size: 310] [--> http://siteisup.htb/dev/]
Progress: 40869 / 87665 (46.62%)^C
[!] Keyboard interrupt detected, terminating.

===============================================================
2023/01/03 23:06:40 Finished
===============================================================

```


# directory enumeration on /dev 
```
└─$ dirsearch -u http://siteisup.htb/dev/

  _|. _ _  _  _  _ _|_    v0.4.2
 (_||| _) (/_(_|| (_| )

Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 30 | Wordlist size: 10927

Output File: /home/scriptkiddie/.dirsearch/reports/siteisup.htb/-dev-_23-01-04_12-21-59.txt

Error Log: /home/scriptkiddie/.dirsearch/logs/errors-23-01-04_12-21-59.log

Target: http://siteisup.htb/dev/

[12:22:00] Starting: 
[12:22:17] 200 -    3KB - /dev/.git/                                       
[12:22:17] 200 -   73B  - /dev/.git/description                            
[12:22:17] 200 -   21B  - /dev/.git/HEAD
[12:22:17] 301 -  315B  - /dev/.git  ->  http://siteisup.htb/dev/.git/     
[12:22:17] 200 -  959B  - /dev/.git/info/                                  
[12:22:17] 200 -  772B  - /dev/.git/branches/                              
[12:22:17] 301 -  325B  - /dev/.git/logs/refs  ->  http://siteisup.htb/dev/.git/logs/refs/
[12:22:17] 200 -    1KB - /dev/.git/logs/
[12:22:17] 200 -  179B  - /dev/.git/logs/HEAD
[12:22:17] 200 -  298B  - /dev/.git/config
[12:22:17] 301 -  340B  - /dev/.git/logs/refs/remotes/origin  ->  http://siteisup.htb/dev/.git/logs/refs/remotes/origin/
[12:22:17] 301 -  331B  - /dev/.git/logs/refs/heads  ->  http://siteisup.htb/dev/.git/logs/refs/heads/
[12:22:17] 200 -  179B  - /dev/.git/logs/refs/remotes/origin/HEAD          
[12:22:17] 200 -    4KB - /dev/.git/hooks/                                 
[12:22:17] 301 -  333B  - /dev/.git/logs/refs/remotes  ->  http://siteisup.htb/dev/.git/logs/refs/remotes/
[12:22:17] 200 -  240B  - /dev/.git/info/exclude
[12:22:17] 200 -  521B  - /dev/.git/index
[12:22:18] 200 -  112B  - /dev/.git/packed-refs
[12:22:18] 301 -  326B  - /dev/.git/refs/heads  ->  http://siteisup.htb/dev/.git/refs/heads/
[12:22:18] 200 -    1KB - /dev/.git/objects/
[12:22:18] 200 -    1KB - /dev/.git/refs/
[12:22:18] 301 -  328B  - /dev/.git/refs/remotes  ->  http://siteisup.htb/dev/.git/refs/remotes/
[12:22:18] 301 -  335B  - /dev/.git/refs/remotes/origin  ->  http://siteisup.htb/dev/.git/refs/remotes/origin/
[12:22:18] 200 -   30B  - /dev/.git/refs/remotes/origin/HEAD               
[12:22:18] 301 -  325B  - /dev/.git/refs/tags  ->  http://siteisup.htb/dev/.git/refs/tags/
[12:22:19] 403 -  277B  - /dev/.ht_wsr.txt                                 
[12:22:19] 403 -  277B  - /dev/.htaccess.bak1                              
[12:22:19] 403 -  277B  - /dev/.htaccess.orig                              
[12:22:19] 403 -  277B  - /dev/.htaccess_extra
[12:22:19] 403 -  277B  - /dev/.htaccess.sample
[12:22:19] 403 -  277B  - /dev/.htaccess_orig
[12:22:19] 403 -  277B  - /dev/.htaccess_sc
[12:22:19] 403 -  277B  - /dev/.htaccessBAK
[12:22:19] 403 -  277B  - /dev/.htaccess.save
[12:22:19] 403 -  277B  - /dev/.htaccessOLD
[12:22:19] 403 -  277B  - /dev/.htaccessOLD2
[12:22:19] 403 -  277B  - /dev/.htm
[12:22:19] 403 -  277B  - /dev/.html
[12:22:20] 403 -  277B  - /dev/.htpasswds                                  
[12:22:20] 403 -  277B  - /dev/.htpasswd_test
[12:22:20] 403 -  277B  - /dev/.httr-oauth
[12:22:25] 403 -  277B  - /dev/.php                                        
[12:24:21] 200 -    0B  - /dev/index.php                                    
[12:24:21] 200 -    0B  - /dev/index.php/login/                             
                                               
```

# found .git directory
![[Pasted image 20230104130207.png]]