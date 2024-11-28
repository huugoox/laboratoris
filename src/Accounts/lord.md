# Lord of the system

![](./figs/lord-of-the-ring-trilogy.png)

Tres anillos para los Reyes Elfos bajo el cielo.

Siete para los Señores Enanos en casas de piedra.

Nueve para los Hombres Mortales condenados a morir.

Uno para el Señor Oscuro, sobre el trono oscuro.

Un Anillo para gobernarlos a todos. Un anillo para encontrarlos,

un Anillo para atraerlos a todos y atarlos en las tinieblas

en la Tierra de Mordor donde se extienden las Sombras

## Objectius

* Aprendre a gestionar comptes en servidors UNIX/Linux.
* Familiaritzar-se amb els mecanismes de protecció i control d'usuaris.

## Preparant el servidor

1. Actualitzar totes les llibreries amb ```sudo dnf update -y```.
2. Instal·lar un editor de text (vim o nano) amb ```sudo dnf install vim -y```.
3. Instal·lar la shell tcsh amb ```sudo dnf install tcsh -y```.
4. Actualitzar el nom de l'amfitrió amb: ```sudo hostnamectl set-hostname middlearth```.

## Mostrant informació de benvinguda

El fitxer **/etc/motd** és l'arxiu on es guarda un missatge de benvinguda; normalment és un arxiu de text senzill que es mostra als usuaris quan inicien sessió. Pot contenir informació com la benvinguda al sistema, informació d'actualitat, polítiques de l'empresa, enllaços a recursos importants o qualsevol altra cosa que es consideri útil per als usuaris en el moment d'iniciar sessió.

Copieu el text següent al fitxer **/etc/motd**:

```txt
#                Bienvenido a la tierra media!                  #
#                            *  *  *                            #
#                         *  *  *  *  *                         #
#                      *  *  *  *  *  *  *                      #
#                      *  *  *  *  *  *  *                      #
#                      *  *  *  *  *  *  *                      #
#                         *  *  *  *  *                         #
#                            *  *  *                            #
# El hogar está atrás, el mundo por delante, y ha               #
# y muchos caminos que recorrer a través de la                  #
# sombras hasta el borde de la noche, hasta que                 #
# las estrellas estén encendidas                                #
#################################################################
```

```sh
 sudo vim /etc/motd 
# Afegiu la informació a l'arxiu.
 exit
```

Tornem a iniciar sessió **SSH**. Per veure el nostre banner, després de fer *login*.

## Mostrant informació: Connexions remotes

L'arxiu **/etc/issue.net** és similar al missatge del dia (**/etc/motd**), però aquest s'utilitza per mostrar un missatge als usuaris abans que aquests s'autentiquin en un servidor mitjançant protocols com *SSH*. Aquest missatge normalment conté informació bàsica o una benvinguda als usuaris quan intenten connectar-se al servidor.

Per activar-ho:

1. Copieu el contingut del fitxer */etc/issue.net* a */etc/issue.net.default*. D'aquesta manera sempre mantindrem una còpia del fitxer original sense editar.
2. Copieu el següent text al fitxer */etc/issue.net*:

```txt
#################################################################
#                   _    _           _   _                      #
#                  / \  | | ___ _ __| |_| |                     #
#                 / _ \ | |/ _ \ '__| __| |                     #
#                / ___ \| |  __/ |  | |_|_|                     #
#               /_/   \_\_|\___|_|   \__(_)                     #
#                                                               #
#               You are entering into Mordor!                   #
#   Username has been noted and has been sent to the server     #
#                       administrator!                          #
#################################################################
```

Finalment, tornem a iniciar sessió **SSH**. Per veure el nostre banner.

```sh
 sudo cp /etc/issue.net /etc/issue.net.default
 sudo vim /etc/issue.net
# Copieu el text
# Guardar i sortir
```

Per podeu veure el banner, s'ha d'editar la configuració del servei SSH:

```sh
sudo vim /etc/ssh/sshd_config
# Descomentar 
Banner /etc/issue.net
# Guardar i sortir
sudo systemctl restart sshd
exit
```

## Creació de grups

La comanda **groupadd** s'utilitza per crear nous grups en un sistema Linux. Els grups són una manera de gestionar col·leccions d'usuaris, permetent una administració eficaç de permisos i accés als recursos del sistema. Els grups ajuden a organitzar i controlar els usuaris amb un mateix conjunt de permisos.

```sh
groupadd [opcions] nom_del_grup
```

### Crea 4 grups amb els següents GIDs

|**nom**|**GID**|
|---|---|
|hobbits| 6000|
|elfs| 7000|
|nans| 8000|
|mags| 9000|

```sh
groupadd -g 6000 hobbits 
groupadd -g 7000 elfs 
groupadd -g 8000 nans 
groupadd -g 9000 mags
```

