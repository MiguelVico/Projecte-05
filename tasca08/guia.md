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

---

## Introducció

En aquesta tasca hem vist com funciona la protecció contra malware en Windows 11, des de la configuració bàsica fins a simular un atac de ransomware per entendre com actuen les defenses i què passa quan fallen. La idea és aprendre a identificar riscos, configurar proteccions i entendre l’impacte del malware en un entorn controlat.

---

## Configuració inicial de seguretat

Abans de res, cal veure com està configurada la seguretat del sistema. A la **captura1.png** es mostra la configuració de seguretat de Microsoft Edge, on es poden gestionar certificats, activar SmartScreen, bloquejar aplicacions no desitjades i configurar DNS segur.

![Configuració de seguretat de Microsoft Edge](/tasca08/img_T08/captura1.png)

**Anàlisi**: Aquí es veu que l’usuari té accés a totes les opcions de seguretat del navegador, incloent protecció contra phishing i descarregues malicioses. Això és la primera línia de defensa quan naveguem per Internet.

---

A la **captura5.png** i **captura6.png** es mostra el panell de seguretat de Windows Security. Tot està en verd, així que el sistema diu que “No es requereix cap acció”. Però cal anar amb compte, perquè de vegades aquestes alertes no detecten tot.

![Panell de seguretat de Windows](/tasca08/img_T08/captura5.png)
![Estat de la protecció antivirus](/tasca08/img_T08/captura6.png)

**Anàlisi**: Tot sembla correcte, però hem de verificar si la protecció en temps real està activada. Més endavant veurem que no ho estava, i això va ser clau.

---

## Test amb fitxer EICAR

Per provar si l’antivirus funciona, es fa servir un fitxer de prova anomenat **EICAR**. És un fitxer inofensiu però que tots els antivirus el detecten com a virus per provar que funcionen.

A la **captura2.png** i **captura3.png** es veu la pàgina web d’EICAR, on es poden descarregar diverses versions del fitxer de prova.

![Pàgina web d'EICAR](/tasca08/img_T08/captura2.png)
![Àrea de descàrrega d'EICAR](/tasca08/img_T08/captura3.png)

**Anàlisi**: El fitxer EICAR és un estàndard de la indústria per provar antivirus. No fa mal, però si l’antivirus el detecta, vol dir que està actiu i vigilant.

---

A la **captura4.png** es veu com Microsoft Edge amb SmartScreen activat **bloqueja la descàrrega** del fitxer `eicar_com.zip` perquè detecta que és un virus.

![Descàrrega bloquejada per SmartScreen](/tasca08/img_T08/captura4.png)

**Anàlisi**: El navegador actua com a primera barrera i evita que es descarregui un fitxer potencialment perillós, encara que en aquest cas sigui només de prova. Això demostra que les proteccions integrades funcionen.

---

Tot i així, l’usuari descarrega altres versions del fitxer EICAR com `.7z`, `.tar`, i `.zip` com es veu a la **captura11.png** i **captura12.png**.

![Contingut de la carpeta Descàrregues](/tasca08/img_T08/captura11.png)
![Fitxers EICAR descarregats](/tasca08/img_T08/captura12.png)

**Anàlisi**: L’usuari ha pogut descarregar els fitxers comprimits. Això pot ser perquè l’antivirus no escaneja fitxers comprimits amb la mateixa agressivitat, o perquè la protecció en temps real no està activada.

---

A la **captura10.png** es mostra una alerta de Windows Security quan s’intenta executar `eicar.com`. El sistema l’identifica com a “Arxiu malintencionat” i recomana no executar-lo.

![Alerta de fitxer malintencionat](/tasca08/img_T08/captura10.png)

**Anàlisi**: Windows Defender sí que detecta l’amenaça quan es vol executar el fitxer. És una protecció en segon nivell, però depèn que l’usuari no ignori l’avís.

---

## Descarrega i execució de ransomware simulat

Aquí ve el més intens: simular un atac de ransomware. Primer, l’usuari descarrega un script de PowerShell anomenat **PSRansom.ps1**.

Abans d’executar-lo, cal canviar la política d’execució de PowerShell per permetre scripts no signats. Això es veu a la **captura14.png**.

![Canvi de política d'execució a PowerShell](/tasca08/img_T08/captura14.png)

**Anàlisi**: Amb la comanda `Set-ExecutionPolicy -ExecutionPolicy Unrestricted` s’eliminen les restriccions per executar scripts. Això és **molt perillós** i només s’ha de fer en entorns de prova com aquest.

---

Després, s’executa el script `PSRansom.ps1` (captura15.png). El script simula un atac de ransomware: genera una clau AES de 256 bits, xifra fitxers de prova i guarda un log a `readme.txt`.

![Execució del ransomware simulat](/tasca08/img_T08/captura15.png)

**Anàlisi**: El script mostra informació del sistema (hostname, usuari, hora) i simula la comunicació amb un servidor de Comandament i Control (C&C). Com el servidor està caigut, genera una clau local i xifra els fitxers. Això passa perquè l’antivirus no ha detectat el script com a maliciós, possiblement perquè la protecció en temps real estava desactivada.

---

## Anàlisi de l’atac i resultats

Després de l’execució, els fitxers de prova (`prova1.txt`, `prova2.txt`, `prova3.txt`) queden xifrats amb extensió `.psr` (captura16.png).

![Fitxers xifrats després de l'atac](/tasca08/img_T08/captura16.png)

**Anàlisi**: Els fitxers originals han estat reemplaçats per versions xifrades. El ransomware també ha creat un fitxer `readme.txt` amb les instruccions (suposem) i la clau de xifrat.

---

Si obrim un fitxer xifrat (captura17.png), es veu contingut binari/aleatori, confirmant que està xifrat.

![Contingut d'un fitxer xifrat](/tasca08/img_T08/captura17.png)

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

**Nota**: Aquesta guia s’ha fet a partir de les captures 1 a 17. Si hi ha més captures (fins a 34), es poden afegir després per complementar l’anàlisi.
