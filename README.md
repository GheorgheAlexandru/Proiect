# PROIECT - CIRCUITE CU AMPLIFICATOARE OPERAȚIONALE

Colecție de circuite cu amplificatoare operaționale (OP482) integrate în sistem, de la amplificare până la detecție vârf.

---

## 🎯 DESCRIERE GENERALĂ

**Sistem integrat: Draft1 → Draft2 → Draft3 → Draft4**

Patru etaje cascadate care procesează și amplifică semnale mici, cu compensare frecvență și detecție finală de vârf.

---

## 📊 CIRCUITELE SISTEMULUI

### **DRAFT1 - AMPLIFICATOR SUMATOR (3 Etaje)**

**Rolul**: Amplificare inițială cu inversie de fază

| Parametru | Valoare |
|-----------|---------|
| Componente | 3× OP482, rezistori (6.8kΩ, 10kΩ) |
| Alimentare | ±5V |
| Guadagn | -1.47 per etaj |
| Guadagn Total | **-3.18 V/V (10.05 dB)** |
| Impedanța intrare | 10kΩ |

**Formula Guadagn:**
```
G = -Rf/Rin = -10kΩ/6.8kΩ ≈ -1.47
```

---

### **DRAFT2 - AMPLIFICATOR CASCADAT (3 Etaje non-invertoare)**

**Rolul**: Amplificare secundară cu stabilitate frecvență

| Parametru | Valoare |
|-----------|---------|
| Componente | 3× OP482, rezistori (2kΩ), condensatori (18.8nF, 21.1nF) |
| Alimentare | ±5V |
| Guadagn per etaj | 2 V/V |
| Guadagn Total | **8 V/V (18.06 dB)** |
| Lățime bandă -3dB | **3.77 MHz** |
| Intrare | ← OUT Draft1 |

**Formula Guadagn Non-Invertor:**
```
G = 1 + Rf/Rg = 1 + 2kΩ/2kΩ = 2
```

**Frecvența de Tăiere:**
```
fc = 1/(2πRC) = 1/(2π × 2kΩ × 18.8nF) ≈ 4.24 MHz
```

---

### **DRAFT3 - AMPLIFICATOR INVERTOR VARIABIL**

**Rolul**: Ajustare parametrică guadagn cu toleranță

| Parametru | Valoare |
|-----------|---------|
| Componente | 1× OP482, R1=10kΩ, R2=variabil |
| Alimentare | ±5V |
| R2 Range | 11.2kΩ → 22.39kΩ |
| Guadagn Min | -1.12 V/V (0.98 dB) |
| Guadagn Max | -2.24 V/V (7.01 dB) |
| Intrare | ← OUT Draft2 |

**Tabel Parametric:**

| R2 | Guadagn | Tip |
|----|---------|-----|
| 11.2kΩ | -1.12 | Atenuare |
| 14.3kΩ | -1.43 | **Optimal** |
| 17.7kΩ | -1.77 | Amplificare |
| 22.39kΩ | -2.24 | Max |

---

### **DRAFT4 - DETECTOR DE VÂRF (Diodă Ideală)**

**Rolul**: Detecție și ținere valoare vârf

| Parametru | Valoare |
|-----------|---------|
| Componente | 2× OP482, 2 diode, C=470pF, R=330kΩ |
| Alimentare | ±15V |
| Topologie | Perfect Diode (compensare cădere) |
| Timp constantă | τ = RC = 155 ns |
| Guadagn | 1 V/V (unity buffer) |
| Cădere diodă | **~0V** (vs 0.7V standard) |
| Intrare | ← OUT Draft3 |

---

## 🔗 FLUXUL COMPLET AL SISTEMULUI

```
INPUT (1mV)
   ↓
┌─────────────────────────────────────────────────────────────┐
│ DRAFT1: Amplificator Sumator                                │
│ • 3 etaje invertoare                                        │
│ • Guadagn: -3.18                                            │
│ • Output: ±3.18 mV                                          │
└─────────────────────────────────────────────────────────────┘
   ↓ (impedanță interfață: Zin = 1MΩ)
┌─────────────────────────────────────────────────────────────┐
│ DRAFT2: Amplificator Cascadat cu Compensare                 │
│ • 3 etaje non-invertoare                                    │
│ • Guadagn: 8                                                │
│ • BW: 3.77 MHz                                              │
│ • Output: ±25.4 mV                                          │
└─────────────────────────────────────────────────────────────┘
   ↓ (impedanță interfață: Zin = 10kΩ)
┌─────────────────────────────────────────────────────────────┐
│ DRAFT3: Amplificator Invertor Variabil                      │
│ • Selectabil: 11.2kΩ ... 22.39kΩ                           │
│ • Guadagn: -1.12 la -2.24                                   │
│ • Output: ±28.5 mV (max)                                    │
└─────────────────────────────────────────────────────────────┘
   ↓ (impedanță interfață: Zin = 1MΩ)
┌─────────────────────────────────────────────────────────────┐
│ DRAFT4: Detector Vârf - Diodă Ideală                        │
│ • Detecție valoare pic                                      │
│ • Ținere semnal (capacitor)                                 │
│ • Output stabil: Vârf detectat ≈ 28.5 mV                   │
└─────────────────────────────────────────────────────────────┘
   ↓
OUTPUT FINAL (Semnal vârf stabil)
```

