# **Projekt zaliczeniowy - Zastosowania Uczenia Maszynowego**

## **1. Informacje ogólne**
**Nazwa projektu:**
Rozpoznawanie emocji w głosie

**Autor:**
Karol Dudzic

**Kierunek, rok i tryb studiów:**
Informatyka, semestr III, studia II stopnia z wykorzystaniem metod i technik kształcenia na odległość

**Data oddania projektu:**
dd.mm.rrrr

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
- **Co robi model?** Klasyfikuje nagranie mowy do jednej z klas emocjonalnych (np. radość, smutek, złość, neutralność).
- **Jakie pytanie odpowiada?** Jaką emocję wyraża mówca w danym fragmencie nagrania?
- **Zastosowania:** automatyczna analiza emocji w obsłudze klienta, wsparcie diagnostyki psychologicznej, interfejsy człowiek-komputer reagujące na nastrój użytkownika.

---

## **5. Struktura projektu**
Projekt składa się z czterech głównych etapów, każdy w osobnym notatniku `.ipynb`:

| Etap | Nazwa pliku | Opis |
|------|--------------|------|
| 1 | `1_EDA.ipynb` | Analiza danych audio, wizualizacje sygnałów, rozkład klas emocji |
| 2 | `2_Preprocessing_Features.ipynb` | Ekstrakcja cech (MFCC, mel-spektrogram, pitch, energia), normalizacja |
| 3 | `3_Models_Training.ipynb` | Trening modeli klasycznego ML, sieci neuronowej i modelu transformerowego |
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

- Wyniki / metryki: *(do uzupełnienia)*

### **6.2 Sieć neuronowa**
**Sieć CNN analizująca spektrogramy melowe**

- Liczba warstw / neuronów: *(do uzupełnienia)*
- Funkcje aktywacji i optymalizator: *(do uzupełnienia)*
- Wyniki: *(do uzupełnienia)*

### **6.3 Model transformerowy (fine-tuning)**
**Model transformerowy HuBERT**
- Zakres dostosowania: fine-tuning klasyfikatora emocji na wybranych zbiorach danych
- Wyniki: *(do uzupełnienia)*

---

## **7. Ewaluacja**
**Użyte metryki:**
accuracy, precision, recall, F1-score (macro/weighted), macierz pomyłek

**Porównanie modeli:**

| Model | Metryka główna | Wynik | Uwagi |
|--------|----------------|--------|--------|
| Klasyczny ML |  |  |  |
| Sieć neuronowa |  |  |  |
| Transformer |  |  |  |

**Wizualizacje:**
- Macierz pomyłek dla każdego modelu
- Rozkład klas emocji w zbiorze danych
- Wizualizacje mel-spektrogramów dla różnych emocji
- Learning curve (krzywa uczenia)

---

## **8. Wnioski i podsumowanie**
- Który model okazał się najlepszy i dlaczego?
- Jakie trudności pojawiły się podczas pracy? *(np. niezbalansowane klasy, różnice między zbiorami danych, szumy w nagraniach)*
- Co można by poprawić w przyszłości? *(np. dane multimodalne: audio + tekst, więcej klas emocji)*
- Jakie potencjalne zastosowania ma ten projekt?

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
- TensorFlow lub PyTorch
- HuggingFace Transformers (wav2vec 2.0 / HuBERT)

---

## **11. Licencja projektu**
Projekt udostępniony na licencji:
*(np. MIT License)*

Źródła danych: zgodnie z licencjami wskazanymi w sekcji **Dane** (RAVDESS - CC BY-NC-SA 4.0; CREMA-D - ODbL).
