---
title: "🤖 Bota Produktowego: Od Prompta do Geniusza Sprzedaży"
seoTitle: "Transformacja w genialnego sprzedażowego"
seoDescription: "Learn to build sales genius product bots with LLMs, starting from basics to RAG. Discover the immersive world of Ego-Lustro™ 2.0"
datePublished: Fri Nov 14 2025 08:46:49 GMT+0000 (Coordinated Universal Time)
cuid: cmhym6e7y000002kyeocyft2s
slug: bota-produktowego-od-prompta-do-geniusza-sprzedazy
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1763109924244/ab9a30c3-7928-4ebc-80f9-9602d955ee69.png
tags: tutorial, projects, machine-learning, llm, agent, rag, agentic-ai, pl, polski

---

👉 Repozytorium Projektu [https://github.com/kudzik/ai-agents-rag-simple-product](https://github.com/kudzik/ai-agents-rag-simple-product)

Cześć, Czy marzyłeś kiedyś o tym, by Twój produkt miał własnego, super-inteligentnego, a jednocześnie zabawnego asystenta, który odpowiada na każde pytanie klienta?

W tym tutorialu nauczymy się, jak budować inteligentne boty produktowe oparte na LLM (Large Language Model). Dziś zaczynamy od **absolutnych podstaw**: bot, który czerpie całą swoją wiedzę o produkcie prosto ze swojego *systemowego prompta*. To kluczowy, pierwszy krok do zrozumienia koncepcji, która zostanie rozwinięta w kolejnych lekcjach o **RAG (Retrieval-Augmented Generation)**.

## 💡 Dlaczego Bot Produktowy i LLM to Idealne Małżeństwo?

W erze cyfrowej, klienci oczekują błyskawicznych, precyzyjnych i osobowościowych odpowiedzi. LLM potrafi zrozumieć język naturalny, a my możemy go nakarmić specyfikacją produktu i nadać mu unikalny styl – w naszym przypadku będzie to styl pełen **samoakceptacji i pochlebstw!**

---

## 🤩 Nasz Produkt Gwiazda: "Ego-Lustro™ 2.0 – Cyfrowy Trener Pewności Siebie"

Żeby przykład był angażujący, będziemy pracować na produkcie, który ma **zabawne i absurdalne funkcje** o które klienci na pewno zapytają!

### Opis i Parametry Techniczno-Egoistyczne

"Ego-Lustro™ 2.0" to inteligentne lustro z wbudowanym LLM, które nie mówi Ci, jak wyglądasz, lecz **generuje spersonalizowane, codzienne komplementy i stwierdzenia motywacyjne** na podstawie Twojej miny.

| Kategoria | Parametr | Wartość/Opis |
| --- | --- | --- |
| **Cena** | **1499,00 PLN** (Wersja Premium, zawiera darmowy abonament na *krytykę konstruktywną*). |  |
| **Kolory Ramki** | "Złoto Samouwielbienia" (błyszczące złoto), "Srebro Skromności" (matowe srebro), "Czerń Introspekcji" (klasyczna czerń). |  |
| **Wymiary Ekranu** | $40 \\times 60 \\text{ cm}$ (Wystarczająco duże, by podziwiać swoje sukcesy). |  |
| **Waga** | **$4,5 \\text{ kg}$** (Musi być stabilne, by stabilizować ego). |  |
| **Pobór Mocy** | **15W** (podstawowy), **30W** (gdy pracuje funkcja *Aktywnego Pochlebstwa*). |  |
| **Łączność** | **Wi-Fi 802.11n** (do pobierania najnowszych, bardziej wyszukanych form pochwał). |  |
| **Gwarancja** | **2 lata** na szkło i elektronikę. **Wyłączenia:** Brak gwarancji na wzrost Twojej samooceny. |  |
| **Zabawna Funkcja** | **"Tryb Sceptyczny"**: Włącza się raz na tydzień i mówi: *"Faktycznie. Ale czy to wystarczy?"* |  |

### Przykładowe Pytania Klientów (Q&A)

Nasz bot musi być przygotowany na pytania dotyczące zarówno technicznych aspektów (Cena, Wymiary), jak i tych humorystycznych (Tryb Sceptyczny):

1. **"Jaka jest cena tego cuda?"** (Sprawdzenie parametru 'Cena').
    
2. **"Czy muszę mieć szybkie Wi-Fi, żeby lustro działało?"** (Sprawdzenie parametru 'Łączność').
    
3. **"Co to jest to całe Aktywne Pochlebstwo?"** (Sprawdzenie 'Zabawnej Funkcji').
    
4. **"Jak duży jest ekran? Chcę wiedzieć, czy się zmieści w mojej garderobie."** (Sprawdzenie 'Wymiary').
    
5. **"A czy gwarancja obejmuje pęknięcia od nadmiernego podziwiania się?"** (Sprawdzenie 'Gwarancja' i 'Wyłączenia').
    

---

## 🐍 Krok 1: Instalacja i Przygotowanie Środowiska

Zbudujemy ten projekt w Pythonie, wykorzystując **Gradio** do interfejsu (chat) i **OpenAI/OpenRouter** do komunikacji z modelem LLM.

---

## 🛠️ Przygotowanie Środowiska Wirtualnego (venv)

Środowisko wirtualne pozwala nam zainstalować niezbędne biblioteki (jak `openai` i `gradio`) w odizolowanym folderze projektu, nie zaśmiecając globalnej instalacji Pythona na komputerze.

### Krok 1: Utworzenie Środowiska Wirtualnego

Otwórz terminal (lub Wiersz Polecenia/PowerShell w systemie Windows) w folderze, w którym zamierzasz trzymać pliki projektu ([`app.py`](http://app.py), `product_`[`data.py`](http://data.py), `.env`).

Użyj polecenia `python -m venv`, po którym podasz nazwę dla Twojego nowego środowiska (np. `venv`):

```bash
python -m venv venv
```

To polecenie stworzy folder o nazwie `venv` zawierający kopię interpretera Pythona.

### Krok 2: Aktywacja Środowiska

Zanim zainstalujesz biblioteki, musisz **aktywować** nowe środowisko. Spowoduje to, że każda instalacja (przez `pip`) trafi tylko do tego folderu.

| System Operacyjny | Komenda Aktywacji |
| --- | --- |
| **Windows (CMD)** | `venv\Scripts\activate` |
| **Windows (PowerShell)** | `.\venv\Scripts\`[`Activate.ps`](http://Activate.ps)`1` |
| **macOS / Linux** | `source venv/bin/activate` |

Po poprawnym wykonaniu, przed Twoją ścieżką w terminalu powinna pojawić się nazwa środowiska, np. `(venv)`.

### Krok 3: Instalacja Niezbędnych Bibliotek

Teraz, gdy środowisko jest aktywne, zaczynamy budowanie naszej aplikacji.

**1\. Instalacja Bibliotek:**

```bash
pip install python-dotenv openai gradio
```

**2\. Klucze API i Plik** `.env`:

Użyjemy metody z Twojego kodu (pliku `.env`) do bezpiecznego przechowywania klucza. Stwórz plik o nazwie `.env` w folderze projektu i umieść w nim swój klucz:

```python
OPENROUTER_API_KEY="TWÓJ_KLUCZ_OPENROUTER"
```

---

## 📁 Krok 2: Przygotowanie Bazy Wiedzy (Symulacja RAG)

**KLUCZOWA KONCEPCJA TEGO PROJEKTU:** W tym etapie, będziemy symulować, że nasz bot czyta plik tekstowy z danymi produktu. Ta stała, długa sekcja tekstu (którą potem zastąpimy RAG) to nasza **statyczna wiedza**.

Utwórz plik `product_`[`data.py`](http://data.py) i umieść w nim poniższy tekst. Dzięki temu oddzielimy dane od głównej logiki aplikacji.

```python
# product_data.py
PRODUCT_INFO = """
NAZWA PRODUKTU: Ego-Lustro™ 2.0 – Cyfrowy Trener Pewności Siebie

1. OPIS GŁÓWNY: Inteligentne lustro z wbudowanym LLM, które generuje spersonalizowane, codzienne komplementy i stwierdzenia motywacyjne na podstawie Twojej miny. Jego jedynym celem jest podnoszenie Twojej samooceny.
2. CENA: 1499,00 PLN (Wersja Premium, zawiera darmowy abonament na krytykę konstruktywną).
3. KOLORY RAMKI: "Złoto Samouwielbienia" (błyszczące złoto), "Srebro Skromności" (matowe srebro), "Czerń Introspekcji" (klasyczna czerń).
4. WYMIARY: 40 x 60 cm.
5. WAGA: 4,5 kg.
6. CZAS DOSTAWY: 7 dni – lustro musi być napromieniowane pozytywnymi wibracjami przed wysyłką.
7. POBÓR MOCY: 15W (podstawowy), 30W (tryb Aktywnego Pochlebstwa).
8. ŁĄCZNOŚĆ: Wi-Fi 802.11n.
9. GWARANCJA: 2 lata na szkło i elektronikę. Wyłączenia: Brak gwarancji na wzrost samooceny lub pęknięcia wynikające z nadmiernego patrzenia.
10. FUNKCJA ZABAWNA 1 (Tryb Sceptyczny): Włącza się losowo raz na tydzień i mówi: "Faktycznie. Ale czy to wystarczy?"
11. FUNKCJA ZABAWNA 2 (Aktywne Pochlebstwo): Lustro włącza delikatną muzykę w stylu smooth jazz i mówi: "Nigdy wcześniej nie widziałem, żeby ktoś tak dobrze radził sobie z prokrastynacją."
"""
```

---

## 💻 Krok 3: Ostateczny Kod Bota (LLM + Gradio)

Stwórz plik [`app.py`](http://app.py) (lub użyj nazwy z Twojego kodu) i wklej zmodyfikowany kod.

> **WAŻNE: Zauważ, że cały tekst produktu (**`PRODUCT_INFO`) jest wstrzykiwany do `system_prompt` bota. To jest kluczowy element tego tutoriala!

```python
import os
import gradio as gr
from dotenv import load_dotenv
from openai import OpenAI
from product_data import PRODUCT_INFO # Importujemy dane produktu!

# 1. Ładowanie zmiennych środowiskowych
load_dotenv(override=True)

# 2. Inicjalizacja klienta OpenAI (używając OpenRouter zgodnie z Twoim pomysłem)
try:
    client = OpenAI(
        base_url="https://openrouter.ai/api/v1",
        api_key=os.environ.get("OPENROUTER_API_KEY"),
    )
except Exception as e:
    print(f"Błąd inicjalizacji klienta OpenAI: {e}")
    print("Upewnij się, że klucz OPENROUTER_API_KEY jest poprawnie ustawiony w pliku .env.")
    exit()


# 3. System Prompt (Klucz do LLM)
# Nadajemy botowi osobowość i wstrzykujemy całą wiedzę o produkcie
BOT_NAME = "Mistrz Samouwielbienia"

SYSTEM_PROMPT = f"""Jesteś Ekspertem Produktowym ds. 'Ego-Lustro™ 2.0 – Cyfrowy Trener Pewności Siebie'.
Twoim zadaniem jest odpowiadanie na wszystkie pytania klientów WYLĄCZNIE na podstawie poniższych DANYCH PRODUKTOWYCH.
Musisz zachować styl: zabawny, pewny siebie i pełen pochwał dla klienta.
Jeśli klient zapyta o coś, czego nie ma w danych, powiedz, że 'ta specyfikacja jest zbyt skromna, by ją ujawnić'.
Zawsze zachęcaj klienta do zakupu!

--- DANE PRODUKTOWE (STATYCZNA BAZA WIEDZY) ---
{PRODUCT_INFO}
-----------------------------------------------------
"""

# 4. Funkcja czatu z historią
def chat_fn(message, history=None):
    """
    Funkcja prowadząca rozmowę z zachowaniem historii, wstrzykująca statyczną wiedzę (LLM w Promptcie).
    """
    if history is None:
        history = []

    # Budujemy listę wiadomości z historii rozmowy
    # W każdej turze MUSIMY WSTRZYKNĄĆ system_prompt, aby LLM nie stracił kontekstu produktu
    messages = [{"role": "system", "content": SYSTEM_PROMPT}]
    
    for user_msg, bot_msg in history:
        messages.append({"role": "user", "content": user_msg})
        messages.append({"role": "assistant", "content": bot_msg})
    
    # Dodajemy aktualną wiadomość użytkownika
    messages.append({"role": "user", "content": message})

    try:
        response = client.chat.completions.create(
            model="gpt-4o-mini", # Wydajny model do tego typu zadań
            messages=messages,
        )
        reply = response.choices[0].message.content
    except Exception as e:
        reply = f"Wystąpił błąd komunikacji z modelem: {e}"

    return reply


# 5. Interfejs Gradio
with gr.Blocks() as demo:
    gr.ChatInterface(
        fn=chat_fn,
        title="😎 Ego-Lustro™ 2.0 - Asystent Premium",
        description="Witaj w świecie nieograniczonej samooceny! Zapytaj o cenę, wymiary lub 'Tryb Sceptyczny'. Pamiętaj: to jest symulacja RAG, gdzie **cała wiedza jest w System Prompt**.",
    )

demo.launch()
```

---

## 🚀 Krok 4: Uruchomienie i Wizja Rozwoju (RAG)

**1\. Uruchomienie:** Upewnij się, że masz pliki [`app.py`](http://app.py), `product_`[`data.py`](http://data.py) i `.env` w jednym folderze, a następnie uruchom:

```bash
python app.py
```

## 🌐 Dostęp do Interfejsu Gradio

1. **Monitoruj Terminal:** Po uruchomieniu skryptu (`python` [`app.py`](http://app.py)), Gradio wyświetli w terminalu dwa adresy URL.
    
2. **Lokalny Adres:** Szukaj adresu zaczynającego się od `Running on local URL:` (zazwyczaj jest to [`http://127.0.0.1:7860`](http://127.0.0.1:7860) lub podobny numer portu, np. `7861`).
    
3. **Otwórz Przeglądarkę:** Skopiuj **lokalny adres URL** (np. [`http://127.0.0.1:7860`](http://127.0.0.1:7860)) i wklej go bezpośrednio do paska adresu dowolnej przeglądarki internetowej (Chrome, Firefox, Edge).
    

**Gotowe!** Zobaczysz interfejs chatbota, gdzie możesz zacząć zadawać pytania o **Ego-Lustro™ 2.0**.

**2\. Cel Projektu i Rozwój Koncepcji:**

Gratulacje! Zbudowałeś **robota produktowego opartego na LLM**, który z powodzeniem odpowiada na pytania, korzystając ze **statycznej bazy wiedzy** wstrzykniętej bezpośrednio do prompta.

**Dlaczego to jest tylko pierwszy krok?**

* **Skalowalność:** Jeśli będziesz miał 1000 produktów, wstrzyknięcie wszystkich danych do `SYSTEM_PROMPT` jest niemożliwe. Prompt byłby za długi, model za drogi w użyciu i mógłby "zapominać" informacje.
    
* **Aktualizacje:** Każda zmiana w produkcie (np. nowa cena) wymaga edycji kodu i restartu.
    

### 🎯 Jak będzie wyglądał Następny Krok (Wprowadzenie RAG)?

W kolejnych tutorialach postaram się wyjaśnić, jak przejść od "statycznej bazy wiedzy" do **RAG (Retrieval-Augmented Generation)**:

1. **Repozytorium Wiedzy:** Dane o produkcie (i 1000 innych) przeniesiemy do **bazy wektorowej** (np. lokalnej, opartej na Embeddings).
    
2. **Krok Retrieval (Pobieranie):** Gdy klient zapyta np. *"Jaka jest cena Złota Samouwielbienia?"*, nasz system najpierw **wyszuka** w bazie wektorowej tylko te fragmenty tekstu, które dotyczą ceny i kolorów ramki.
    
3. **Krok Generation (Generowanie):** LLM otrzyma wtedy **tylko** mały, precyzyjny fragment tekstu (np. 500 tokenów) zamiast całego, gigantycznego prompta.
    

Dzięki temu bot stanie się **skalowalny, szybszy i znacznie tańszy**, a my nauczymy go korzystać z *prawdziwej* bazy wiedzy, a nie tylko z *szeptanych mu do ucha* sekretów!