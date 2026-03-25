# **Projekt zaliczeniowy – Zastosowania Uczenia Maszynowego**

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
Celem projektu jest automatyczne rozpoznawanie emocji na podstawie nagrań mowy. Na podstawie cech akustycznych sygnału audio – takich jak ton (pitch), tempo mówienia, energia sygnału oraz mel-spektrogram – model klasyfikuje emocje mówiącego do jednej z kategorii: radość, smutek, złość lub neutralność.

Projekt może znaleźć zastosowanie m.in. w systemach call center do analizy nastrojów klientów, w asystentach głosowych, w psychologii klinicznej do monitorowania stanu emocjonalnego pacjentów, a także w grach i aplikacjach interaktywnych reagujących na nastrój użytkownika.

---

## **3. Dane**
**Źródło danych:**
RAVDESS, CREMA-D

**Linki do danych:**
- RAVDESS: `https://zenodo.org/record/1188976`
- CREMA-D: `https://github.com/CheyneyComputerScience/CREMA-D`

**Opis danych:**
- format danych: pliki audio (.wav)
- rodzaj etykiet / klas: emocje (radość, smutek, złość, neutralność i inne, zależnie od zbioru)
- RAVDESS: ~7300 nagrań, 24 aktorów, 8 emocji
- CREMA-D: ~7400 nagrań, 91 aktorów, 6 emocji
- licencja: RAVDESS – CC BY-NC-SA 4.0; CREMA-D – Open Database License

**Uwaga dotycząca danych:**
Nie wrzucaj do repozytorium pełnego zbioru danych!
- Zamieść **jedynie niewielką próbkę** (np. kilka plików `.wav`) w folderze `data/sample/`, aby pokazać strukturę danych.
- Pełen zbiór powinien być pobierany osobno zgodnie z instrukcją w sekcji poniżej lub przez link w README.
- Dzięki temu repozytorium pozostanie czyste i zgodne z zasadami licencyjnymi.

**Uwagi dotyczące preprocessingu:**
*(np. ujednolicenie częstotliwości próbkowania do 22050 Hz, przycięcie ciszy, augmentacja danych, itp.)*

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
- Algorytm: *(np. SVM, Random Forest, Gradient Boosting)*
- Krótki opis: klasyfikacja na podstawie wektorów cech akustycznych (MFCC, energia, pitch)
- Wyniki / metryki: *(do uzupełnienia)*

### **6.2 Sieć neuronowa zbudowana od zera**
- Architektura: *(np. CNN na mel-spektrogramach, LSTM na sekwencjach MFCC)*
- Liczba warstw / neuronów: *(do uzupełnienia)*
- Funkcje aktywacji i optymalizator: *(do uzupełnienia)*
- Wyniki: *(do uzupełnienia)*

### **6.3 Model transformerowy (fine-tuning)**
- Nazwa modelu: *(np. wav2vec 2.0, HuBERT, Whisper)*
- Biblioteka: HuggingFace Transformers
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
projekt_zum_2025/
│
├── data/
│   ├── sample/          # mała próbka nagrań audio
│   └── processed/       # wektory cech / spektrogramy
├── notebooks/
│   ├── 1_EDA.ipynb
│   ├── 2_Preprocessing_Features.ipynb
│   ├── 3_Models_Training.ipynb
│   └── 4_Evaluation.ipynb
├── models/              # zapisane modele (.pkl, .pt, .h5)
├── results/             # wykresy, macierze pomyłek
├── README.md
└── requirements.txt
```

---

## **10. Technologia i biblioteki**
- Python 3.x
- NumPy, Pandas, Matplotlib, Seaborn
- librosa – ekstrakcja cech audio (MFCC, mel-spektrogram, pitch, energia)
- scikit-learn
- TensorFlow lub PyTorch
- HuggingFace Transformers (wav2vec 2.0 / HuBERT)

---

## **11. Licencja projektu**
Projekt udostępniony na licencji:
*(np. MIT License)*

Źródła danych: zgodnie z licencjami wskazanymi w sekcji **Dane** (RAVDESS – CC BY-NC-SA 4.0; CREMA-D – ODbL).