## Creant els usuaris

La comanda *useradd* s'utilitza per crear nous usuaris en un sistema Linux. Cada usuari té un identificador únic (**UID**) i pot ser assignat a un o més grups. Aquesta comanda també pot crear els directoris inicials (home directories) pels usuaris.

### Crea els següents comptes

|**usuari**  |**UID**|**GID**|**nom**    |
|--------|---|---|-------|
|frodo   |6001|6000|Frodo  |
|gollum  |6002|6000|Smeagol|
|samwise |6003|6000|Samwise|
|legolas |7001|7000|Legolas|
|gimli   |8001|8000|Gimli  |
|gandalf |9001|9000|Gandalf|

```sh
sudo useradd frodo -g 6000 -u 6001 -c "Frodo, portador de l'anell" -m -d /home/frodo 
sudo useradd gollum -g 6000 -u 6002 -c "Smeagol, el cercador de l'anell" -m -d /home/gollum 
sudo  useradd samwise -g 6000 -u 6003 -c "Samwise, l'amic fidel" -m -d /home/samwise 
sudo  useradd legolas -g 7000 -u 7001 -c "Legolas, el mestre de l'arc" -m -d /home/legolas 
sudo  useradd gimli -g 8000 -u 8001 -c "Gimli, el valent guerrer" -m -d /home/gimli
sudo  useradd gandalf -g 900 -u 9001 -c "Gandalf, el mag" -m -d /home/gandalf
```

La *contrasenya* de tots els comptes ha de ser *Tolkien2LOR*.

```sh
sudo passwd frodo 
sudo passwd gollum 
sudo passwd samwise 
sudo passwd legolas 
sudo passwd gandalf 
```

## Actualitzant Usuaris

* Afegiu l'usuari **root** al grup dels **mags**.

```sh
sudo usermod -aG mags root
```

o bé podeu fer servir (*gpasswd*):

```sh
sudo gpasswd -a root mags
```

* L'usuari **gollum** vol tenir el seu *home* amb el nom **smeagol**.

```sh
sudo usermod -d /home/smeagol -m gollum
```

* L'usuari **legolas** vol tenir per defecte la *shell tcsh*.

```sh
sudo usermod -s /bin/tcsh legolas
```

* L'usuari **gimli** no vol tenir *contrasenya*.

```sh
sudo passwd -d gimli
```

## Notificació de la comarca

Un cop s'han creat els usuaris entrarem al sistema com a **frodo** i enviarem un *mail* a la resta amb el següent missatge: **Benvinguts a la companyia, anem direcció mordor**.

```sh
sudo dnf -y install postfix
sudo dnf -y install s-nail
```

Postfix és un programari de servidor de correu electrònic que té com a objectiu principal gestionar l'enviament, recepció i l'encaminament de correus electrònics en un entorn de servidor. És conegut per la seva eficiència, seguretat i flexibilitat, i és àmpliament utilitzat en servidors de correu electrònic en tot el món.

### Configuració del postfix

Editeu el fitxer */etc/postfix/main.cf*:

* **myhostname** = mail.middlearth.udl.cat
* **mydomain** = udl.cat
* **myorigin** = \$mydomain
* **inet_interfaces** = all
* **inet_protocols** = ipv4
* **mydestination** = \$myhostname, localhost.$mydomain, localhost, \$mydomain
* **mynetworks** = 127.0.0.0/8
* **home_mailbox** = Maildir/

Afegiu al final del fitxer */etc/postfix/main.cf*:

```conf
# Amaga el tipus o la versió del programari SMTP
smtpd_banner = $myhostname ESMTP

# Afegeix el següent al final
# Desactiva la comanda SMTP VRFY
disable_vrfy_command = yes

# Requereix la comanda HELO als amfitrions emissors
smtpd_helo_required = yes

# Límit de mida d'un correu electrònic
# Exemple a continuació significa límit de 10M bytes
message_size_limit = 10240000

# Configuracions SMTP-Auth
smtpd_sasl_type = dovecot
smtpd_sasl_path = private/auth
smtpd_sasl_auth_enable = yes
smtpd_sasl_security_options = noanonymous
smtpd_sasl_local_domain = $myhostname
smtpd_recipient_restrictions = permit_mynetworks, permit_auth_destination, permit_sasl_authenticated, reject
```

### Configurant el dimoni

* Arrancant el dimoni
  
```sh
sudo systemctl enable --now postfix
```

### Prova

```sh
[root@middlearth ~]# su - frodo
[gandalf@middlearth ~]$ mail
[frodo@middlearth ~]$ mail gandalf@localhost
Subject: Notificació de la comarca
Benvinguts a la companyia, anem direcció mordor.
[frodo@middlearth ~]$ exit
[root@middlearth ~]# su - gandalf
[gandalf@middlearth ~]$ mail
```

