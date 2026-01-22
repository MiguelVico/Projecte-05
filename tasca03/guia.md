# Guia de la Tasca 03: Serveis de transferència de fitxers

Aquesta guia explica com configurar un servidor FTP utilitzant **vsftpd** a Ubuntu, des de la instal·lació fins a la configuració d’usuaris i directoris. Utilitzem imatges de les captures realitzades durant la pràctica per il·lustrar cada pas.

---

## 1. Instal·lació i configuració inicial del servidor FTP

### 1.1 Actualització del sistema
Abans de començar, és bona pràctica actualitzar el sistema per assegurar-se que tenim les darreres versions dels paquets.

![Actualització del sistema](/tasca03/imgT03/captura2.png)

**Comandes utilitzades:**
```bash
sudo apt update
sudo apt upgrade -y
```
**Anàlisi:**
Amb aquestes comandes actualitzem la llista de paquets disponibles i després actualitzem els paquets instal·lats. Es veu que el sistema té disponibles 55 actualitzacions pendents.

---

### 1.2 Instal·lació del servidor vsftpd
Instal·lem el paquet `vsftpd`, que és el servidor FTP que utilitzarem.

![Instal·lació de vsftpd](/tasca03/imgT03/captura1.png)

**Comanda utilitzada:**
```bash
sudo apt install vsftpd
```
**Anàlisi:**
Aquí s’instal·len dos paquets: `vsftpd` (el servidor) i `ssl-cert` (per a certificats SSL si es vol fer FTPS). Es descarreguen 137 kB i s’ocupen 380 kB addicionals de disc.

---

### 1.3 Còpia de seguretat del fitxer de configuració
Abans de modificar res, fem una còpia de seguretat del fitxer de configuració original per si alguna cosa falla.

![Còpia de seguretat](/tasca03/imgT03/captura3.png)

**Comandes utilitzades:**
```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.original
sudo cp /etc/vsftpd.conf /etc/vsfTpd.bak
```
**Anàlisi:**
Es creen dues còpies del fitxer de configuració. Una amb el nom `.original` i una altra amb un nom alternatiu `.bak`. Així, sempre podem tornar a l’estat inicial si toquem alguna cosa que no hauríem.

---

### 1.4 Verificació de l’estat del servidor
Comprovem que el servei està actiu i funcionant després de la instal·lació.

![Estat del servei](/tasca03/imgT03/captura3.png)

**Comanda utilitzada:**
```bash
sudo systemctl status vsftpd
```
**Anàlisi:**
El servei està `active (running)`, això vol dir que el servidor FTP ja està en marxa. També podem veure el PID (process ID) i que el servei s’inicia automàticament en arrencar el sistema.

---

## 2. Configuració del servidor FTP

### 2.1 Configuració del fitxer `vsftpd.conf`
Aquí és on configurem el comportament del servidor FTP. Per exemple, podem permetre o denegar l’accés anònim, activar l’escriptura, etc.

![Configuració del fitxer](/tasca03/imgT03/captura4.png)

**Configuracions principals vistes:**
- `listen_ipv6=YES`: escolta en IPv6.
- `anonymous_enable=NO`: **desactiva l’accés anònim** per seguretat (aquest canvi es veu més endavant a la captura 14).
- `local_enable=YES`: permet l’accés amb usuaris locals.
- `write_enable=YES`: permet pujar i modificar fitxers.

---

### 2.2 Reinici del servei
Després de modificar la configuració, hem de reiniciar el servei perquè els canvis tinguin efecte.

![Reinici del servei](/tasca03/imgT03/captura5.png)

**Comanda utilitzada:**
```bash
sudo systemctl restart vsftpd
```
**Anàlisi:**
Amb aquesta comanda reiniciem el servei vsftpd perquè carregui la nova configuració que hem modificat.

---

## 3. Configuració del directori FTP

### 3.1 Creació del directori FTP
Creem el directori on es serviran els fitxers via FTP.

![Creació del directori FTP](/tasca03/imgT03/captura6.png)

**Comandes utilitzades:**
```bash
sudo mkdir -p /srv/ftp
sudo chown nobody:nogroup /srv/ftp
sudo chmod 755 /srv/ftp
```
**Anàlisi:**
- `mkdir -p`: crea el directori, i si no existeixen els pares, els crea també.
- `chown`: canvia el propietari del directori a `nobody:nogroup`, per evitar que cap usuari local en sigui propietari.
- `chmod 755`: dóna permisos de lectura i execució a tothom, però només el propietari pot escriure.

---

### 3.2 Estructura de directoris
Dins del directori FTP, creem una estructura organitzada (subdirectoris per a diferents tipus de fitxers).

