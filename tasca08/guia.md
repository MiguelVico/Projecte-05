# Guia Tasca T08: Seguretat – Protegint-nos contra el malware

Hey, aquí teniu la guia completa de la tasca sobre seguretat i malware. He analitzat totes les captures que m'has passat (fins a la 17) i us explico pas a pas tot el que he vist, amb un llenguatge clar però professional, com si ho expliqués a un company de classe. 💻🔒

---

## Índex
1. [Introducció](#introducció)
2. [Configuració inicial de seguretat](#configuració-inicial-de-seguretat)
3. [Test amb fitxer EICAR](#test-amb-fitxer-eicar)
4. [Descarrega i execució de ransomware simulat](#descarrega-i-execució-de-ransomware-simulat)
5. [Anàlisi de l’atac i resultats](#anàlisi-de-l’atac-i-resultats)
6. [Conclusions i aprenentatges](#conclusions-i-aprenentatges)
7. [Execució repetida del ransomware simulat](#execució-repetida-del-ransomware-simulat)
8. [Treball amb carpetes protegides i comprimides](#treball-amb-carpetes-protegides-i-comprimides)
9. [Anàlisi de malware real: WannaCry](#anàlisi-de-malware-real-wannacry)
10. [Ús de portals de Threat Intelligence](#ús-de-portals-de-threat-intelligence)
11. [Procediment de neteja i restauració](#procediment-de-neteja-i-restauració)
12. [Conclusions finals](#conclusions-finals)


---

## Introducció

En aquesta tasca hem vist com funciona la protecció contra malware en Windows 11, des de la configuració bàsica fins a simular un atac de ransomware per entendre com actuen les defenses i què passa quan fallen. La idea és aprendre a identificar riscos, configurar proteccions i entendre l’impacte del malware en un entorn controlat.

---

## Configuració inicial de seguretat

Abans de res, cal veure com està configurada la seguretat del sistema. A la **captura1.png** es mostra la configuració de seguretat de Microsoft Edge, on es poden gestionar certificats, activar SmartScreen, bloquejar aplicacions no desitjades i configurar DNS segur.

![Configuració de seguretat de Microsoft Edge](/tasca08/imgT08/captura1.png)

**Anàlisi**: Aquí es veu que l’usuari té accés a totes les opcions de seguretat del navegador, incloent protecció contra phishing i descarregues malicioses. Això és la primera línia de defensa quan naveguem per Internet.

---

A la **captura5.png** i **captura6.png** es mostra el panell de seguretat de Windows Security. Tot està en verd, així que el sistema diu que “No es requereix cap acció”. Però cal anar amb compte, perquè de vegades aquestes alertes no detecten tot.

![Panell de seguretat de Windows](/tasca08/imgT08/captura5.png)
![Estat de la protecció antivirus](/tasca08/imgT08/captura6.png)

**Anàlisi**: Tot sembla correcte, però hem de verificar si la protecció en temps real està activada. Més endavant veurem que no ho estava, i això va ser clau.

---

## Test amb fitxer EICAR

Per provar si l’antivirus funciona, es fa servir un fitxer de prova anomenat **EICAR**. És un fitxer inofensiu però que tots els antivirus el detecten com a virus per provar que funcionen.

A la **captura2.png** i **captura3.png** es veu la pàgina web d’EICAR, on es poden descarregar diverses versions del fitxer de prova.

![Pàgina web d'EICAR](/tasca08/imgT08/captura2.png)
![Àrea de descàrrega d'EICAR](/tasca08/imgT08/captura3.png)

**Anàlisi**: El fitxer EICAR és un estàndard de la indústria per provar antivirus. No fa mal, però si l’antivirus el detecta, vol dir que està actiu i vigilant.

---

A la **captura4.png** es veu com Microsoft Edge amb SmartScreen activat **bloqueja la descàrrega** del fitxer `eicar_com.zip` perquè detecta que és un virus.

![Descàrrega bloquejada per SmartScreen](/tasca08/imgT08/captura4.png)

**Anàlisi**: El navegador actua com a primera barrera i evita que es descarregui un fitxer potencialment perillós, encara que en aquest cas sigui només de prova. Això demostra que les proteccions integrades funcionen.

---

Tot i així, l’usuari descarrega altres versions del fitxer EICAR com `.7z`, `.tar`, i `.zip` com es veu a la **captura11.png** i **captura12.png**.

![Contingut de la carpeta Descàrregues](/tasca08/imgT08/captura11.png)
![Fitxers EICAR descarregats](/tasca08/imgT08/captura12.png)

**Anàlisi**: L’usuari ha pogut descarregar els fitxers comprimits. Això pot ser perquè l’antivirus no escaneja fitxers comprimits amb la mateixa agressivitat, o perquè la protecció en temps real no està activada.

---

A la **captura10.png** es mostra una alerta de Windows Security quan s’intenta executar `eicar.com`. El sistema l’identifica com a “Arxiu malintencionat” i recomana no executar-lo.

![Alerta de fitxer malintencionat](/tasca08/imgT08/captura10.png)

**Anàlisi**: Windows Defender sí que detecta l’amenaça quan es vol executar el fitxer. És una protecció en segon nivell, però depèn que l’usuari no ignori l’avís.

---

## Descarrega i execució de ransomware simulat

Aquí ve el més intens: simular un atac de ransomware. Primer, l’usuari descarrega un script de PowerShell anomenat **PSRansom.ps1**.

Abans d’executar-lo, cal canviar la política d’execució de PowerShell per permetre scripts no signats. Això es veu a la **captura14.png**.

![Canvi de política d'execució a PowerShell](/tasca08/imgT08/captura14.png)

**Anàlisi**: Amb la comanda `Set-ExecutionPolicy -ExecutionPolicy Unrestricted` s’eliminen les restriccions per executar scripts. Això és **molt perillós** i només s’ha de fer en entorns de prova com aquest.

---

Després, s’executa el script `PSRansom.ps1` (captura15.png). El script simula un atac de ransomware: genera una clau AES de 256 bits, xifra fitxers de prova i guarda un log a `readme.txt`.

![Execució del ransomware simulat](/tasca08/imgT08/captura15.png)

**Anàlisi**: El script mostra informació del sistema (hostname, usuari, hora) i simula la comunicació amb un servidor de Comandament i Control (C&C). Com el servidor està caigut, genera una clau local i xifra els fitxers. Això passa perquè l’antivirus no ha detectat el script com a maliciós, possiblement perquè la protecció en temps real estava desactivada.

---

## Anàlisi de l’atac i resultats

Després de l’execució, els fitxers de prova (`prova1.txt`, `prova2.txt`, `prova3.txt`) queden xifrats amb extensió `.psr` (captura16.png).

![Fitxers xifrats després de l'atac](/tasca08/imgT08/captura16.png)

**Anàlisi**: Els fitxers originals han estat reemplaçats per versions xifrades. El ransomware també ha creat un fitxer `readme.txt` amb les instruccions (suposem) i la clau de xifrat.

---

Si obrim un fitxer xifrat (captura17.png), es veu contingut binari/aleatori, confirmant que està xifrat.

![Contingut d'un fitxer xifrat](/tasca08/imgT08/captura17.png)

**Anàlisi**: El fitxer original ja no és llegible. Sense la clau de desxifrat, és impossible recuperar-lo. Així actua un ransomware real.

---

## Conclusions i aprenentatges

✅ **El navegador i el sistema operatiu tenen proteccions integrades** que poden bloquejar amenaces abans que arribin (SmartScreen, Windows Defender).  
❌ **Si desactivem proteccions com l’execució restringida de PowerShell o la protecció en temps real**, el sistema queda exposat.  
⚠️ **Els fitxers comprimits poden passar més desapercebuts** per l’antivirus, però en descomprimir-se o executar-se poden ser detectats.  
🔐 **Un ransomware real xifra els fitxers i demanda un rescat**. En aquest cas era una simulació, però en un entorn real podria ser catastròfic.

---

### Recomanacions per a l’entorn professional:

1. **Mantenir sempre activada la protecció en temps real**.
2. **No canviar les polítiques d’execució de PowerShell** llevat que sigui absolutament necessari i en entorns controlats.
3. **Educar als usuaris** perquè no descarreguin ni executin fitxers desconeguts.
4. **Fer còpies de seguretat periòdiques** per poder recuperar-se d’un atac de ransomware.

---

Aquesta tasca m’ha ajudat a entendre **com funcionen les defenses de Windows**, **com es propaguen les amenaces** i **què passa quan fallen les proteccions**. Ara tinc més criteris per protegir equips en un entorn real. 🛡️👨‍💻

---

## Execució repetida del ransomware simulat

A la **captura18.png** es torna a executar el script `PSRansom.ps1`, similar a abans. Això indica que l'usuari està provant múltiples vegades el funcionament del ransomware simulat.

![Nova execució de PSRansom.ps1](/tasca08/imgT08/captura18.png)

**Anàlisi**: Cada execució genera una clau nova i xifra els fitxers de nou. És important entendre que un ransomware real només s'executa una vegada, però en un entorn de prova podem repetir-ho per veure el comportament.

---

A la **captura19.png** es mostra l'estat dels fitxers després de l'execució. Es veuen els fitxers `prova1`, `prova2`, `prova3` que han sigut desxifrats.

![Fitxers després del xifrat](/tasca08/imgT08/captura19.png)

**Anàlisi**: Els fitxers han estat modificats (xifrats) i ara tenen una data recent. Això és un indicador d'activitat sospitosa en un entorn real.

---

## Treball amb carpetes protegides i comprimides

A la **captura20.png** es mostra la finestra de configuració de WinRAR per crear un arxiu comprimit de la `carpeta prova`. Es veu que hi ha opcions per posar contrasenya i triar el mètode de compressió.

![Configuració de WinRAR per comprimir](/tasca08/imgT08/captura20.png)

**Anàlisi**: Comprimir carpetes amb contrasenya és una manera de protegir fitxers, però també pot ser utilitzat pel malware per amagar el seu contingut. En seguretat, és important escanejar també els arxius comprimits.

---

A la **captura21.png** es veu el contingut de la `carpeta prova` amb diversos tipus de fitxers: documents, imatges, PDFs, etc.

![Contingut de la carpeta prova](/tasca08/imgT08/captura21.png)

**Anàlisi**: Aquesta carpeta conté dades reals (imatges, documents) que podrien ser objectiu d'un ransomware. És una bona pràctica tenir còpies de seguretat d'aquest tipus de contingut.

---

## Anàlisi de malware real: WannaCry

Ara passem a un malware real: **WannaCry**. A la **captura22.png** i **captura23.png** es mostra el repositori **theZoo** de GitHub, on hi ha mostres reals de malware per a anàlisi.

![Repositori theZoo a GitHub](/tasca08/imgT08/captura22.png)
![Llista de ransomware al theZoo](/tasca08/imgT08/captura23.png)

**Anàlisi**: theZoo és un recurs públic per analitzar malware en entorns controlats. És útil per entendre com funcionen les amenaces sense posar en perill sistemes reals.

---

A la **captura24.png** es veu que s'ha descarregat el fitxer `Ransomware.WannaCry.zip` a la carpeta de Descàrregues.

![Descàrrega de WannaCry](/tasca08/imgT08/captura24.png)

**Anàlisi**: El fitxer està comprimit i protegit amb contrasenya ("infected") per evitar execucions accidentals. Això és una pràctica comuna en repositoris de malware.

---

A la **captura25.png** es demana la contrasenya per descomprimir el fitxer WannaCry.

![Demana contrasenya per descomprimir WannaCry](/tasca08/imgT08/captura25.png)

**Anàlisi**: La contrasenya actua com a seguretat addicional. Sense ella, no es pot extreure el contingut maliciós. Això és clau quan es treballa amb mostres perilloses. Posem la contrasenya ("infected") per obrir el programa.

---

A la **captura26.png** Windows Defender detecta l'executable de WannaCry com a maliciós i mostra una alerta.

![Alerta de Windows Defender per WannaCry](/tasca08/imgT08/captura26.png)

**Anàlisi**: El sistema identifica el fitxer com a perillós abans fins i tot d'executar-se. Això demostra que les signatures d'antivirus estan actualitzades per a amenaces conegudes com WannaCry.

---

## Ús de portals de Threat Intelligence

A la **captura27.png** es mostra el portal **Kaspersky Threat Intelligence** on es pujen fitxers per analitzar-los.

![Portal Kaspersky Threat Intelligence](/tasca08/imgT08/captura27.png)

**Anàlisi**: Aquestes eines permeten verificar si un fitxer és maliciós basant-se en bases de dades globals. És útil per a investigacions de seguretat.

---

A la **captura28.png** i **captura29.png** es mostra **VirusTotal**, un altre portal molt utilitzat. Es veu que alguns motors no detecten l'amenaça, però altres sí.

![Anàlisi a VirusTotal (part 1)](/tasca08/imgT08/captura28.png)
![Anàlisi a VirusTotal (part 2)](/tasca08/imgT08/captura29.png)

**Anàlisi**: VirusTotal agrega resultats de múltiples antivirus. En aquest cas, el hash SHA256 del fitxer mostra que és detectat per molts vendors. És una eina essencial per a analistes de seguretat.

---

A la **captura30.png** es mostra la configuració de xarxa d'una màquina virtual (probablement VirtualBox) on s'està executant l'anàlisi.

![Configuració de xarxa de la màquina virtual](/tasca08/imgT08/captura30.png)

**Anàlisi**: Treballar amb malware en una màquina virtual aïllada i sense xarxa evita que l'amenaça es propagui a altres sistemes. És una pràctica fonamental en anàlisi de malware.

---

## Procediment de neteja i restauració

A la **captura31.png** i **captura32.png** es veu que el fitxer WannaCry ha estat mogut a la Paperera de reciclage després de ser detectat.

![WannaCry a la paperera (1)](/tasca08/imgT08/captura31.png)
![WannaCry a la paperera (2)](/tasca08/imgT08/captura32.png)

**Anàlisi**: Un cop detectat, el malware ha de ser aïllat i eliminat. En un entorn real, també s'haurien d'esborrar les entrades de registre i altres rastres.

---

A la **captura33.png** es mostra que alguns fitxers dins de la `carpeta prova` han estat afectats (possiblement xifrats) i ara tenen extensió `.WNCRY`.

![Fitxers afectats per WannaCry](/tasca08/imgT08/captura33.png)

**Anàlisi**: WannaCry xifra fitxers i canvia la seva extensió a `.WNCRY`. Això és característic d'aquest ransomware. En un atac real, sense còpia de seguretat, les dades es perden.

---

Finalment, a la **captura34.png** es mostra l'opció de tancar la màquina virtual i **restaurar una instantània anterior** ("instantània abans de virus").

![Restauració d'instantània de la màquina virtual](/tasca08/imgT08/captura34.png)

**Anàlisi**: Aquesta és la millor pràctica: després de provar malware, es restaura la màquina virtual a un estat net. Això assegura que qualsevol canvi malició es descarta completament.

---

## Conclusions finals

✅ **Els entorns virtuals són claus** per provar malware sense riscos.  
✅ **Les eines de Threat Intelligence com VirusTotal** ajuden a identificar amenaces ràpidament.  
✅ **Els antivirus actualitzats detecten amenaces conegudes** com WannaCry.  
✅ **Les còpies de seguretat i les instantànies** són la millor defensa contra ransomware.  
⚠️ **El malware pot amagar-se en arxius comprimits**, cal escanejar-ho tot.  
🛡️ **La prevenció (formació, configuracions segures) és més important que la cura**.

---

### Resum de tot el que hem après:

1. **Configurar defenses bàsiques** (antivirus, SmartScreen, polítiques d'execució).
2. **Provar amb fitxers de prova** com EICAR per verificar el funcionament.
3. **Simular atacs controlats** per entendre el comportament del malware.
4. **Analitzar malware real** en entorns aïllats amb eines específiques.
5. **Utilitzar portals d'intel·ligència** per verificar fitxers sospitosos.
6. **Restaurar entorns nets** després de les proves amb instantànies.

---

Aquesta tasca m'ha obert els ulls sobre com de fàcil és que un malware entri al sistema si baixem la guardia, però també m'ha ensenyat que amb les eines i pràctiques adequades, es poden evitar desastres. Ara tinc més clara la importància de la seguretat en un entorn professional. 🚀👨‍💻


# Preguntes:

# Sistemes de protecció Windows 11

## Quines proteccions incorpora Windows 11 a la secció de "Protección antivirus y contra amenazas"?

Aquesta secció del Centre de seguretat de Windows Defender inclou diverses capes de protecció :

1. **Protecció en temps real**: Detecta i bloqueja malware en el moment que intenta executar-se o instal·lar-se al sistema.

2. **Protecció basada en el núvol**: Proporciona una detecció més ràpida i actualitzada gràcies a la informació compartida amb Microsoft.

3. **Enviament automàtic de mostres**: Envia fitxers sospitosos a Microsoft per analitzar-los i millorar la detecció futura.

4. **Protecció contra manipulacions**: Evita que el malware desactivi les proteccions de seguretat del sistema.

5. **Protecció de unitat per a desenvolupadors**: Ofereix anàlisi asíncrona per reduir l'impacte en el rendiment en equips de desenvolupament.

6. **Exclusions de l'antivirus**: Permet configurar carpetes o processos específics que no seran escanejats (útil per a entorns de desenvolupament).

---

## Quines opcions tenim a "Control de aplicaciones y navegador"?

La secció "Control d'aplicacions i navegador" es divideix en tres àrees principals segons la documentació oficial de Microsoft :

### 1. **Smart App Control**
- Funciona amb IA i intel·ligència al núvol per predir la seguretat de les aplicacions
- Només disponible en instal·lacions netes de Windows 11
- Bloqueja aplicacions potencialment perilloses que poden alentir l'equip o mostrar anuncis inesperats
- Tres modes: Avaluació, Activat, Desactivat

### 2. **Protecció basada en reputació**
- **SmartScreen per a aplicacions i fitxers**: Avalua la reputació de les descàrregues web
- **SmartScreen per a Microsoft Edge**: Protegeix contra llocs web maliciosos i phishing
- **Protecció contra phishing**: Adverteix si introduïu la contrasenya de Windows en llocs sospitosos
- **Bloqueig d'aplicacions potencialment no desitjades (PUA)**: Identifica i bloqueja aplicacions que poden mostrar publicitat o minar criptomonedes
- **SmartScreen per a Microsoft Store**: Capa addicional de seguretat per a aplicacions de la botiga

### 3. **Protecció contra exploits**
- Mitigacions preconfigurades per reduir la superfície d'atac
- Es pot personalitzar per al sistema operatiu o aplicacions individuals
- Protegeix contra tècniques comunes d'explotació de vulnerabilitats

---

## Investigueu quines opcions específiques hi ha per la protecció contra ransomware a Windows 11

Windows 11 incorpora una protecció específica anomenada **"Accés controlat a carpetes"** (Controlled Folder Access), que es troba dins de "Protección antivirus y contra amenazas" > "Protección contra ransomware".

**Característiques principals**:

1. **Protecció de carpetes predeterminades**: Protegeix automàticament carpetes com Documents, Imatges, Vídeos, Música i Escriptori.

2. **Permet afegir carpetes personalitzades**: Els usuaris poden protegir carpetes addicionals on guardin dades importants.

3. **Llista d'aplicacions permeses**: Només les aplicacions de la llista blanca poden modificar fitxers dins de les carpetes protegides.

4. **Alertes en temps real**: Quan una aplicació no autoritzada intenta modificar un fitxer protegit, l'usuari rep una notificació.

5. **Historial de bloquejos**: Permet revisar quins intents s'han bloquejat i gestionar excepcions si cal.

---

# Atacs de Ransomware: WannaCry

## Expliqueu quins són els factors que fan que WannaCry es propagui tan ràpid. Expliqueu què vol dir.

WannaCry es propaga ràpidament per diversos factors clau :

**Factor principal: EternalBlue**
WannaCry utilitza un exploit anomenat **EternalBlue**, que va ser desenvolupat originalment per l'Agència de Seguretat Nacional dels EUA (NSA) i posteriorment filtrat per un grup hacker anomenat Shadow Brokers l'abril de 2017.

**Què vol dir això?**
- **Propagació automàtica**: A diferència d'altres ransomware que necessiten que l'usuari faci clic en un enllaç o obri un fitxer adjunt, WannaCry s'escampa automàticament per la xarxa sense interacció de l'usuari.
- **Velocitat d'infecció**: Un sistema Windows infectat escaneja aproximadament **25 adreces IP aleatòries per segon** buscant altres ordinadors amb el port 445 (SMB) obert .
- **Comportament de cuc**: Quan troba un altre ordinador vulnerable, s'hi copia automàticament i l'infecta, creant una reacció en cadena.
- **Impacte organitzacional**: En una empresa, pot paralitzar centenars d'ordinadors en qüestió de minuts perquè es propaga per la xarxa interna.

---

## Quina vulnerabilitat en concret es fa servir? Busqueu el CVE associat. És molt greu?

La vulnerabilitat específica que explota WannaCry és en el **protocol SMBv1** (Server Message Block) de Windows .

**CVE associat**: **CVE-2017-0144** (també relacionats: CVE-2017-0143, CVE-2017-0145, CVE-2017-0146, CVE-2017-0147, CVE-2017-0148)

**És molt greu?**
Sí, és **extremadament greu** per diverses raons:
- Permet l'**execució remota de codi** sense autenticació
- No requereix cap interacció de l'usuari
- Es pot propagar automàticament per la xarxa
- Afecta múltiples versions de Windows (Vista, 7, 8.1, 10, Server 2008, 2012, 2016) 
- Microsoft va qualificar la vulnerabilitat com a **crítica** i va publicar el pedaç MS17-010 el març de 2017, però moltes organitzacions no l'havien instal·lat quan va esclatar l'atac al maig 

---

## S'ha de pagar el rescat demanat? Per què? Busqueu per internet a veure si trobeu alguna empresa negociadora de rescats i com funciona.

**No, NO s'ha de pagar el rescat** per diverses raons fonamentals:

**Per què no pagar** :
1. **Entre el 35% i el 40% de les vegades**, els delinqüents no compleixen la seva part del tracte: ni retornen l'accés, ni restableixen les dades, o les acaben publicant igualment.
2. No hi ha garantia que recuperis els fitxers encara que paguis.
3. Estàs finançant activitats criminals i organitzacions delictives.
4. Pots ser marcat com a "objectiu fàcil" per a futurs atacs.
5. En molts països, pagar rescats a ciberdelinqüents pot tenir implicacions legals.

**Empreses negociadores de rescats** :

Tot i que no es recomana pagar, existeixen empreses especialitzades en **negociació amb ciberdelinqüents**. Funcionen com a intermediaris professionals:

**Com funciona**:
1. **Avaluació inicial**: Analitzen l'abast de l'atac i el perfil de l'atacant.
2. **Estratègia de negociació**: Gestionen els temps de resposta per guanyar temps mentre els equips tècnics treballen en la recuperació.
3. **Prova de vida**: Sol·liciten que l'atacant desxifri un fitxer com a demostració que té la clau.
4. **Obtenció d'intel·ligència**: Intenten esbrinar si els atacants encara tenen accés a la infraestructura, quant de temps han estat dins, i si realment posseeixen la informació que diuen tenir.
5. **Gestió del pagament**: Si finalment es paga (cosa que no recomanen), gestionen les transaccions en criptomonedes.

**Exemples d'empreses** :
- **Lazarus Technology**: Proveïdor global de ciberseguretat amb més de 700 incidents gestionats.
- **Coveware**
- **GroupSense**
- **LMG Security**

**Filosofia**: "Negociar no és claudicar, sinó gestionar bé per guanyar temps, obtenir informació i recuperar el control" .

---

## Quines mesures podem aplicar si volem PREVENIR un atac de Ransomware abans que passi?

Segons el CERT.br i experts en seguretat, les mesures preventives essencials són :

### 1. **Mantenir els sistemes actualitzats**
- Instal·lar tots els pedaços de seguretat (especialment el MS17-010 per WannaCry)
- Prioritzar la correcció de sistemes i serveis exposats a Internet

### 2. **Utilitzar autenticació multifactor (MFA)**
- Especialment per a accés remot (VPN, escriptoris remots)
- Per a comptes amb privilegis d'administrador

### 3. **Fer còpies de seguretat periòdiques i protegir-les**
- Regla 3-2-1: 3 còpies, 2 mitjans diferents, 1 fora de l'oficina
- Mantenir les còpies fora de línia (offline) per evitar que també siguin xifrades
- Provar periòdicament que la restauració funciona correctament

### 4. **Segmentar la xarxa**
- Dividir la xarxa en segments més petits i independents
- Aïllar sistemes crítics, equips d'usuaris i sistemes heredats
- Limitar el moviment lateral en cas d'infecció

### 5. **Reduir la superfície d'atac**
- Desactivar serveis que no s'utilitzin
- No exposar serveis innecessàriament (RDP, SMB)
- Desactivar SMBv1 si no és estrictament necessari

### 6. **Educar els usuaris**
- Formació per reconèixer phishing i altres tècniques d'enginyeria social
- Saber quins són els canals oficials de suport tècnic
- Protocol per informar sobre possibles incidents

### 7. **Principi de mínim privilegi**
- Otorgar només els permisos necessaris per a cada funció
- Limitar el nombre de comptes amb accés privilegiat
- Revisar periòdicament comptes i permisos

### 8. **Activar proteccions específiques de Windows**
- Accés controlat a carpetes (protecció contra ransomware)
- Protecció en temps real de l'antivirus
- SmartScreen i protecció basada en reputació

---

## Quines mesures aplicarem si JA HEM SOFERT un atac de WannaCry i no hem aplicat les mesures de prevenció o ho hem fet parcialment?

Davant un atac actiu de ransomware, cal seguir un protocol d'actuació urgent :

### 1. **Aïllar l'equip infectat immediatament**
- Desconnectar el cable de xarxa i desactivar el Wi-Fi
- Apagar el Bluetooth i altres connexions sense fil
- No apagar l'equip (es podrien perdre proves forenses)

### 2. **Contenir la propagació**
- A nivell de xarxa, bloquejar el trànsit SMB (port 445) al tallafocs
- Desconnectar unitats de xarxa i emmagatzematge compartit
- Aïllar el segment de xarxa afectat

### 3. **Documentar l'atac**
- Fer captures de pantalla del missatge de rescat
- Anotar l'adreça de Bitcoin dels atacants
- Registrar data i hora exactes de la infecció
- Identificar quins fitxers han estat xifrats (extensió .WNCRY)

### 4. **Notificar a les autoritats**
- En molts països és obligatori reportar ciberatacs
- Contactar amb l'INCIBE (Espanya) o CERT de referència
- Algunes autoritats tenen eines de desxifrat per a ransomware conegut

### 5. **Intentar recuperar amb còpies de seguretat**
- Si tenim còpies netes i recents (fora de línia), restaurar des d'elles
- Abans de restaurar, assegurar-se que l'amenaça està completament eliminada

### 6. **NO PAGAR EL RESCAT** (reiterem)
- Recordar que el 35-40% dels casos no recuperaràs les dades encara que paguis 
- Estàs finançant el crim organitzat

### 7. **Eines de desxifrat específiques**
- Per a WannaCry, si l'equip NO s'ha reiniciat després de la infecció, eines com **WannaKey** o **Wanakiwi** poden recuperar les claus de xifrat de la memòria RAM .
- Consultar pàgines com **No More Ransom Project** (iniciativa d'Europol) per veure si hi ha eines de desxifrat disponibles.

### 8. **Contactar amb experts en seguretat**
- Empreses especialitzades poden analitzar l'abast de l'atac
- Poden ajudar a identificar com va entrar el malware per tancar la bretxa
- Assessorar en la recuperació segura dels sistemes

### 9. **Pla de comunicació**
- Informar el personal intern, clients i proveïdors de forma controlada
- Evitar rumors que poden causar més dany reputacional que l'atac mateix 

### 10. **Un cop recuperat, revertir a instantània neta**
- En entorns de proves (com la teva màquina virtual), apagar i restaurar la instantània "Abans del virus" [citation:34.png]
