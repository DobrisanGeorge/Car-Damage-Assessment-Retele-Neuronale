**Disciplina:** Rețele Neuronale  
**Instituție:** POLITEHNICA București – FIIR  
**Student:** Dobrisan Andrei George
**Data:** 2025.11.20  

----------------------------------------------------------------------------------------------------------------------------------

## Introducere

Acest document descrie activitățile realizate în **Etapa 3**, în care se analizează și se preprocesează setul de date necesar proiectului „AutoDataAI". Scopul etapei este pregătirea corectă a datelor pentru instruirea modelului RN, respectând bunele practici privind calitatea, consistența și reproductibilitatea datelor.

----------------------------------------------------------------------------------------------------------------------------------

##  1. Structura Repository-ului Github (versiunea Etapei 3)

```
AutoDataAI/
├── README.md
├── docs/
│   └── datasets/          # descriere seturi de date, surse, diagrame
├── data/
│   ├── raw/               # date brute
│   ├── processed/         # date curățate și transformate
│   ├── train/             # set de instruire
│   ├── validation/        # set de validare
│   └── test/              # set de testare
├── src/
│   ├── preprocessing/     # funcții pentru preprocesare
│   ├── data_acquisition/  # generare / achiziție date (dacă există)
│   └── neural_network/    # implementarea RN (în etapa următoare)
├── config/                # fișiere de configurare
└── requirements.txt       # dependențe Python (dacă aplicabil)
```

----------------------------------------------------------------------------------------------------------------------------------

##  2. Descrierea Setului de Date

### 2.1 Sursa datelor

* **Origine:** Set de imagini propriu (colectare manuală/simulată)
* **Modul de achiziție:** Senzori reali (cameră smartphone) / Fișier extern
* **Perioada / condițiile colectării:** Colectare în Noiembrie 2025; Imagini sub diverse condiții de lumină, unghiuri și claritate, pentru a simula utilizarea reală.

-------------------------------------------------------------------------------------------------------------------------------------

### 2.2 Caracteristicile dataset-ului

* **Număr total de observații:** $\approx 150-200 Imagini (fotografii de talon).
* **Număr de caracteristici (features):** aprox 10
* **Tipuri de date:**  Imagini
* **Format fișiere:**  PNG / JPEG

----------------------------------------------------------------------------------------------------------------------------------

### 2.3 Descrierea fiecărei caracteristici

| Caracteristică (Clasă)    | Descriere                                             | Domeniu valori                      |
|---------------------------|-------------------------------------------------------|-------------------------------------|
| E_VIN                     | Seria de Șasiu (Vehicle Identification Number)        | Alfanumeric (17 caractere)          |
| A_NR_INMATRICULARE        | Numărul de înmatriculare                              | Alfanumeric                         |
| B_DATA_INMATRICULARE      | Data primei înmatriculări                             | Dată (ZZ.LL.AAAA)                   |
| D1_MARCA                  | Marca vehiculului                                     | Text                                |
| D3_DENUMIRE_COMERCIALA    | Denumirea comercială (Modelul)                        | Text                                |
| C1_NUME                   | Numele deținătorului (Proprietar/Utilizator)          | Text                                |
| C1_PRENUME                | Prenumele deținătorului                               | Text                                |
| C1_ADRESA                 | Adresa de reședință a deținătorului                   | Alfanumeric                         |
| F1_MASA_MAXIMA            | Masa maximă totală autorizată                         | Numeric (kg)                        |
| G_MASA_NEINCARCATA        | Masa proprie a vehiculului (neîncărcat)               | Numeric (kg)                        |
| P1_CAPACITATE_CIL.        | Capacitatea cilindrică a motorului                    | Numeric (cm³)                       |
| P2_PUTERE                 | Puterea netă maximă                                   | Numeric (kW)                        |
| P3_TIP_COMBUSTIBIL        | Tipul de combustibil                                  | Text/Literal                        |
| V9_NORMA_POLUARE          | Norma de poluare (ex: Euro 6)                         | Text/Literal                        |
| S1_NUMAR_LOCURI           | Numărul de locuri pe scaune                           | Numeric                             |
| X.1_DATA_ITP              | Data de expirare a ITP                                | Dată (ZZ.LL.AAAA)                   |
| H_PERIOADA_VALIDITATE     | Perioada de valabilitate a înmatriculării             | Dată (ZZ.LL.AAAA)                   |
| J_CATEGORIA_CE            | Categoria vehiculului conform legislației CE          | Text/Literal                        |
| K_NR_OMOLOGARE            | Numărul omologării de tip                             | Alfanumeric                         |
| I_DATA_INMATR_UE          | Data înmatriculării în UE                             | Dată (ZZ.LL.AAAA)                   |
| Q_RAPORT_PUTERE_MASA      | Raport putere/masă                                    | Numeric                             |
| T_VITEZA_MAXIMA           | Viteza maximă                                         | Numeric                             |
| U1/U2_NIVEL_SONOR         | Nivelul sonor (staționare / mers)                     | Numeric                             |

