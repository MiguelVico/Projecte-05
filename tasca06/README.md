# T06: Configuració del domini

## 📋 Descripció de la tasca
Aquesta tasca consisteix en configurar un domini Active Directory des de zero. Després de crear el domini, organitzarem tots els objectes (usuaris, grups i equips) utilitzant Unitats Organitzatives (OU). També aprendrem a crear plantilles d'usuari per estalviar temps i assegurar consistència en la configuració.

## 🎯 Objectius
- Organitzar objectes del domini amb Unitats Organitzatives (OU)
- Crear i gestionar grups de seguretat
- Treballar amb plantilles d'usuari per a departaments diferents
- Afegir equips al domini
- Verificar el funcionament del domini amb usuaris reals

## 🏗️ Estructura del projecte
El projecte inclou dues guies tècniques:

- **`GuiaWindows.md`** - Guia completa per a la configuració en Windows (client)
- **`GuiaServer.md`** - Guia completa per a la configuració al servidor Linux/Windows Server

## 📁 Estructura del domini
```
Dominio.local
├── Empresa
│   ├── Usuaris
│   ├── Equips
│   └── Grups
│       ├── gestio
│       ├── magatzem
│       ├── gerencia
│       └── personal (conté tots els grups anteriors)
```

## 🛠️ Tasques principals
1. **Creació d'Unitats Organitzatives (OU)**
   - Estructura jeràrquica per organitzar els objectes

2. **Configuració de grups de seguretat**
   - Creació de grups per departaments
   - Grup "personal" que engloba tots els departaments

3. **Plantilles d'usuari**
   - Plantilla per a cada departament (Gestio, Magatzem, Gerencia)
   - Configuració automàtica de carpetes personals

4. **Usuaris de prova**
   - Creació d'usuaris reals a partir de les plantilles
   - Assignació de contrasenyes temporals

5. **Integració d'equips**
   - Configuració d'un equip Windows 11
   - Addició al domini
   - Verificació d'accés

## 🖼️ Captures de pantalla
Les captures de pantalla estan numerades seqüencialment (`captura17.png` a `captura22.png`) i mostren tot el procés:
- Creació d'OU i grups
- Configuració de plantilles
- Creació d'usuaris
- Addició d'equips al domini
- Prova d'accés

## 📚 Competències treballades
- **Instal·lació i configuració de programari** en condicions de qualitat i seguretat
- **Configuració de serveis multiusuari** en entorn de xarxa local
- **Gestió d'usuaris i grups** en sistemes operatius en xarxa
- **Administració de dominis** amb eines específiques

## 🎓 Resultats d'Aprenentatge (RA)
- **RA2**: Gestiona usuaris i grups de sistemes operatius en xarxa
- **RA3**: Realitza tasques de gestió sobre dominis

## 🔧 Prerequisits
- Servidor amb Active Directory configurat
- Client Windows 10/11
- Accés d'administrador al domini
- Connexió de xarxa entre servidor i client

## 📅 Termini i lliurament
- **Producte final**: Guia d'instal·lació en format MarkDown
- **Carpeta del repositori**: `/tasca06/`
- **Enllaç al Moodle**: Seguir indicacions de l'aula virtual

## 👥 Treball en equip
Aquesta tasca promou:
- Autonomia en la resolució de problemes
- Organització del treball
- Responsabilitat en la configuració de sistemes crítics
- Innovació en l'aproximació a solucions tècniques

---

**Autor**: [El teu nom]  
**Data**: [Data de realització]  
**Curs**: 0224 Sistemes Operatius en Xarxa
