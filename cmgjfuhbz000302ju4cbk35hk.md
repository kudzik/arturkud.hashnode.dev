---
title: "🧠 NeuroNote-vibe: Jak stworzyć własny system transkrypcji i tłumaczenia audio z AI"
seoTitle: "Jak Zautomatyzować Transkrypcję i Tłumaczenie Audio z Whisper i Huggin"
seoDescription: " Naucz się, jak stworzyć skrypt w Pythonie do automatycznej transkrypcji i tłumaczenia nagrań audio. Użyj modeli AI takich jak OpenAI Whisper i Hugging Face"
datePublished: Thu Oct 09 2025 13:13:21 GMT+0000 (Coordinated Universal Time)
cuid: cmgjfuhbz000302ju4cbk35hk
slug: neuronote-vibe-jak-stworzyc-wlasny-system-transkrypcji-i-tlumaczenia-audio-z-ai
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/ylveRpZ8L1s/upload/18d83bc9a74d2da88d3f14d286a6b110.jpeg
tags: tutorial, ai, python, tutorials, machine-learning, nlp, translation, example, huggingface, transcription, whisper, audio-processing, automatization, poradnik

---

*Czy kiedykolwiek marzyłeś o tym, żeby automatycznie transkrybować spotkania i tłumaczyć je na swój język? W tym artykule pokażę Ci, jak stworzyć własny system AI do transkrypcji i tłumaczenia audio używając Python i modeli Hugging Face.*

## 🎯 Co to jest NeuroNote-vibe?

Projekt NeuroNote-vibe został zrealizowany w duchu vibe codingu — lekkiego, intuicyjnego podejścia do programowania, które pozwala szybko przejść od pomysłu do działającego rozwiązania bez zbędnych warstw technicznych.

NeuroNote-vibe łączy potęgę sztucznej inteligencji z prostotą użycia. Pozwala na:

* **Automatyczną transkrypcję** plików audio na tekst
    
* **Tłumaczenie** transkrypcji na język polski
    
* **Zapis wyników** w czytelnych plikach Markdown
    

## 🤔 Dlaczego to przydatne?

Wyobraź sobie, że masz nagranie spotkania w języku angielskim, a chcesz:

* Szybko przejrzeć najważniejsze punkty
    
* Przetłumaczyć na polski dla zespołu
    
* Stworzyć notatki strukturalne
    

Zamiast słuchać całego nagrania, możesz po prostu przeczytać transkrypcję!

## 🛠️ Co będziemy potrzebować?

### Wymagania techniczne

* **Python 3.8+** - język programowania
    
* **Token Hugging Face** - dostęp do modeli AI
    
* **Plik audio** - coś do transkrypcji
    
* **CUDA (opcjonalne)** - dla szybszego przetwarzania
    

### Pakiety Python

```python
# Główne biblioteki, które będziemy używać:
torch                    # PyTorch - framework do AI
transformers            # Hugging Face - modele AI
datasets                # Obsługa danych audio
python-dotenv           # Zarządzanie zmiennymi środowiskowymi
```

## 🚀 Krok po kroku - instalacja

### 1\. Przygotowanie środowiska

```bash
# 1. Sklonuj projekt (lub stwórz nowy katalog)
git clone https://github.com/kudzik/NeuroNote-vibe.git
cd NeuroNote-vibe

# 2. Utwórz środowisko wirtualne (venv)
# Dlaczego venv? Izoluje pakiety projektu od systemu
python3 -m venv .venv

# 3. Aktywuj środowisko wirtualne
source .venv/bin/activate  # Na Linux/Mac
# lub
.venv\Scripts\activate     # Na Windows
```

**💡 Wyjaśnienie:** Środowisko wirtualne to jak "piaskownica" dla Twojego projektu. Zapewnia, że pakiety nie kolidują z innymi projektami.

### 2\. Instalacja zależności

```bash
# Zainstaluj wszystkie potrzebne pakiety
pip install -r requirements.txt
```

**💡 Co się dzieje:** Pobieramy i instalujemy wszystkie biblioteki potrzebne do działania AI.

### 3\. Konfiguracja tokenów

