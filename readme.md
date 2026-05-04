# **Projekt zaliczeniowy - Zastosowania Uczenia Maszynowego**

## **1. Informacje ogólne**
**Nazwa projektu:**
Rozpoznawanie emocji w głosie

**Autor:**
Karol Dudzic

**Kierunek, rok i tryb studiów:**
Informatyka, semestr III, studia II stopnia z wykorzystaniem metod i technik kształcenia na odległość

**Data oddania projektu:**
04.05.2026

---

## **2. Opis projektu**
Celem projektu jest automatyczne rozpoznawanie emocji na podstawie nagrań mowy. Na podstawie cech akustycznych sygnału audio - takich jak ton (pitch), tempo mówienia, energia sygnału oraz mel-spektrogram - model klasyfikuje emocje mówiącego do jednej z kategorii: radość, smutek, złość lub neutralność.

Projekt może znaleźć zastosowanie m.in. w systemach call center do analizy nastrojów klientów, w asystentach głosowych, w psychologii klinicznej do monitorowania stanu emocjonalnego pacjentów, a także w grach i aplikacjach interaktywnych reagujących na nastrój użytkownika.

---

## **3. Dane**

**Źródło danych:**  
RAVDESS *(Ryerson Audio-Visual Database of Emotional Speech and Song)*

**Link do pobrania pełnego zbioru:**  
`https://zenodo.org/record/1188976` - plik `Audio_Speech_Actors_01-24.zip` (~200 MB)

**Opis zbioru:**

| Cecha | Wartość |
|-------|---------|
| Liczba nagrań (speech) | 1440 plików `.wav` |
| Liczba aktorów | 24 (12 kobiet, 12 mężczyzn) |
| Liczba klas emocji | 8 |
| Poziomy intensywności | normal, strong |
| Częstotliwość próbkowania | 48 000 Hz (surowe) |
| PCM | 16 bit |
| Czas trwania nagrania | ~3-4 s |
| Licencja | CC BY-NC-SA 4.0 |

**Klasy emocji (kod - etykieta):**

| Kod | Emocja | Liczba próbek |
|-----|--------|---------------|
| 01 | neutral (neutralna) | 96 |
| 02 | calm (spokojna) | 96 |
| 03 | happy (radosna) | 192 |
| 04 | sad (smutna) | 192 |
| 05 | angry (zła) | 192 |
| 06 | fearful (przestraszona) | 192 |
| 07 | disgust (odraza) | 192 |
| 08 | surprised (zaskoczona) | 192 |

> **Uwaga:** klasy *neutral* i *calm* mają mniej próbek, ponieważ dla tych emocji nagrano wyłącznie normalną intensywność (brak wersji z mocną intensywnością).

**Schemat nazewnictwa plików:**  
`MM-VV-EE-II-SS-RR-AA.wav`  
np. `03-01-05-02-01-01-12.wav` - audio-only, mowa, zły, silna intensywność, zdanie 1, powtórzenie 1, aktor 12 (kobieta)

| Pozycja | Opis |
|---------|------|
| MM=03 | Modalność: audio-only |
| VV=01 | Kanał: mowa |
| EE | Emocja (01-08) |
| II | Intensywność: 01=normalna, 02=mocna |
| SS | Zdanie: 01="Kids are talking by the door", 02="Dogs are sitting by the door" |
| RR | Powtórzenie: 01 lub 02 |
| AA | Numer aktora: 01-24 (nieparzyste=mężczyźni, parzyste=kobiety) |

**Próbka danych - RAVDESS:**  
Folder `data/sample/ravdess/` zawiera kilka przykładowych nagrań ze zbioru

**Instrukcja pobrania pełnego zbioru RAVDESS:**

---

### CREMA-D *(Crowd-Sourced Emotional Multimodal Actors Dataset)*

**Link do pobrania:** `https://github.com/CheyneyComputerScience/CREMA-D`

| Cecha | Wartość |
|-------|---------|
| Liczba nagrań | 7442 plików `.wav` |
| Liczba aktorów | 91 (48 mężczyzn, 43 kobiety) |
| Liczba klas emocji | 6 |
| Poziomy intensywności | LO, MD, HI, XX |
| Częstotliwość próbkowania | 16 000 Hz (surowe) |
| PCM | 16 bit |
| Liczba zdań | 12 |
| Licencja | Open Database License (ODbL) |

**Klasy emocji (kod - etykieta):**

| Kod | Emocja |
|-----|--------|
| ANG | angry (zły) |
| DIS | disgust (odraza) |
| FEA | fearful (przestraszony) |
| HAP | happy (radosny) |
| NEU | neutral (neutralny) |
| SAD | sad (smutny) |

