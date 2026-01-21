# T04: Instal·lació Windows Server 2025

## 📌 Descripció del Projecte

Aquest projecte documenta el procés complet d'instal·lació i configuració d'un servidor **Windows Server 2025** en una màquina virtual, seguint les especificacions sol·licitades per l'empresa fictícia **TransLògic S.A**.

L'objectiu principal és crear una guia tècnica detallada que serveixi com a referència per a futurs desplegaments de servidors Windows dins de l'empresa.

---

## 🎯 Objectius de la Tasca

- **Instal·lar** Windows Server 2025 en mode GUI amb configuració multillenguatge
- **Configurar** una màquina virtual amb especificacions tècniques concretes
- **Documentar** tot el procés amb captures de pantalla i explicacions
- **Comparar** la nostra configuració amb els requisits oficials de Microsoft
- **Actualitzar** i assegurar el sistema operatiu

---

## 🛠️ Configuració Tècnica Requerida

### Especificacions de la Màquina Virtual:
- **RAM:** 8 GB
- **Processadors:** 2 nuclis
- **Disc Principal:** 32 GB (per al SO)
- **Disc Secundari:** 10 GB (emmagatzematge addicional)
- **Interfícies de Xarxa:**
  - 1 x Xarxa NAT (per a connexió a Internet)
  - 1 x Xarxa Host-only (per a xarxa interna)

### Requisits del Sistema Operatiu:
- **Windows Server 2025 Standard Evaluation**
- **Idioma:** English (United States) amb teclat espanyol
- **Nom de l'equip:** DCxx (on xx és el número de llista)
- **Mode:** Desktop Experience (amb interfície gràfica)

---

## 📋 Procés d'Instal·lació - Resum

### 1. **Preparació del Entorn**
   - Creació de la màquina virtual amb VirtualBox/VMware
   - Configuració dels recursos hardware
   - Muntatge de la imatge ISO de Windows Server 2025

### 2. **Instal·lació del Sistema Operatiu**
   - Selecció d'idioma anglès amb format espanyol
   - Instal·lació de l'edició Standard amb Desktop Experience
   - Creació de la contrasenya d'administrador

### 3. **Configuració Post-Instal·lació**
   - Canvi del nom de l'equip a DC26
   - Configuració de les interfícies de xarxa
   - Actualització del sistema operatiu
   - Pausa d'actualitzacions automàtiques

### 4. **Verificació Final**
   - Comprovació dels recursos del sistema
   - Test de connectivitat de xarxa
   - Validació de la configuració DNS

---

## 📁 Estructura del Repositori

```
tasca04/
│
├── imgT04/                    # Carpeta amb totes les captures
│   ├── captura1.png
│   ├── captura2.png
│   ├── captura3.png
│   ├── captura4.png
│   ├── captura5.png
│   ├── captura6.png
│   └── captura7.png
│
├── GuiaWindows.md            # Guia tècnica completa de la instal·lació
├── README.md                 # Aquest arxiu
└── documentacio_extra/       # Documentació addicional (si n'hi ha)
```

---

## 🔍 Anàlisi Comparatiu

### Requisits Oficials Microsoft vs. Nostra Configuració:

| Component | Mínim Microsoft | Configuració Nostra | Compatibilitat |
|-----------|-----------------|---------------------|----------------|
| **RAM** | 512 MB | 8 GB | ✅ Supera requisits |
| **CPU** | 1.4 GHz 64-bit | 2 processadors | ✅ Supera requisits |
| **Disc Dur** | 32 GB | 32 GB + 10 GB | ✅ Complert |
| **Ethernet** | 1 Gbps | 2 interfícies | ✅ Supera requisits |

**Conclusió:** La nostra configuració compleix i supera tots els requisits mínims, garantint un rendiment òptim per a funcions de servidor.

---

## 🖼️ Captures de Pantalla Clau

Totes les captures estan numerades seqüencialment i mostren els passos més importants del procés:

1. **Selecció d'idioma i configuració regional**
2. **Server Manager i canvi de nom de l'equip**
3. **Configuració de DNS i xarxa**
4. **Selecció del teclat espanyol**
5. **Opció d'instal·lació vs. reparació**
6. **Triar l'edició de Windows Server**
7. **Crear contrasenya d'administrador**

---

## 📚 Competències Treballades

### Competències Tècniques:
- **RA1:** Instal·lació de sistemes operatius en xarxa
- **CA1.1:** Estudi de compatibilitat del sistema
- **CA1.2:** Diferència entre modes d'instal·lació
- **CA1.8:** Actualització del sistema operatiu
- **CA1.9:** Comprovació de connectivitat

### Capacitats Clau:
- ✅ **Autonomia** - Treball individual planificat
- ✅ **Organització** - Documentació estructurada
- ✅ **Resolució de problemes** - Configuració de xarxa complexa
- ✅ **Responsabilitat** - Seguiment de protocols tècnics

---

## ⚠️ Punts Crítics i Solucions

### Problemes Potencials:
1. **Configuració de xarxa dual:** Assegurar que les dues interfícies no tinguin conflictes d'IP
2. **Espai en disc:** Verificar que el disc de 32 GB és suficient per al SO + aplicacions
3. **Actualitzacions:** Recordar pausar-les després de la instal·lació inicial

### Solucions Implementades:
- Assignació d'IP estàtiques a la interfície host-only
- Ús del disc secundari per a dades i logs
- Configuració de Windows Update per a actualitzacions manuals

---

## 🚀 Properes Etapes

Si aquest projecte continués, les següents accions serien:

1. **Instal·lar rols de servidor** (Active Directory, DNS, DHCP)
2. **Configurar polítiques de grup**
3. **Implementar mesures de seguretat** addicionals
4. **Crear usuaris i grups** del domini
5. **Provar la connectivitat** amb clients Windows

---

## 👥 Autoria

**Estudiant:** [Miquel vico]    
**Data de Realització:** 22 de Gener de 2026  
**Assignatura:** Sistemes Operatius en Xarxa

---

## 📄 Llicència

Aquesta documentació s'ha creat amb finalitats educatives dins del marc del cicle formatiu. Tot el contingut és original i basat en la pràctica realitzada.

---

*"Documentar no és només explicar el que has fet, sinó deixar el camí preparat per a qui vingui després."*
