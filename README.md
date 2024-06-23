# Maquina-Vacaciones

## Resolución de la Maquina Vacaciones de Dockerlabs.es "El pinguino de Mario"

## Paso N°1: Busqueda con nmap.

```bash
nmap -sV 172.17.0.2
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-06-23 08:31 -03
Nmap scan report for 172.17.0.2
Host is up (0.000093s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 6.18 seconds
```
_Adjunto imagen:_

![01](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/981f38e1-a867-425c-a015-96e8f0f3a69d)

#################################################################################################

## Paso N°2: Busqueda con Nikto.

```bash
nikto -h http://172.17.0.2             
- Nikto v2.5.0
---------------------------------------------------------------------------
+ Target IP:          172.17.0.2
+ Target Hostname:    172.17.0.2
+ Target Port:        80
+ Start Time:         2024-06-23 08:44:45 (GMT-3)
---------------------------------------------------------------------------
+ Server: Apache/2.4.29 (Ubuntu)
+ /: The anti-clickjacking X-Frame-Options header is not present. See: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options
+ /: The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type. See: https://www.netsparker.com/web-vulnerability-scanner/vulnerabilities/missing-content-type-header/
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Apache/2.4.29 appears to be outdated (current is at least Apache/2.4.54). Apache 2.2.34 is the EOL for the 2.x branch.
+ /: Server may leak inodes via ETags, header found with file /, inode: 4a, size: 616e75cb6bc40, mtime: gzip. See: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2003-1418
+ OPTIONS: Allowed HTTP Methods: POST, OPTIONS, HEAD, GET .
+ /icons/README: Apache default file found. See: https://www.vntweb.co.uk/apache-restricting-access-to-iconsreadme/
+ 8102 requests: 0 error(s) and 6 item(s) reported on remote host
+ End Time:           2024-06-23 08:45:16 (GMT-3) (31 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested
```
_Adjunto imagen:_ 

![02](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/6b32f19d-993f-45cb-960d-ff4fb3e137f9)

#################################################################################################

## Paso N°3: Busqueda con dirb.

```bash
dirb http://172.17.0.2 

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Sun Jun 23 08:48:23 2024
URL_BASE: http://172.17.0.2/
WORDLIST_FILES: /usr/share/wordlists/dirb/common.txt
-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://172.17.0.2/ ----
+ http://172.17.0.2/index.html (CODE:200|SIZE:74)
==> DIRECTORY: http://172.17.0.2/javascript/                                                           
+ http://172.17.0.2/server-status (CODE:403|SIZE:275)
                                         
---- Entering directory: http://172.17.0.2/javascript/ ----
==> DIRECTORY: http://172.17.0.2/javascript/jquery/                                                    
                                                                                                       
---- Entering directory: http://172.17.0.2/javascript/jquery/ ----
+ http://172.17.0.2/javascript/jquery/jquery (CODE:200|SIZE:268026)                                                                                                   
-----------------
END_TIME: Sun Jun 23 08:48:25 2024
DOWNLOADED: 13836 - FOUND: 3
```
_Adjunto imagen:_ 

![03](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/0adff950-b91c-4554-836f-d9313aefe3c6)

## _Aca podemos ver 3 directorios, dos de ellos nos envia  a paginas que no funcionan y una la "index.html" que no muestra nada pero si le damos a inspeccionar vemos un mensaje que dice: "De: Juan Para: Camilo, te he dejado un correo es importante"
_Adjunto imagen:_ 

![05](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/6ea184f0-a4cc-41b8-b071-366cac35a497)
![06](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/0c4c2b80-f609-4d4d-a030-a40b482cd97b)
![07](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/e3b537f8-0bd7-460b-a5f2-4df80340db54)



como sabemos que tiene el "puerto 22" abierto que es el de ssh vamos a probar con el nombre de camilo y si no funciona probamos con el de juan.

## Paso N°4: Ataque de Fuerza Bruta con Hydra.

_Asi que ahora procedemos a usar hydra de la siguiente manera:

```bash
hydra  -l camilo -P /usr/share/wordlists/rockyou.txt -t4 -vV 172.17.0.2 ssh
```



