**NOTA**: Podeu acabar el mail prement *control+d* enlloc de amb **.**.

## Nasguls

* Creareu un usuari **nasgul** que pugui esdevenir **root**, però que no pugui accedir al sistema.

El grup *wheel* és un grup especial en alguns sistemes com els basats en Red Hat. Aquest grup té un significat històric i està relacionat amb la seguretat i els privilegis d'administració del sistema. En aquests sistmes, el grup **wheel** té permisos especials per accedir a determinades funcionalitats o comandes que requereixen permisos d'administrador o *root*. Aquest grup sol estar associat amb la possibilitat d'utilitzar la comanda **su** (superuser) per canviar a l'usuari *root* o altres usuaris amb privilegis d'administrador.

```sh
sudo useradd nasgul -s /bin/nologin
sudo usermod -aG wheel nasgul
```

* El **Frodo** ha sofert l’atac d’un **nasgul** i ha oblidat la seva *contrasenya*. Reinicialitza-la a **Hawkings** i assegura’t de què en el proper **login**, ell **l’actualitzarà**.

```sh
sudo passwd -e Frodo
```

## Actualitzant l'equip

* Actualitza el *username* de **legolas** a **glorfindel**.

```sh
sudo usermod legolas -l glorfindel
```

* Creació de fitxers i directoris amb l'usuari gimli:

```sh
su - gimli
touch espassa_nana.txt
mkdir tresors
exit
```

* Actualitza el *UID* de **gimli** a *800*.

```sh
sudo usermod gimli -u 800
```

* L'usuari **gandalf** ha de poder invocar a l'usuari **root**.

```sh
sudo usermod -aG gandalf wheel
```

o bé:

```sh
sudo gpasswd -a gandalf wheel
```

* Bloca el compte de **glorfindel**.

```sh
sudo passwd -l glorfindel 
```

**OBSERVACIÓ:** Aquesta comanda no bloquejarà realment el compte, només canviarà la contrasenya per una contrasenya encriptada que no es pot desxifrar.

Per tant, la forma adequada de bloquejar el compte de l'usuari glorfindel i impedir que pugui iniciar sessió:

```sh
sudo passwd -L glorfindel 
```

o bé (*chage*):

```sh
sudo chage -E 0 glorfindel
```

## El poder de l'anell

* Crearem un directori **/anell**.

```sh
sudo mkdir /anell
```

* Crearem un grup portadors.

```sh
sudo groupadd portadors
```

* Assignarem a frodo com a propietari del directori **/anell**.

```sh
sudo chown frodo:portadors /anell
```

* Modificarem els permisos del directori: Els fitxers d’aquest directori únicament podran ser **executats/editats** per l’usuari **Frodo**, la resta d’usuaris no ha de tenir cap permís ni de lectura, a excepció del grup d'usuari del grup **portadors** que *han de poder llegir el directori*.

```sh
sudo chmod a-rwx /anell
sudo chmod g+r /anell
sudo chmod u+rwx /anell
```

* En **Frodo** ha de poder executar tots els fitxers del director **/anell/bin** sense haver d'afegir tota la ruta, únicament indicant el nom de l'executable.

```sh
sudo echo "export PATH=$PATH:/anell/bin" >> $HOME/.bashrc 
sudo source $HOME/.bashrc 
```

## Final del viatge

* Gimli es confon amb tots els missatges que apareixen a la pantalla quan inicia sessió. Configureu el seu compte perquè no es mostri cap missatge a la pantalla quan comenci la sessió.

```sh
su gimli
touch ~/.hushlogin
```

* No s’ha de permetre que en Gimli executi programes des del seu propi directori **/home**. Per fer-ho heu d'utilitzar (**setfacl**):

```sh
sudo setfacl -m u:gimli:--- /home/gimli
```

* L’usuari Samwise s’ha perdut i ha acabat a Narnia, elimineu-lo de l’univers.

```sh
sudo userdel -r samwise
```

## Exercicis

1. Investiga el mòdul PAM (Pluggable Authentication Modules) i comprova com podem modificar les polítiques de contrasenyes.
2. Investiga el mòdul **pam_tally2** i comprova com podem bloquejar comptes d'usuari després d'un nombre determinat d'intents fallits.
3. Investiga com podem configurar ssh perquè un usuari determinat no pugui veure tot el sistema operatiu, sinó únicament el seu directori home i únicament ha de poder executar programes python i no en bash.
4. Desenvolupa un script per donar d'alta usuaris en un sistema. Aquest script ha de crear per cada usuari una partició LVM amb el seu nom d'usuari i muntar-la en el seu directori home assumeix que existeix el vg_users.