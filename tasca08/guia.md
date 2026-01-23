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

