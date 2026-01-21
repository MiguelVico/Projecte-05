# T05: Instal·lació del domini translogic26.test

## Introducció
En aquesta tasca, anem a instal·lar i configurar un domini Active Directory a Windows Server 2025. La idea és crear un entorn de prova per al client TransLògic, on el domini es dirà `translogic26.test` (el 26 és el meu número de llista). Després de fer tot el procés, guardarem un script de PowerShell per poder repetir la instal·lació automàticament si cal.

---

## 1. Obrirem el Server Manager i començarem a afegir rols
Primer, obrim el **Server Manager** i anem a **Administrar → Afegir rols i funcions**.

![Pantalla inicial del Server Manager](/tasca05/imgT05/captura4.png)

Aquí seleccionem **Role-based or feature-based installation** per configurar un servidor afegint rols i funcions.

---

## 2. Seleccionem el servidor on instal·lar
En aquest pas, triem el servidor on instal·larem el domini. En el meu cas, el servidor és **DC26** amb IP `10.0.2.4`.

![Selecció del servidor DC26](/tasca05/imgT05/captura5.png)

**Anàlisi:**  
Aquí veiem que hi ha un servidor disponible al pool amb Windows Server 2025 Standard Evaluation. És el servidor que farem servir per promocionar a controlador de domini.

---

## 3. Seleccionem el rol **Active Directory Domain Services**
Ara marquem la casella per instal·lar el rol d’Active Directory Domain Services.

![Selecció del rol Active Directory Domain Services](/tasca05/imgT05/captura6.png)

En clicar, ens demana que afegim també les **eines d’administració necessàries**. Fem clic a **Add Features** per incloure-les totes.

---

## 4. Instal·lació del rol
Després de confirmar, comença la instal·lació. Podem veure el progrés a la següent pantalla:

![Progrés de la instal·lació](/tasca05/imgT05/captura7.png)

Aquí es mostren les eines que s’estan instal·lant:
- Group Policy Management
- Remote Server Administration Tools
- AD DS and AD LDS Tools
- Active Directory PowerShell module
- AD DS Snap-Ins, etc.

---

## 5. Configuració del Domini – Deployment Configuration
Un cop instal·lat el rol, ens apareix l’assistent de configuració. Seleccionem **Add a new forest** per crear un domini nou des de zero.

![Configuració del Deployment – Nou bosc](/tasca05/imgT05/captura8.png)

**Anàlisi:**  
Aquí és on definim el nom del domini: **translogic26.test**. Aquest serà el nom del domini i també del bosc.

---

## 6. Opcions del Controlador de Domini
Ara configurem les opcions del controlador de domini:

![Opcions del Domain Controller](/tasca05/imgT05/captura9.png)

Seleccionem:
- **Forest functional level**: Windows Server 2025
- **Domain functional level**: Windows Server 2025
- Marquem **DNS server** i **Global Catalog**
- Posem la contrasenya de **Directory Services Restore Mode (DSRM)**

---

## 7. Opcions de DNS
Ens apareix un avís que no es pot crear la delegació DNS perquè no hi ha cap zona pare. És normal, ja que és el primer servidor DNS del domini.

![Opcions de DNS](/tasca05/imgT05/captura10.png)

Deixem la casella **Create DNS delegation** sense marcar.

---

## 8. Nom NetBIOS del domini
El sistema proposa automàticament el nom NetBIOS **TRANSLOGIC26**. El deixem tal qual.

![Nom NetBIOS](/tasca05/imgT05/captura11.png)

---

## 9. Ubicació de les carpetes d’AD DS
Aquí definim on es guardaran la base de dades, els fitxers de log i la carpeta SYSVOL. Per defecte, tot anirà a `C:\WINDOWS\NTDS` i `C:\WINDOWS\SYSVOL`.

![Ubicació de les carpetes](/tasca05/imgT05/captura12.png)

---

## 10. Revisió de les opcions
Abans d’instal·lar, podem revisar tot el que hem configurat.

![Revisió de les opcions](/tasca05/imgT05/captura13.png)

**Anàlisi:**  
Veiem un resum complet:
- Domini: translogic26.test
- Nom NetBIOS: TRANSLOGIC26
- Nivell funcional: Windows Server 2025
- DNS: Sí
- Global Catalog: Sí

També hi ha l’opció **View script** per veure i guardar el script de PowerShell equivalent.

---

## 11. Comprovació de prerrequisits
El sistema fa una comprovació automàtica i tot passa correctament, tot i alguns avisos:

![Comprovació de prerrequisits](/tasca05/imgT05/captura14.png)

Els avisos són:
1. Que el servidor no té una IP estàtica assignada (recomanable però no obligatori per la prova).
2. Que no es pot crear delegació DNS.
3. Que el servidor es reiniciarà automàticament després de la instal·lació.

---

## 12. Instal·lació i resultats
Fem clic a **Install** i esperem que s’instal·li tot. Quan acabi, veurem aquesta pantalla:

![Resultat de la instal·lació](/tasca05/imgT05/captura15.png)

**Anàlisi:**  
El servidor s’ha configurat correctament com a controlador de domini i es reiniciarà automàticament.

---

## 13. Inici de sessió al nou domini
Després del reinici, ja podrem iniciar sessió amb el compte d’administrador del domini: **TRANSLOGIC26\Administrator**.

![Inici de sessió al domini](/tasca05/imgT05/captura16.png)

---

## 14. Guardar l’script de PowerShell
Durant la configuració (pas 10), vam poder **View script**. Allà hi havia totes les comandes de PowerShell per replicar la instal·lació automàticament. Aquest script el guardem a la carpeta del repositori per si el necessitem en el futur.

**Exemple del contingut del script** (extret de l’assistent):
```powershell
Install-ADDSForest `
-DomainName "translogic26.test" `
-DomainNetbiosName "TRANSLOGIC26" `
-ForestMode "WinThreshold" `
-DomainMode "WinThreshold" `
-InstallDns:$true `
-NoRebootOnCompletion:$false `
-Force:$true
```

---

## Resum
En aquesta tasca hem:
1. Instal·lat el rol **Active Directory Domain Services**.
2. Configurat un **nou bosc** amb el domini `translogic26.test`.
3. Establert el nivell funcional a **Windows Server 2025**.
4. Promocionat el servidor com a **controlador de domini** amb DNS i Global Catalog.
5. Guardat l’**script de PowerShell** per automatitzar el procés.

Ara ja tenim un entorn de domini actiu preparat per fer proves i mostrar al client com funcionaria en producció.