![Estructura de directoris](/tasca03/imgT03/captura7.png)

**Comandes utilitzades:**
```bash
sudo mkdir -p /srv/ftp/pub/files
sudo mkdir -p /srv/ftp/pub/music
sudo mkdir -p /srv/ftp/pub/pics
```
**Anàlisi:**
Es creen tres subdirectoris dins de `/srv/ftp/pub/` per organitzar els fitxers: `files`, `music`, `pics`.

---

### 3.3 Creació de fitxers d’exemple
Afegim alguns fitxers de prova als directoris creats.

![Creació de fitxers](/tasca03/imgT03/captura8.png)

**Comandes utilitzades:**
```bash
sudo echo "Informació pública" | sudo tee /srv/ftp/info.txt
sudo echo "FITXER1" | sudo tee /srv/ftp/pub/files/file1.txt
```
**Anàlisi:**
El `tee` s’utilitza per escriure tant a un fitxer com a la pantalla. Creem un fitxer `info.txt` a la carpeta principal i un `file1.txt` dins de `pub/files`.

---

### 3.4 Creació de fitxers buits
També creem alguns fitxers buits per simular contingut multimèdia.

![Fitxers buits](/tasca03/imgT03/captura9.png)

**Comandes utilitzades:**
```bash
sudo touch /srv/ftp/pub/music/song.mp3
sudo touch /srv/ftp/pub/pics/sun.jpg
```
**Anàlisi:**
El `touch` crea fitxers buits si no existeixen. Aquí simulem una cançó MP3 i una imatge JPG.

---

### 3.5 Comprovació de l’estructura final
Llistem el contingut del directori FTP per veure com ha quedat tot.

![Estructura final](/tasca03/imgT03/captura10.png)

**Comanda utilitzada:**
```bash
sudo ls -la /srv/ftp/
```
**Anàlisi:**
Es veu el fitxer `info.txt` i el directori `pub`, amb els permisos i propietaris corresponents.

---

## 4. Creació d’usuaris per a l’accés FTP

### 4.1 Creació de l’usuari `prova1`
Creem el primer usuari per accedir al FTP de manera autenticada.

![Creació de prova1](/tasca03/imgT03/captura11.png)

**Comanda utilitzada:**
```bash
sudo adduser prova1
```
**Anàlisi:**
L’usuari `prova1` es crea amb UID/GID 1001, se li assigna la seva pròpia carpeta personal (`/home/prova1`) i se l’afegeix al grup `users`.

---

### 4.2 Creació de l’usuari `prova2`
Creem un segon usuari per tenir més d’un usuari de prova.

![Creació de prova2](/tasca03/imgT03/captura12.png)

**Comanda utilitzada:**
```bash
sudo adduser prova2
```
**Anàlisi:**
`prova2` té UID/GID 1002, i també s’afegeix al grup `users`. Així tenim dos usuaris per fer proves d’accés.

---

### 4.3 Verificació dels usuaris creats
Comprovem que els usuaris s’han creat correctament amb els grups corresponents.

![Verificació d’usuaris](/tasca03/imgT03/captura13.png)

**Comandes utilitzades:**
```bash
id prova1
id prova2
```
**Anàlisi:**
Amb `id` veiem l’UID, GID i grups als que pertany cada usuari. Tots dos pertanyen al grup `users` a més del seu grup per defecte.

---

## 5. Últimes configuracions i reinici

### 5.1 Configuració final de vsftpd.conf
Abans de reiniciar, ens assegurem que la configuració estigui correcta, especialment que l’accés anònim estigui desactivat.

![Configuració final](/tasca03/imgT03/captura14.png)

**Anàlisi:**
Es confirma que `anonymous_enable=NO`, `local_enable=YES` i `write_enable=YES`. Això vol dir que només usuaris locals podran connectar-se i podran pujar fitxers.

---

### 5.2 Reinici definitiu
Reiniciem el servei per aplicar totes les configuracions.

![Reinici definitiu](/tasca03/imgT03/captura15.png)

**Comanda utilitzada:**
```bash
sudo systemctl restart vsftpd
```
**Anàlisi:**
Amb aquest reinici, el servidor ja està configurat i llest per a connexions autenticades d’usuaris locals, sense accés anònim.

---

## 📌 Resum de la configuració
- **Servidor instal·lat**: vsftpd
- **Accés anònim**: desactivat
- **Usuaris locals**: `prova1` i `prova2`
- **Directori FTP**: `/srv/ftp`
- **Permisos d’escriptura**: activats
- **Servei en marxa**: sí

Aquesta configuració ens permet ara connectar-nos al servidor FTP des d’un client (com FileZilla o des de la terminal) utilitzant les credencials d’un usuari local i transferir fitxers de manera segura (sense accés públic).


