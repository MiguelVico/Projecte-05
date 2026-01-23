# Tasca T08: Seguretat – Protegint-nos contra el malware

Aquest repositori conté la documentació i guia pràctica de la tasca T08 de l'assignatura de Seguretat Informàtica, on hem analitzat mecanismes de protecció contra malware, simulat atacs de ransomware i treballat amb eines reals de detecció d'amenaces.

---

## 📌 Descripció de la tasca

En aquesta tasca hem explorat com protegir un sistema Windows davant amenaces de malware, des de la configuració bàsica de seguretat fins a la simulació d'atacs controlats i l'anàlisi de ransomware real com WannaCry. L'objectiu és desenvolupar competències per identificar riscos, aplicar mesures de protecció i entendre com actuen les amenaces informàtiques en un entorn professional.

---

## 🎯 Objectius

- Identificar els riscos associats al malware
- Conèixer eines i configuracions per protegir i recuperar sistemes
- Simular atacs de ransomware en entorns controlats
- Analitzar malware real amb eines de Threat Intelligence
- Aplicar protocols de seguretat activa i plans de contingència

---

## 🛠️ Continguts treballats

### 1. Configuració inicial de seguretat
- Revisió de les opcions de seguretat de Microsoft Edge
- Configuració de Windows Defender i protecció en temps real
- Polítiques d'execució de PowerShell

### 2. Prova amb fitxers EICAR
- Descarrega i anàlisi de fitxers de prova estàndard
- Comportament del SmartScreen i antivirus
- Detecció de fitxers comprimits maliciosos

### 3. Simulació de ransomware
- Execució del script `PSRansom.ps1`
- Xifrat de fitxers locals amb AES-256
- Anàlisi de l'impacte en dades reals

### 4. Anàlisi de malware real
- Treball amb el repositori **theZoo** de GitHub
- Descompressió i anàlisi de WannaCry
- Ús de portals de Threat Intelligence (VirusTotal, Kaspersky)

### 5. Entorn de proves segur
- Configuració de màquines virtuals aïllades
- Ús d'instantànies per restauració ràpida
- Bones pràctiques per a anàlisi de malware

---

## 📁 Estructura del projecte

```
tasca08/
├── img_T08/                    # Captures de pantalla
│   ├── captura1.png
│   ├── captura2.png
│   ├── ...
│   └── captura34.png
├── README.md                   # Aquest arxiu
├── guia_tasca08.md            # Guia completa amb anàlisi detallat
└── recursos/                  # Enllaços i documentació addicional
```

---

## 🔍 Eines utilitzades

- **Sistema operatiu**: Windows 11 Enterprise
- **Antivirus**: Microsoft Defender
- **Navegador**: Microsoft Edge amb SmartScreen
- **Compressió**: WinRAR 7.13
- **Entorn virtual**: Oracle VirtualBox / VMware
- **Anàlisi de malware**: VirusTotal, Kaspersky Threat Intelligence Portal
- **Scripts**: PowerShell per a simulació de ransomware
- **Repositori de malware**: theZoo (GitHub)

---

## 📋 Conclusions principals

✅ **La prevenció és clau**: Configuracions bàsiques com la protecció en temps real poden evitar la majoria d'atacs  
✅ **L'educació de l'usuari** és tan important com les eines tècniques  
✅ **Els entorns virtuals són indispensables** per a proves de seguretat  
✅ **Les còpies de seguregia regulars** són la millor defensa contra ransomware  
⚠️ **Cap sistema és 100% segur**, cal una aproximació en capes (defensa en profunditat)  

---

## 🧠 Competències desenvolupades

- **RA3**: Aplicar mecanismes de seguretat activa descrivint-ne les característiques
- **CA3.1**: Seguir plans de contingència davant fallades de seguretat
- **CA3.2**: Classificar els principals tipus de programari maliciós
- **CA3.5**: Instal·lar, provar i actualitzar aplicacions per a detecció i eliminació de malware

---

## 📚 Materials de referència

- [EICAR Anti-Malware Testfile](https://www.eicar.org/download-anti-malware-testfile/)
- [theZoo - Repository of LIVE malwares](https://github.com/ytisf/theZoo)
- [VirusTotal - Online malware scanner](https://www.virustotal.com/)
- [Microsoft Security Documentation](https://docs.microsoft.com/security/)

---

## 👥 Autoria

Aquesta tasca ha estat desenvolupada com a part de la formació en Seguretat Informàtica, seguint les directrius del mòdul professional. Les proves s'han realitzat en entorns controlats i virtualitzats per garantir la seguretat.

---

## ⚠️ Avís de seguretat

**ATENCIÓ**: Els procediments descrits en aquesta tasca inclouen la manipulació de malware real i simulacions d'atacs. S'han de realitzar **ÚNICAMENT** en entorns aïllats i controlats (màquines virtuals sense connexió a xarxa real). No executeu aquestes proves en equips de producció o amb dades reals sense les precaucions adequades.

---

## 📄 Llicència

Aquest material s'ha creat amb finalitats educatives. Pots utilitzar-lo i modificar-lo sempre que es reconegui l'autoria original.

---

**✨ "La seguretat no és un producte, sinó un procés."**  
*Bruce Schneier*
