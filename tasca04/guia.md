Guia d'Instal·lació Windows Server 2025
Introducció
Aquesta guia explica pas a pas com instal·lar i configurar Windows Server 2025 per a TransLògic S.A., seguint els requeriments especificats al full de tasca. Documentarem tot el procés amb captures de pantalla i explicacions clares.

1. Comparativa de Requisits
Requisits de Microsoft vs. Nossa Configuració
Component	Mínim de Microsoft	La nostra VM	Coherent?
RAM	512 MB	8 GB	✅ Sí (sobra)
Processadors	1.4 GHz 64-bit	2 processadors	✅ Sí
Espai disc	32 GB	Disc 1: 32 GB
Disc 2: 10 GB	✅ Sí
Xarxa	-	2 interfícies (NAT + Host-only)	✅ Sí (per les pròximes fases)
Conclusió: La nostra configuració supera amb escreix els requisits mínims de Microsoft, cosa que assegura un bon rendiment.

2. Procés d'Instal·lació
Pas 1: Configuració d'idioma i teclat
A la primera pantalla de setup, configurem l'idioma d'instal·lació a anglès (USA) però el format hora/moneda i el teclat a espanyol. Això permet tenir la base en anglès (més compatible) però la interfície adaptada al nostre entorn.

https:///tasca04/img_t04/captura1.png

https:///tasca04/img_t04/captura4.png

Pas 2: Tipus d'instal·lació
Seleccionem "Install Windows Server" i acceptem que es borraran totes les dades (és una instal·lació nova).

https:///tasca04/img_t04/captura5.png

Pas 3: Selecció d'edició
Triem Windows Server 2025 Standard Evaluation (Desktop Experience) per tenir l'entorn gràfic (GUI), necessari per a la gestió visual.

https:///tasca04/img_t04/captura6.png

Pas 4: Configuració d'administrador
Creem la contrasenya per al compte Administrator. És molt important posar una contrasenya segura i recordar-la.

https:///tasca04/img_t04/captura7.png

3. Configuració Post-Instal·lació
Canvi de nom de l'equip
Un cop dins del sistema, anem a Server Manager → Local Server → Properties i canviem el nom de l'equip a DC26 (en aquest cas, el número 26 correspon al número de llista).

https:///tasca04/img_t04/captura2.png

Observació: Després de canviar el nom, cal reiniciar el servidor perquè els canvis tinguin efecte.

Configuració de xarxa
Comprovem que tenim les dues interfícies de xarxa configurades:

Una en mode NAT (per accés a internet)

Una en mode host-only (per xarxa interna)

També configurem el DNS a 127.0.0.1 (localhost) perquè més endavant aquest servidor farà de controlador de domini i servidor DNS.

https:///tasca04/img_t04/captura3.png

4. Actualització del Sistema
Procés d'actualització
Anem a Settings → Windows Update

Busquem actualitzacions disponibles

Instal·lem totes les actualitzacions crítiques i de seguretat

Un cop actualitzat, posem en pausa les actualitzacions durant el màxim temps possible per evitar reinicis inesperats durant les proves.

Per què posar en pausa?: En un entorn de prova, volem tenir control sobre quan es reinicia el servidor, no que ho faci automàticament per una actualització.

5. Comprovacions Finals
Verificacions a fer:
✅ Nom de l'equip: DC26

✅ Dues interfícies de xarxa actives

✅ DNS configurat a 127.0.0.1

✅ Sistema completament actualitzat

✅ Accés a internet (des de la interfície NAT)

✅ Connexió local (des de la interfície host-only)

Possibles errors/avisos:
A la imatge del Server Manager es veuen alguns avisos (warnings) relacionats amb el servei de temps i Remote Management. Aquests són normals en una instal·lació fresca i es poden resoldre més endavant quan configurem els serveis específics.

6. Conclusions
Hem aconseguit instal·lar correctament Windows Server 2025 seguint tots els requisits del client:

✅ Configuració de maquinari adequada (sobra)

✅ Idioma anglès amb teclat i format espanyol

✅ Mode Desktop Experience (GUI)

✅ Nom d'equip personalitzat DCxx

✅ Actualitzacions aplicades i posades en pausa

✅ Xarxa configurada per a les properes fases (domini, DNS, etc.)

Aquesta instal·lació serveix com a plantilla per a les properes VMs que s'hagin de desplegar a TransLògic S.A.