```bash
# Skopiuj plik konfiguracyjny
cp .env_example .env

# Edytuj plik .env i dodaj swój token
nano .env
```

**💡 Wyjaśnienie:** Token to jak "klucz" do modeli AI. Bez niego nie możemy korzystać z usług Hugging Face.

## 🧠 Jak to działa? - Analiza kodu

### Krok 1: Sprawdzenie środowiska wirtualnego

```python
def check_venv():
    """Sprawdza czy program jest uruchamiany w środowisku wirtualnym"""
    if not hasattr(sys, "real_prefix") and not (
        hasattr(sys, "base_prefix") and sys.base_prefix != sys.prefix
    ):
        print("⚠️  UWAGA: Program nie jest uruchamiany w środowisku wirtualnym!")
        # ... reszta kodu
```

**💡 Wyjaśnienie:** Ta funkcja sprawdza, czy używamy venv. To ważne, bo bez venv możemy mieć problemy z pakietami.

### Krok 2: Ładowanie modelu Whisper

```python
# Ładowanie modelu do transkrypcji
processor = WhisperProcessor.from_pretrained("openai/whisper-medium")
model = WhisperForConditionalGeneration.from_pretrained("openai/whisper-medium")
```

**💡 Wyjaśnienie:**

* **Whisper** to model AI od OpenAI, który świetnie radzi sobie z transkrypcją
    
* **Processor** przygotowuje dane audio do przetworzenia
    
* **Model** to "mózg" AI, który faktycznie wykonuje transkrypcję
    

### Krok 3: Przetwarzanie audio

```python
# Wczytanie pliku audio
dataset = Dataset.from_dict({"audio": [audio_path]}).cast_column(
    "audio", Audio(sampling_rate=16000)
)
sample = dataset[0]["audio"]

# Przetworzenie audio przez AI
inputs = processor(
    sample["array"], 
    sampling_rate=sample["sampling_rate"], 
    return_tensors="pt"
)
```

**💡 Wyjaśnienie:**

* **Sampling rate 16000** - to częstotliwość próbkowania, którą preferuje Whisper
    
* **return\_tensors="pt"** - zwraca dane w formacie PyTorch (tensor)
    

### Krok 4: Generowanie transkrypcji

```python
with torch.no_grad():  # Wyłącz obliczanie gradientów (szybsze)
    predicted_ids = model.generate(inputs.input_features.to(device))
transcription_en = processor.batch_decode(predicted_ids, skip_special_tokens=True)[0]
```

**💡 Wyjaśnienie:**

