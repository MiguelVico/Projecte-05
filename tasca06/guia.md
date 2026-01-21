# T06: Configuració del domini

## Introducció
Aquesta tasca consisteix en configurar el nostre domini Active Directory. Després de crear el domini, toca organitzar-lo: crear unitats organitzatives (OU), grups, usuaris i equips. També aprendrem a fer servir plantilles d'usuari per estalviar temps.

## Estructura de la tasca
1. Crear les Unitats Organitzatives (OU)
2. Crear els grups de seguretat
3. Crear plantilles d'usuari per a cada grup
4. Crear usuaris de prova amb les plantilles
5. Afegir un equip (PC1) al domini
6. Provar l'accés amb els usuaris creats

---

## 1. Creació de les Unitats Organitzatives (OU)

Primer, obrim **Server Manager** i anem a **Tools → Active Directory Users and Computers**.

Creem les següents OU dins del nostre domini:
- **Empresa** (arrel)
  - **Usuaris**
  - **Equips**
  - **Grups**

![Creació de les OU](/tasca06/imgT06/captura17.png)

**Anàlisi**: Aquí es veu la jerarquia que hem creat. Les OU serveixen per organitzar els objectes del domini de manera lògica, com si fossin carpetes.

---

## 2. Creació dels grups de seguretat

Dins de la OU **Grups**, creem els següents grups de seguretat (Security Groups):
- **gestio**
- **magatzem**
- **gerencia**
- **personal** (aquest grup contindrà tots els anteriors)

![Creació del grup gestio](/tasca06/imgT06/captura18.png)

Per afegir els grups a **personal**, anem a les propietats del grup **personal** → tab **Members** → **Add** i hi afegim els grups gestio, magatzem i gerencia.

**Anàlisi**: Els grups de seguretat ens permeten assignar permisos a molts usuaris de cop. El grup **personal** actua com a "supergrup" que conté tots els altres, així podem assignar permisos comuns fàcilment.

---

## 3. Creació de plantilles d'usuari

A la OU **Usuaris**, creem tres comptes d'usuari que faran de plantilla:
- **Plantilla_Gestio**
- **Plantilla_Magatzem**
- **Plantilla_Gerencia**

Per a cada plantilla:
1. Desactivem la conta (Account is disabled)
2. A la pestanya **Member Of**, l'afegim al grup corresponent (gestio, magatzem o gerencia)
3. A la pestanya **Profile**, posem la ruta de la carpeta personal (ex: `\\server\usuaris\%username%`)

![Configuració de la plantilla](/tasca06/imgT06/captura19.png)

**Anàlisi**: Les plantilles d'usuari ens estalvien temps quan hem de crear molts usuaris amb la mateixa configuració. Només cal copiar-les i canviar el nom.

---

## 4. Creació d'usuaris de prova

Ara copiem cada plantilla per crear usuaris reals:
1. Clic dret a la plantilla → **Copy**
2. Posem el nom d'usuari (ex: **usuari_gestio1**)
3. Assignem contrasenya i marquem **User must change password at next logon**
4. Activem el compte (desmarquem **Account is disabled**)

Repetim el procés per a cada àrea.

![Creació d'usuari a partir de plantilla](/tasca06/imgT06/captura20.png)

**Anàlisi**: En copiar la plantilla, l'usuari nou hereda totes les propietats: grup, carpeta personal, etc. Així ens assegurem que tots els usuaris del mateix departament tinguin la mateixa configuració.

---

## 5. Afegir un equip al domini (PC1)

Ara anem a una màquina amb Windows 11 (client) i:
1. Obrim **Settings → System → About**
2. Clic a **Rename this PC (advanced)**
3. A la pestanya **Computer Name**, clic a **Change**
4. Posem el nom de l'equip: **PC1**
5. Marquem **Domain** i posem el nom del nostre domini
6. Posem credencials d'administrador del domini
7. Reiniciem

![Afegir equip al domini](/tasca06/imgT06/captura21.png)

**Anàlisi**: Quan afegim un equip al domini, aquest apareix automàticament a la OU **Computers** de l'Active Directory. Després el podem moure a la nostra OU **Equips**.

---

## 6. Prova d'accés amb usuaris del domini

Des del PC1, fem logoff i intentem iniciar sessió:
1. A la pantalla d'inici, clic a **Other user**
2. Posem l'usuari del domini (ex: `domini\usuari_gestio1`)
3. Introduïm la contrasenya
4. El sistema ens demanarà canviar-la (si vam configurar això)

![Inici de sessió amb usuari del domini](/tasca06/imgT06/captura22.png)

**Anàlisi**: Si tot ha sortit bé, podrem iniciar sessió amb qualsevol dels usuaris creats. Això demostra que el domini està funcionant correctament i que els usuaris tenen accés als equips clients.

---

## Conclusió
En aquesta tasca hem après a organitzar un domini Active Directory amb OU, grups i plantilles d'usuari. Aquesta organització és clau per administrar xarxes grans de manera eficient. També hem comprovat que els usuaris poden accedir des d'un equip client, que és l'objectiu principal d'un domini.
