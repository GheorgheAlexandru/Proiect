# PROIECT - CIRCUITE CU AMPLIFICATOARE OPERAȚIONALE

Colecție de circuite cu amplificatoare operaționale (OP482) integrate în sistem, de la amplificare până la detecție vârf.

---

## 🎯 DESCRIERE GENERALĂ

**Sistem integrat: Draft1 → Draft2 → Draft3 → Draft4**

Patru etaje cascadate care procesează și amplifică semnale mici, cu compensare frecvență și detecție finală de vârf.

---

## 📊 CIRCUITELE SISTEMULUI

### **DRAFT1 -Amplificator de instrumentatie V-V cu 3 AO cu
intrari diferentiale**

**Rolul**: Amplificare inițială cu inversie de fază

| Parametru | Valoare |
|-----------|---------|
| Componente | 3× OP482, rezistori (6.8kΩ, 10kΩ) |
| Alimentare | ±5V |
| Impedanța intrare | 10kΩ |


---

### **DRAFT2 - Filtru trece jos cu 3 AO V-V KHN**

**Rolul**: Amplificare secundară cu stabilitate frecvență

| Parametru | Valoare |
|-----------|---------|
| Componente | 3× OP482, rezistori (2kΩ), condensatori (18.8nF, 21.1nF) |
| Alimentare | ±5V |
| Lățime bandă -3dB | **3.77 MHz** |
| Intrare | ← OUT Draft1 |


**Frecvența de Tăiere:**
```
fc = 1/(2πRC) = 1/(2π × 2kΩ × 18.8nF) ≈ 4.24 MHz
```

---

### **DRAFT3 - PGA , switch-uri in calea de semnal, conexiune
serie**

**Rolul**: Ajustare parametrică Gain cu toleranță

| Parametru | Valoare |
|-----------|---------|
| Componente | 1× OP482, R1=10kΩ, R2=variabil |
| Alimentare | ±5V |
| R2 Range | 11.2kΩ → 22.39kΩ |
| Gain Min | -1.12 V/V (0.98 dB) |
| Gain Max | -2.24 V/V (7.01 dB) |
| Intrare | ← OUT Draft2 |

**Tabel Parametric:**

| R2 | Guadagn | Tip |
|----|---------|-----|
| 11.2kΩ | -1.12 | Atenuare |
| 14.3kΩ | -1.43 | **Optimal** |
| 17.7kΩ | -1.77 | Amplificare |
| 22.39kΩ | -2.24 | Max |

---

### **DRAFT4 - REDRESOR DETECTOR DE VÂRF (Diodă Ideală)**

**Rolul**: Detecție și ținere valoare vârf

| Parametru | Valoare |
|-----------|---------|
| Componente | 2× OP482, 2 diode, C=470pF, R=330kΩ |
| Alimentare | ±15V |
| Topologie | Perfect Diode (compensare cădere) |
| Timp constantă | τ = RC = 155 ns |
| Cădere diodă | **~0V** (vs 0.7V standard) |
| Intrare | ← OUT Draft3 |

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

| Etapă | Intrare | Ieșire | 
|-------|---------|--------|
| Draft1 | 1mV | ±3.18mV   | 
| Draft2 | 3.18mV | ±25.4mV|
| Draft3 | 25.4mV | ±28.5mV|
| Draft4 | 28.5mV | **Vârf: 28.5mV** |




## 📋 FIȘIERE PROIECT

```
Proiect/
├── Draft1.asc      ← Amplificator Sumator (3-etaje, ±5V)
├── Draft2.asc      ← Amplificator Cascadat cu Compensare
├── Draft3.asc      ← Analiză Parametrică 
├── Draft4.asc      ← Detector Vârf cu Diodă Ideală
├── Proiect.pdf     ← Documentație completă
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

## 👤 Autor

**GheorgheAlexandru**

Data: **Ianuarie 2026**

Licență: **Utilizare educațională și profesională**

---

**Note**: Simulări presupun condiții ideale. Implementarea hardware necesită considerații termice și protecție intrare.