**Tip:** Obiect
**Format:** Bounding Box

----------------------------------------------------------------------------------------------------------------------------------

**Fișier recomandat:**  `data/README.md`

----------------------------------------------------------------------------------------------------------------------------------

##  3. Analiza Exploratorie a Datelor (EDA) – Sintetic

### 3.1 Statistici descriptive aplicate

* **Medie, mediană, deviație standard**
* **Min–max și quartile**
* **Distribuții pe caracteristici** (histograme)
* **Identificarea outlierilor** (IQR / percentile)

### 3.2 Analiza calității datelor

* **Detectarea valorilor lipsă** (% pe coloană)
* **Detectarea valorilor inconsistente sau eronate**
* **Identificarea caracteristicilor redundante sau puternic corelate**

### 3.3 Probleme identificate

* [exemplu] Feature X are 8% valori lipsă
* [exemplu] Distribuția feature Y este puternic neuniformă
* [exemplu] Variabilitate ridicată în clase (class imbalance)

---

##  4. Preprocesarea Datelor

### 4.1 Curățarea datelor

* **Eliminare duplicatelor**
* **Tratarea valorilor lipsă:**
  * Feature A: imputare cu mediană
  * Feature B: eliminare (30% valori lipsă)
* **Tratarea outlierilor:** IQR / limitare percentile

### 4.2 Transformarea caracteristicilor

* **Normalizare:** Min–Max / Standardizare
* **Encoding pentru variabile categoriale**
* **Ajustarea dezechilibrului de clasă** (dacă este cazul)
* **Augmentare Geometrică: Aplicarea de rotații, translații și zoom-uri aleatorii în timpul antrenării pentru a crește diversitatea setului de date.**
* **Augmentare Cromatică: Ajustarea dinamică a luminozității și contrastului (în Python/OpenCV) pentru a face modelul robust la diferite condiții de iluminare.**

### 4.3 Structurarea seturilor de date

**Împărțire recomandată:**
* 70–80% – train
* 10–15% – validation
* 10–15% – test

**Principii respectate:**
* Stratificare pentru clasificare
* Fără scurgere de informație (data leakage)
* Statistici calculate DOAR pe train și aplicate pe celelalte seturi
* Organizare YOLO: Seturile sunt organizate în foldere separate, conținând atât imaginile, cât și fișierele de adnotare (.txt).

### 4.4 Salvarea rezultatelor preprocesării

* Date preprocesate în `data/processed/`
* Seturi train/val/test în foldere dedicate
* Parametrii de preprocesare în `config/preprocessing_config.*` (opțional)

---

##  5. Fișiere Generate în Această Etapă

* `data/raw/` – date brute
* `data/processed/` – date curățate & transformate
* `data/train/`, `data/validation/`, `data/test/` – seturi finale
* `src/preprocessing/` – codul de preprocesare
* `data/README.md` – descrierea dataset-ului

---

##  6. Stare Etapă (de completat de student)

- [ ] Structură repository configurată
- [ ] Dataset analizat (EDA realizată)
- [ ] Date preprocesate
- [ ] Seturi train/val/test generate
- [ ] Documentație actualizată în README + `data/README.md`

---