# Guia de la Tasca 03: Serveis de transferència de fitxers (Part 2)

Continuem amb la segona part de la configuració del servidor FTP, on veurem com connectar-nos, configurar seguretat amb chroot, analitzar el trànsit amb Wireshark i utilitzar un client gràfic com FileZilla.

---

## 6. Prova de connexió FTP local

### 6.1 Connexió amb l’usuari `prova1`
Ens connectem al servidor FTP des de la mateixa màquina utilitzant `localhost`.

![Connexió FTP local](/tasca03/imgT03/captura16.png)

**Comanda utilitzada:**
```bash
ftp localhost
```
**Anàlisi:**
- Ens demana usuari i contrasenya. Posem `prova1`.
- Ens diu `230 Login successful` → login correcte.
- El mode de transferència és **binari** (més eficient per a qualsevol tipus de fitxer).
- El directori remot inicial és `/home/prova1` (la seva carpeta personal).

---

### 6.2 Pujada d’un fitxer (`put`)
Pujem un fitxer des del client al servidor.

![Pujada de fitxer](/tasca03/imgT03/captura16.png)

**Comanda utilitzada dins de FTP:**
```bash
put prova1.txt
```
**Anàlisi:**
- El fitxer `prova1.txt` es transfereix correctament.
- S’utilitza el **mode passiu estès** (EPSV), que és més segur perquè el client inicia la connexió de dades.
- Es veu el percentatge de transferència i la velocitat (1.16 MiB/s en aquest cas).

---

### 6.3 Accés a directoris restringits i descàrrega
Provem d’accedir a `/etc` i descarregar el fitxer `passwd`.

![Accés a /etc i descàrrega](/tasca03/imgT03/captura17.png)

**Comandes utilitzades dins de FTP:**
```bash
cd /etc
get passwd
```
**Anàlisi:**
- Es pot canviar al directori `/etc` → això pot ser un problema de seguretat.
- Es descarrega el fitxer `passwd`, que conté informació sensible dels usuaris del sistema.
- Això demostra que, per defecte, un usuari FTP pot navegar per quasi qualsevol lloc del sistema (perillós!).

---

## 7. Configuració de seguretat: chroot

### 7.1 Activació del chroot
Per evitar que els usuaris puguin sortir del seu directori principal, activem l’opció `chroot_local_user`.

![Configuració chroot](/tasca03/imgT03/captura18.png)

**Configuració afegida a `/etc/vsftpd.conf`:**
```bash
chroot_local_user=YES
allow_writeable_chroot=YES
```
**Anàlisi:**
- `chroot_local_user=YES`: confina cada usuari al seu directori `home`. No podrà pujar més amunt.
- `allow_writeable_chroot=YES`: permet que l’usuari pugui escriure dins d’aquest directori tancat.

---

### 7.2 Reinici del servei
Apliquem els canvis de configuració.

![Reinici amb chroot](/tasca03/imgT03/captura19.png)

**Comanda utilitzada:**
```bash
sudo systemctl restart vsftpd
```
**Anàlisi:**
Reiniciem el servei perquè la configuració de chroot tingui efecte.

---

### 7.3 Prova del chroot
Tornem a connectar-nos i provem d’accedir a `/etc`.

![Prova chroot](/tasca03/imgT03/captura20.png)

**Comandes dins de FTP:**
```bash
ls
cd /etc
```
**Anàlisi:**
- Ara l’usuari `prova1` només veu el seu fitxer `prova1.txt` dins del seu `home`.
- Quan intenta anar a `/etc`, reb `550 Failed to change directory` → **funciona el chroot!** L’usuari ja no pot sortir del seu directori.

---

## 8. Estructura del directori FTP públic

### 8.1 Visualització de l’estructura
Utilitzem `tree` per veure tota l’estructura de directoris i fitxers que hem creat per a l’accés anònim/públic.

![Estructura amb tree](/tasca03/imgT03/captura21.png)

**Comanda utilitzada:**
```bash
sudo tree /srv/
```
**Anàlisi:**
- Es veu l’estructura completa del directori FTP públic.
- Hi ha 6 directoris i 4 fitxers de prova.
- Tot està ben organitzat dins de `/srv/ftp/`.

---

## 9. Instal·lació d’eines de xarxa

### 9.1 Instal·lació de Wireshark
Instal·lem Wireshark per capturar i analitzar el trànsit de xarxa (útil per veure com viatgen les dades FTP).

![Instal·lació de Wireshark](/tasca03/imgT03/captura22.png)

**Comanda utilitzada:**
```bash
sudo apt install wireshark
```
**Anàlisi:**
- S’instal·len moltes dependències (41 paquets nous).
- Ocupa uns 214 MB de disc.
- Wireshark ens permetrà veure els paquets FTP **en clar**, ja que FTP no xifra les dades.