**Schemat nazewnictwa plików:**  
`ActorID_SentenceID_Emotion_Level.wav`  
np. `1001_DFA_ANG_XX.wav` - Aktor 1001, zdanie "Don't forget a jacket", zły, poziom nieoznaczony

**Próbka danych - CREMA-D:**  
Folder `data/sample/cremad/` zawiera kilka przykładowych nagrań

---

## **4. Cel projektu**
Model odpowiada na pytanie jaką emocję wyraża mówca w danym fragmencie nagrania.
Klasyfikacja nagrań mowy do jednej z 6 klas emocjonalnych (złość, niesmak, strach, radość, smutek, neutralność).

---

## **5. Struktura projektu**
Projekt składa się z czterech głównych etapów, każdy w osobnym notatniku `.ipynb`:

| Etap | Nazwa pliku | Opis |
|------|--------------|------|
| 1 | `1_EDA.ipynb` | Analiza danych audio, wizualizacje sygnałów, rozkład klas emocji |
| 2 | `2_Preprocessing_Features.ipynb` | Ekstrakcja cech (MFCC, mel-spektrogram, pitch, energia), normalizacja |
| 3 | `3_Models_Training.ipynb` | Trening modeli SVM, CNN oraz modelu transformerowego HuBERT |
| 4 | `4_Evaluation.ipynb` | Ewaluacja, porównanie modeli, wizualizacje wyników |

---

## **6. Modele**
Projekt obejmuje trzy różne podejścia do modelowania danych:

### **6.1 Model klasyczny ML**
**Algorytm SVM** - **Klasyfikacja na podstawie wektorów cech akustycznych**
| Cecha | Wymiar | Uzasadnienie |
|-------|--------|------------- |
| MFCC (N=40): mean + std | 80 | Kształt spektrum - podstawowa cecha w SER |
| Δ MFCC: mean + std | 80 | Dynamika zmian spektrum |
| ΔΔ MFCC: mean + std | 80 | Przyspieszenie zmian - odzwierciedla akcent i rytm |
| ZCR: mean + std | 2 | Szorstkość głosu |
| RMS energia: mean + std | 2 | Głośność (złość/radość > smutek/neutralność) |
| Spectral centroid: mean + std | 2 | "Jasność" brzmienia |
| Spectral bandwidth: mean + std | 2 | Szerokość pasma |
| Spectral rolloff: mean + std | 2 | Granica częstotliwości 85% energii |
| Chroma STFT (12 bins): mean + std | 24 | Profil tonalny głosu |
| Spectral contrast (7 pasm): mean + std | 14 | Różnica szczyt/dolina widma - faktura dźwięku |
| **Razem** | **288** | |

### **6.2 Sieć neuronowa**
**Sieć CNN analizująca spektrogramy melowe**
Sieć o czterech warstwach konwolucyjnych. Struktura:
```
(batch, 1, 128, 94)
  ↓ Conv2d(1→f1, 3x3) + BN + ReLU + MaxPool(2)
  ↓ Conv2d(f1→f2, 3x3) + BN + ReLU + MaxPool(2)
  ↓ Conv2d(f2→f3, 3x3) + BN + ReLU + MaxPool(2)
  ↓ Conv2d(f3→f4, 3x3) + BN + ReLU + MaxPool(2)
  ↓ GlobalAveragePooling(dim=[H, W])
  ↓ Linear(f4 to 256) + ReLU + Dropout
  ↓ Linear(256 to 6)
```

### **6.3 Model transformerowy (fine-tuning)**
**Model transformerowy HuBERT**
Fine-tuning klasyfikatora emocji na wybranych zbiorach danych

---

## **7. Ewaluacja**
**Użyte metryki:**
accuracy, precision, recall, F1-score, macierz pomyłek

**Porównanie modeli:**

| Model | Accuracy | Macro Recall | Macro F1 | Czas inferencji (per próbka) |
|-------|----------|--------------|----------|-------------------------------|
| SVM | 52.8% | 52.8% | 52.4% | 0.88 ms |
| Sieć CNN | 62.0% | 62.5% | 61.8% | 0.36 ms |
| HuBERT Base | 73.5% | 74.2% | 73.3% | 3.99 ms |