* [**torch.no**](http://torch.no)**\_grad()** - przyspiesza obliczenia, bo nie potrzebujemy gradientów
    
* **model.generate()** - to tutaj AI "słucha" audio i generuje tekst
    
* **batch\_decode()** - zamienia ID tokenów na czytelny tekst
    

### Krok 5: Tłumaczenie

```python
try:
    translator = pipeline(
        "translation",
        model="Helsinki-NLP/opus-mt-en-pl",
        device=0 if torch.cuda.is_available() else -1,
    )
    translated_text = translator(transcription_en)[0]["translation_text"]
except Exception as e:
    # Obsługa błędów...
```

**💡 Wyjaśnienie:**

* **Pipeline** to gotowe narzędzie do tłumaczenia
    
* **device** - używa GPU jeśli dostępne, w przeciwnym razie CPU
    
* **Exception handling** - obsługuje błędy, gdy model nie jest dostępny
    

## 🎯 Praktyczne zastosowania

### 1\. Spotkania biznesowe

```python
# Przykład: Transkrypcja spotkania zespołu
audio_path = "./source/team_meeting.mp3"
# Program automatycznie:
# 1. Transkrybuje spotkanie
# 2. Tłumaczy na polski
# 3. Zapisuje w pliku Markdown
```

### 2\. Wykłady i prezentacje

```python
# Przykład: Transkrypcja wykładu
audio_path = "./source/lecture.mp3"
# Rezultat: Gotowe notatki z wykładu
```

### 3\. Wywiady i podcasty

```python
# Przykład: Transkrypcja wywiadu
audio_path = "./source/interview.mp3"
# Rezultat: Tekst wywiadu gotowy do publikacji
```

## ⚡ Optymalizacja wydajności

### Użycie GPU (CUDA)

```python
# Sprawdź czy masz GPU
device = "cuda" if torch.cuda.is_available() else "cpu"
print(f"🔧 Używane urządzenie: {device}")

# Przenieś model na GPU
model = model.to(device)
```

**💡 Wyjaśnienie:** GPU może być 5-10x szybsze niż CPU dla modeli AI.

### Wybór modelu Whisper

```python
# Różne rozmiary modeli:
"openai/whisper-tiny"    # Najszybszy, mniej dokładny
"openai/whisper-small"  # Kompromis
"openai/whisper-medium"  # Dobra dokładność
"openai/whisper-large"  # Najdokładniejszy, najwolniejszy
```

## 🐛 Rozwiązywanie problemów

### Problem: "ModuleNotFoundError"

```bash
# Rozwiązanie: Sprawdź czy venv jest aktywny
source .venv/bin/activate
pip install -r requirements.txt
```

### Problem: "Brak tokena HUGGINGFACE\_TOKEN"

```bash
# Rozwiązanie: Sprawdź plik .env
cat .env
# Powinien zawierać: HUGGINGFACE_TOKEN=twoj_token_tutaj
```

### Problem: Długi czas przetwarzania

```python
# Rozwiązanie: Użyj mniejszego modelu
model = WhisperForConditionalGeneration.from_pretrained("openai/whisper-tiny")
```

## 🚀 Następne kroki

### Możliwe ulepszenia

1. **Interfejs webowy** - dodaj Flask/Django
    
2. **Batch processing** - przetwarzaj wiele plików naraz
    
3. **Różne języki** - dodaj obsługę innych języków
    
4. **API REST** - udostępnij jako usługę
    

### Przykład: Prosty interfejs webowy

```python
from flask import Flask, request, render_template

app = Flask(__name__)

@app.route('/')
def index():
    return render_template('index.html')

@app.route('/upload', methods=['POST'])
def upload():
    # Przetwórz przesłany plik audio
    # Zwróć transkrypcję
    pass
```

## 📚 Czego się nauczyłeś?

W tym projekcie poznałeś:

* **Środowiska wirtualne** - jak izolować projekty Python
    
* **Modele AI** - jak używać gotowych modeli Hugging Face
    
* **Przetwarzanie audio** - jak pracować z plikami audio w Python
    
* **Obsługa błędów** - jak radzić sobie z problemami
    
* **Optymalizacja** - jak przyspieszyć obliczenia AI
    

## 🎉 Podsumowanie

NeuroNote-vibe to doskonały przykład tego, jak AI może ułatwić codzienną pracę. Dzięki gotowym modelom i bibliotekom Python, w kilka godzin stworzyłeś system, który:

* **Automatycznie transkrybuje** audio na tekst
    
* **Tłumaczy** na język polski
    
* **Zapisuje** wyniki w czytelnej formie
    
* **Jest łatwy w użyciu** i rozszerzeniu
    

## 🔗 Przydatne linki

* [Projekt na GitHub](https://github.com/kudzik/NeuroNote-vibe)
    
* [Hugging Face Models](https://huggingface.co/models)
    
* [PyTorch Documentation](https://pytorch.org/docs/)
    
* [Whisper Paper](https://arxiv.org/abs/2212.04356)
    

## 💬 Masz pytania?

Jeśli masz pytania dotyczące projektu lub chcesz podzielić się swoimi doświadczeniami, skontaktuj się ze mną:

* **Email**: [kudzik@outlook.com](mailto:kudzik@outlook.com)
    
* **Blog**: [https://arturkud.hashnode.dev/](https://arturkud.hashnode.dev/)
    
* **GitHub**: [https://github.com/kudzik](https://github.com/kudzik)
    

---

*Dziękuję za przeczytanie! Jeśli artykuł Ci się podobał, zostaw ⭐ na GitHubie i udostępnij znajomym. Do zobaczenia w następnym projekcie! 🚀*