---

### 9.2 Instal·lació del client FTP
Assegurem-nos que tenim el client FTP instal·lat per fer connexions des de la terminal.

![Instal·lació del client FTP](/tasca03/imgT03/captura23.png)

**Comanda utilitzada:**
```bash
sudo apt install ftp -y
```
**Anàlisi:**
- El paquet `ftp` ja estava instal·lat (versió 20230507-2build3).
- Aquest client ens permet connectar-nos a qualsevol servidor FTP des de la línia de comandes.

---

## 10. Connexió FTP des d’una altra IP

### 10.1 Connexió des d’un client extern
Ara ens connectem al servidor FTP utilitzant la seva IP (`192.168.56.101`) des d’un altre equip de la xarxa.

![Connexió des d’una altra IP](/tasca03/imgT03/captura24.png)

**Comanda utilitzada:**
```bash
ftp 192.168.56.101
```
**Anàlisi:**
- Ens connectem correctament amb l’usuari `prova1`.
- El servidor respon amb `220 (vsFTPd 3.0.5)`.
- Això demostra que el servidor FTP és accessible des d’altres màquines de la xarxa local.

---

## 11. Anàlisi del trànsit amb Wireshark

### 11.1 Captura de paquets FTP
Capturem el trànsit de xarxa durant una connexió FTP per veure com es transmeten les dades.

![Captura Wireshark](/tasca03/imgT03/captura25.png)

**Anàlisi:**
- Es veuen paquets TCP i FTP entre `192.168.56.101` (servidor) i `192.168.56.102` (client).
- El port 21 és el port de control FTP.
- Es pot veure com les comandes (`USER prova1`) i les contrasenyes viatgen **en text clar** (no xifrat) → **risc de seguretat**.
- Això justifica per què s’utilitza **SFTP** (sobre SSH) per a transferències segures.

---

## 12. Utilització d’un client gràfic: FileZilla

### 12.1 Descàrrega i instal·lació de FileZilla
Descarreguem i instal·lem FileZilla Client, un client gràfic FTP molt popular.

![Descàrrega FileZilla](/tasca03/imgT03/captura26.png)
![Instal·lació FileZilla 1](/tasca03/imgT03/captura27.png)
![Instal·lació FileZilla 2](/tasca03/imgT03/captura28.png)

**Anàlisi:**
- FileZilla és gratuït i multiplataforma.
- Durant la instal·lació ens ofereixen instal·lar Google Chrome (oferta opcional).
- Triem instal·lar-lo només per a l’usuari actual.

---

### 12.2 Connexió amb el servidor FTP
Configurem FileZilla per connectar-nos al nostre servidor.

![Configuració connexió FileZilla](/tasca03/imgT03/captura29.png)

**Configuració:**
- **Servidor:** `192.168.56.101`
- **Port:** 21 (per defecte)
- **Usuari:** `prova1`
- **Contrasenya:** la que vam posar a l’usuari

---

### 12.3 Navegació i transferència
Un cop connectats, naveguem pels directoris del servidor.

![Navegació FileZilla](/tasca03/imgT03/captura30.png)

**Anàlisi:**
- A l’esquerra veiem els fitxers locals (Windows).
- A la dreta veiem els fitxers remots (servidor FTP).
- Podem arrossegar fitxers per pujar o baixar.
- FileZilla també suporta **SFTP** i **FTPS** per a connexions xifrades.

---

## 📌 Resum final

### Què hem fet?
1. **Instal·lat i configurat vsftpd** com a servidor FTP.
2. **Creat usuaris locals** (`prova1`, `prova2`) per a accés autenticat.
3. **Configurat chroot** per confinar els usuaris als seus homes (seguretat).
4. **Creat una estructura de directoris pública** a `/srv/ftp/`.
5. **Provat connexions** des de la terminal i des d’un client gràfic (FileZilla).
6. **Analitzat el trànsit** amb Wireshark, veient que FTP **no xifra** les dades.
7. **Connectat des d’una altra màquina** de la xarxa local.

### Aprendre destacat
- **FTP bàsic és ràpid però insegur** → les dades viatgen en clar.
- **Chroot és essencial** per evitar que els usuaris accedeixin a tot el sistema.
- **Els clients gràfics** com FileZilla faciliten molt la transferència de fitxers.
- **Per seguretat real, caldria utilitzar SFTP o FTPS** (amb SSL/TLS).

Ara ja tenim un servidor FTP funcionant i hem après a configurar-lo de manera bàsica i segura. El proper pas seria implementar **SFTP** per tenir transferències xifrades.