**Uwagi:**
- Wszystkie modele testowano na tym samym zbiorze testowym (1318 próbek, 6 klas, podział stratyfikowany aktor-wise).
- Klasa *neutral* jest nieco mniej liczna (183 vs 227 próbek pozostałych klas), co widoczne jest w wyższym recall przy niższej precision dla tej klasy - szczególnie w modelu HuBERT (recall 94.5%, precision 67.6%).
- SVM osiąga najkrótszy czas inferencji pominąwszy narzut ładowania danych, jednak jego accuracy jest najniższe - różnica ~20 pp względem HuBERT.
- HuBERT pomimo najdłuższego czasu inferencji wyprzedza pozostałe modele o znaczący margines we wszystkich metrykach.=

*Wizualizacje* z ekstrakcji danych, treningu oraz ewaluacji do odnalezienia w katalogu results.

---

## **8. Wnioski i podsumowanie**

**Najlepszy model:** HuBERT Base z trafnością 73.5% i macro F1 73.3%. Przewaga wynika z pre-trainingu na dużym korpusie mowy - model rozumie strukturę akustyczną języka zanim zobaczy jakąkolwiek etykietę emocji. Fine-tuning pozwala wykorzystać tę wiedzę przy stosunkowo małym zbiorze treningowym.

**Trudności podczas pracy:**
- Scalenie zbiorów RAVDESS i CREMA-D wymagało ujednolicenia klas emocji. RAVDESS zawiera 8 klas, CREMA-D 6 - klasy *calm* i *surprised* obecne tylko w RAVDESS zostały porzucone ze względu na ich brak w drugim zbiorze, co spowodowało utratę części danych.
- Po scaleniu klasa *neutral* pozostała mniej liczna niż pozostałe (brak wersji z mocną intensywnością w RAVDESS), co wymusiło świadomy dobór metryk - samo accuracy mogłoby maskować słabość modelu na tej klasie.
- Czas treningu rośnie dramatycznie wraz ze złożonością modelu: SVM ~3 min, CNN ~kilkanaście minut, HuBERT ~1 godzina. Ma to bezpośrednie przełożenie na dobór hiperparametrów - grid search jest praktycznie wykluczony dla modeli transformerowych przy ograniczonych zasobach obliczeniowych. Parametry HuBERT dobierano ręcznie na podstawie wcześniejszych eksperymentów i literatury.

**Co można poprawić:**
- Augmentacja danych audio (pitch shift, dodawanie szumu, time stretch) została przetestowana, jednak nie przyniosła znaczącej poprawy wyników (1-2 pp).
- Dla SVM przetestowano zarówno jądro liniowe, jak i RBF - wyniki były porównywalne, a wybór jądra zależał od zestawu cech.
- HuBERT można by dalej fine-tunować z mniejszym learning rate lub zastosować większy wariant (HuBERT Large).
- Warto byłoby przeprowadzić dokładniejszy dobór hiperparametrów dla CNN i HuBERT - zwiększenie przestrzeni przeszukiwań i zastosowanie grid search mogłoby poprawić wyniki, jednak przy ograniczonych zasobach obliczeniowych jest to kosztowne czasowo.

Potencjalne zastosowania to automatyczna analiza emocji w obsłudze klienta, wsparcie diagnostyki psychologicznej czy interfejsy człowiek-komputer reagujące na nastrój użytkownika.

---

## **9. Struktura repozytorium**
```
zum/
│
├── data/
│   ├── sample/
│   │   └── ravdess/     # próbka RAVDESS
│   │   └── cremad/      # próbka CREMA-D
│   ├── ravdess/         # pełny zbiór RAVDESS - lokalnie (nie w repo)
│   ├── cremad/          # pełny zbiór CREMA-D - lokalnie (nie w repo)
├── notebooks/
│   ├── 1_EDA.ipynb                        # analiza eksploracyjna
│   ├── 2_Preprocessing_Features.ipynb     # ekstrakcja cech MFCC / mel
│   ├── 3_Models_Training.ipynb            # trening modeli
│   └── 4_Evaluation.ipynb                 # ewaluacja i porównanie
├── models/              # zapisane modele (.pkl, .pt, .h5)
├── results/             # wykresy generowane przez notebooki
├── README.md
└── requirements.txt
```

---

## **10. Technologia i biblioteki**
- Python 3.x
- NumPy, Pandas, Matplotlib, Seaborn
- librosa - ekstrakcja cech audio (MFCC, mel-spektrogram, pitch, energia)
- scikit-learn 
- PyTorch
- HuggingFace Transformers (HuBERT)

---

## **11. Licencja projektu**
Projekt udostępniony na licencji:
*(np. MIT License)*

Źródła danych: zgodnie z licencjami wskazanymi w sekcji **Dane** (RAVDESS - CC BY-NC-SA 4.0; CREMA-D - ODbL).