---

## 📐 FORMULE PRINCIPIALE

### Amplificator Invertor
```
Vout = -(Rf/Rin) × Vin
Zin = Rin
```

### Amplificator Non-Invertor
```
Vout = (1 + Rf/Rg) × Vin
Zin ≈ > 1 MΩ
```

### Filtru RC Trece-Jos
```
fc = 1/(2πRC)
|H(f)| = 1/√(1 + (f/fc)²)
```

### Produsul Guadagn × Lățime de Bandă
```
GBW = |Guadagn| × BW = 5 MHz (OP482)
```

---

## 🔌 CONEXIUNI FIZICE

**Draft1 → Draft2:**
```
OUT (Draft1) ──[1MΩ impedanță]──→ INPUT Draft2
```

**Draft2 → Draft3:**
```
OUT (Draft2) ──[10kΩ impedanță]──→ INPUT Draft3
```

**Draft3 → Draft4:**
```
OUT (Draft3) ──[1MΩ impedanță]──→ INPUT Draft4
```

### Alimentare
- **Draft1, Draft2, Draft3**: ±5V (sursa comună)
- **Draft4**: ±15V (sursa separată)

### Decuplare (fiecare op-amp)
```
100nF ceramic (vcc, gnd)
10µF electrolitic (vcc, gnd)
```

---

## 📈 PERFORMANȚĂ GLOBALĂ

**Semnal de intrare: 1mV AC @ 1kHz**

| Etapă | Intrare | Ieșire | Guadagn |
|-------|---------|--------|---------|
| Draft1 | 1mV | ±3.18mV | -3.18 |
| Draft2 | 3.18mV | ±25.4mV | 8 |
| Draft3 | 25.4mV | ±28.5mV | -1.12..−2.24 |
| Draft4 | 28.5mV | **Vârf: 28.5mV** | 1 |

**Guadagn Total Cascadat:**
```
G_total = (-3.18) × 8 × (-1.77) × 1 ≈ 44.9 (33.05 dB)
```

---

## 📋 FIȘIERE PROIECT

```
Proiect/
├── Draft1.asc      ← Amplificator Sumator (3-etaje, ±5V)
├── Draft2.asc      ← Amplificator Cascadat cu Compensare
├── Draft3.asc      ← Analiză Parametrică Guadagn
├── Draft4.asc      ← Detector Vârf cu Diodă Ideală
├── Proiect.pdf     ← Documentație completă
├── README.md       ← Documentație Engleză
└── README_RO.md    ← Documentație Română (ACEST FIȘIER)
```
├── README.md       ← Documentație 


---

## ⚙️ SIMULARE ÎN LTSpice

### Deschidere circuit
```
File → Open → Selectează fișier .asc
```

### Rulare simulare
```
Simulate → Run  (sau Ctrl+R)
```

### Vizualizare rezultate
- **Forme de undă**: tensiune vs. timp
- **Analiză FFT**: domeniu frecvență
- **Măsurători**: instrumente cursor

### Modificare parametri
```
Click dreapta pe componentă → Edit → Schimbă valori
```

---

## 🎯 CARACTERISTICI OP482

| Parametru | Valoare |
|-----------|---------|
| GBW | 5 MHz |
| Slew Rate | 13 V/µs |
| Tensiune offset | < 2 mV |
| Curent bias | 60 pA |
| CMRR | > 80 dB |
| Alimentare | ±5V la ±18V |

---

## ✅ REZULTATE AȘTEPTATE

✓ **Draft1**: Amplificare cu inversie  
✓ **Draft2**: Amplificare mare + stabilitate frecvență  
✓ **Draft3**: Ajustare guadagn selector  
✓ **Draft4**: Detecție vârf semnal final  

✓ **Sistem complet**: Procesare semnal mic → Output stabil

---

## 👤 Autor

**GheorgheAlexandru**

Data: **Ianuarie 2026**

Licență: **Utilizare educațională și profesională**

---

**Note**: Simulări presupun condiții ideale. Implementarea hardware necesită considerații termice și protecție intrare.
