# Projekt: Predykcja wskaźnika uzależnienia

## Spis treści

1. [Opis projektu](#opis-projektu)
2. [Struktura repozytorium](#struktura-repozytorium)
3. [Wymagania](#wymagania)
4. [Instalacja](#instalacja)
5. [Dane](#dane)
6. [Przygotowanie danych](#przygotowanie-danych)
7. [Modelowanie](#modelowanie)
8. [Ewaluacja i strojenie](#ewaluacja-i-strojenie)
9. [Wyniki](#wyniki)
10. [Wnioski i rekomendacje](#wnioski-i-rekomendacje)
11. [Wdrożenie](#wdrożenie)

---

## Opis projektu

Celem projektu jest zbudowanie modelu uczenia maszynowego, który na podstawie krótkiej ankiety przewidzi wskaźnik uzależnienia użytkownika. Projekt obejmuje całą ścieżkę Data Science: od pozyskania i przygotowania danych po strojenie modelu oraz rekomendacje wdrożeniowe.

## Struktura repozytorium

```
├── data/                  # Dane surowe i przetworzone
├── notebooks/             # Notebooki eksploracyjne i testowe
├── src/                   # Kod źródłowy (preprocessing, modele, ewaluacja)
├── reports/               # Wyniki, wykresy, raporty
├── models/                # Zapisane modele i parametry
├── requirements.txt       # Wymagania Pythona
└── README.md              # Ten plik
```

## Wymagania

* Python 3.8+
* biblioteki (zainstaluj przez `pip install -r requirements.txt`):

  * pandas, numpy, scikit-learn, matplotlib, seaborn

## Instalacja

1. Sklonuj repozytorium:

   ```bash
   git clone https://github.com/twoje-repozytorium/predykcja-uzaleznienia.git
   cd predykcja-uzaleznienia
   ```
2. Utwórz i aktywuj środowisko wirtualne:

   ```bash
   python -m venv venv
   source venv/bin/activate   # Linux/Mac
   venv\Scripts\activate    # Windows
   ```
3. Zainstaluj zależności:

   ```bash
   pip install -r requirements.txt
   ```

## Dane

* Źródło: https://www.kaggle.com/datasets/adilshamim8/social-media-addiction-vs-relationships
* Plik w `dataset/`

## Przygotowanie danych

* usuwanie niepotrzebnych kolumn
* zmiana wartości kategorycznych na numeryczne  - LabelEncoder
* stworzenie nowej kolumny z Country na Region z powodu rozległej ankiety na świecie
* usuwanie przypadków odstających, jeśli takie istnieją
* transformacja wartości niektórych kolumn na binarne - One Hot Encoding

## Modelowanie

Wykorzystane algorytmy:

* Linear Regression
* Ridge i Lasso Regression
* Random Forest Regressor
* Gradient Boosting Regressor
* Support Vector Regressor

Proces:

1. Wstępna ocena modeli przy użyciu 5-fold CV (metryki: R², RMSE, MAE, Explained Variance)
2. Porównanie wyników i wybór obiecujących kandydatów

## Ewaluacja i strojenie

* Dla najlepszego kandydata (Random Forest) wykonano:

  * Strojenie hiperparametrów (`GridSearchCV`, 5-fold CV, 432 konfiguracje)
  * Nested CV lub oddzielny podział na train/test
* Optymalne parametry:

  ```json
  {
    "n_estimators": 200,
    "max_depth": null,
    "max_features": 0.5,
    "min_samples_split": 2,
    "min_samples_leaf": 1
  }
  ```
* Walidacja:

  * negatywny RMSE (CV): -0.1847
  * Testowe: R² = 0.9859, RMSE = 0.1880

## Wyniki

* Najlepszy model osiąga błąd średni 0.19 pkt przy R² ≈ 0.986
* Główne czynniki wpływające na wynik zidentyfikowano poprzez analizę istotności cech oraz korelacji

## Wnioski i rekomendacje

* Model przewiduje wskaźnik uzależnienia z wysoką dokładnością (średni błąd 0.19 pkt)
* Krótka ankieta jest wystarczająca do szybkiej identyfikacji osób z podwyższonym ryzykiem
* Możliwość stworzenia aplikacji rekomendującej bezpieczny poziom korzystania z treści

## Wdrożenie

* Propozycja: udostępnić model jako usługę REST API do codziennej analizy wsadowej
* System alertów przy przekroczeniu progowego ryzyka