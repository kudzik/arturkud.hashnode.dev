---
title: "Orkiestracja LLM Krok Po Kroku"
seoTitle: "Przewodnik po Orkiestracji LLM"
seoDescription: "Efektywnie orkiestruj LLMs, optymalizując koszty, wydajność i niezawodność z użyciem wzorca Orkiestrator-Wykonawca"
datePublished: Thu Nov 13 2025 09:27:01 GMT+0000 (Coordinated Universal Time)
cuid: cmhx868gi000802l10ely0tty
slug: orkiestracja-llm-krok-po-kroku
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/qWwpHwip31M/upload/161a1ef7f21fdfb107e014c37dd810fd.jpeg
tags: orchestration, llm, agents, agentic-ai

---

**#LLM #AI #AgentAI #Python #Programowanie**

W dzisiejszym świecie Large Language Models (LLM) nie chodzi już tylko o wybór GPT-4. Prawdziwa moc tkwi w **orkiestracji**: wykorzystaniu wielu modeli jednocześnie, aby osiągnąć optymalizację kosztów, szybkości i niezawodności.

Zapomnij o poleganiu na jednym, potencjalnie "halucynującym" modelu. Pokażę Ci, jak zaimplementować wzorzec **Orkiestrator-Wykonawca** w Pythonie, używając popularnego agregatora API.

---

## 1\. Wybór Modeli: Koniec z Monogamią

Pierwszą zasadą inżynierii agentowej jest zrozumienie, że żaden model nie jest idealny do wszystkiego.

| Kategoria | Charakterystyka | Idealne Zastosowanie |
| --- | --- | --- |
| **Komercyjne (GPT-4, Claude 3)** | Najwyższa wydajność, ale drogie. | Krytyczne rozumowanie, Ewaluacja, Planowanie. |
| **Otwarte (Llama 3, Mistral)** | Pełna kontrola, możliwość lokalnego uruchomienia (Ollama). | Zadania z mniejszym kontekstem, szybkie prototypowanie. |
| **Optymalizacyjne (Groq, GPT-4o Mini)** | Niska latencja (Groq) lub świetny stosunek koszt/wydajność (Mini). | Zadania wrażliwe na czas (Groq), Wykonawcy prostych zadań (Mini). |

**Kluczowa Strategia:** Najdroższe modele powierzaj tylko najtrudniejszym zadaniom (np. ocenianiu wyników innych modeli).

---

## 2\. Podstawa Architektury: Wzorce Agentowe

Nasz system będzie wykorzystywał trzy kluczowe wzorce:

1. **Planista (Planner) / Łańcuch Monitów (Prompt Chaining):** Pierwszy, tani model (np. GPT-4o Mini) generuje zadanie. Jego wynik jest automatycznie przekazywany dalej.
    
2. **Wykonawcy (Workers) / Równoległe Działanie:** To samo zadanie jest wysyłane do wielu różnych modeli (Wykonawców), aby uzyskać zróżnicowane perspektywy.
    
3. **Ewaluator (Evaluator) / Sędzia:** Najsilniejszy i najbardziej niezawodny model (np. Gemini/GPT-4) ocenia wyniki Wykonawców i uszeregowuje je w formacie **JSON**, co umożliwia programistyczne użycie.
    

---

## 3\. Instrukcja Krok Po Kroku: Budowa Orkiestratora

Aby ułatwić zarządzanie wieloma modelami, użyjemy **OpenRouter** – agregatora API, który pozwala wywoływać różne modele za pomocą **jednego, ujednoliconego klienta** `openai`.

### Krok 1: Instalacja i Konfiguracja Środowiska

#### Tworzenie i Aktywacja Wirtualnego Środowiska (venv)

Wirtualne środowisko izoluje zależności projektu od globalnego Pythona. To najlepsza praktyka.

**Na Windows (PowerShell):**

```powershell
# Tworzenie wirtualnego środowiska
python -m venv venv

# Aktywacja wirtualnego środowiska
.\venv\Scripts\Activate.ps1
```

**Na macOS / Linux (Bash/Zsh):**

```bash
# Tworzenie wirtualnego środowiska
python3 -m venv venv

# Aktywacja wirtualnego środowiska
source venv/bin/activate
```

Po aktywacji powinieneś zobaczyć `(venv)` na początku linii wiersza poleceń.

#### Instalacja Wymaganych Pakietów

Po aktywacji venv, zainstaluj wymagane pakiety:

```bash
# Instalacja wymaganych pakietów
pip install openai python-dotenv
```

#### Konfiguracja Klucza API

Utwórz plik `.env` w katalogu projektu i dodaj swój klucz API:

```bash
# Utwórz plik .env w katalogu projektu (Windows/macOS/Linux)
# Wpisz w nim:
OPENROUTER_API_KEY="Twój_klucz_z_OpenRouter"
```

**Alternatywnie, bez aktywacji venv (dla szybkiego testowania):**

```bash
# Instalacja w konkretnym venv bez aktywacji
./venv/Scripts/pip install openai python-dotenv  # Windows
./venv/bin/pip install openai python-dotenv       # macOS/Linux
```

### Krok 2: Inicjalizacja Klienta i Planista

Inicjalizujemy klienta `openai` (kierując go na endpoint OpenRouter) i tworzymy **Planistę** – pierwszy model, który generuje trudne pytanie.

```python
import os
import json
from dotenv import load_dotenv
from openai import OpenAI

load_dotenv(override=True)

# 1. Konfiguracja klienta (używamy standardu OpenAI, ale kierujemy na OpenRouter)
client = OpenAI(
    base_url="https://openrouter.ai/api/v1",
    api_key=os.environ.get("OPENROUTER_API_KEY"),
)

# 2. Wzorzec Planista/Kreator Zadań
request_prompt = "Proszę o wymyślenie trudnego, zniuansowanego pytania, które mogę zadać wielu LLM, aby ocenić ich inteligencję. Proszę o odpowiedź tylko na to pytanie. Bez wyjaśnienia. Proszę o odpowiedź w języku polskim."

completion = client.chat.completions.create(
    model="openai/o4-mini", # Tani i wystarczająco silny Planista
    messages=[{"role": "user", "content": request_prompt}],
)

question_for_workers = completion.choices[0].message.content
print(f"Pytanie Planisty: {question_for_workers}\n")
```

### Krok 3: Wykonawcy – Równoległe Zbieranie Odpowiedzi

Wykorzystujemy pytanie wygenerowane w poprzednim kroku (`question_for_workers`) i przesyłamy je do różnych modeli Wykonawców.

```python
competitors = [
    "openai/o4-mini", # Najlepszy stosunek ceny do wydajności
    "google/gemini-2.5-flash", # Szybki model od Google
    "anthropic/claude-haiku-4.5", # Szybki i zniuansowany model Anthropic
    "x-ai/grok-4-fast", # Nowy gracz
]

answers = []
print("--- Zbieranie odpowiedzi od Wykonawców ---")

# Pamiętaj: ten kod wykonuje się SEKWENCYJNIE. W produkcji użyj asyncio!
for competitor in competitors:
    print(f"Wywołuję: {competitor}...")
    completion = client.chat.completions.create(
        model=competitor,
        messages=[{"role": "user", "content": question_for_workers}],
    )
    answer = completion.choices[0].message.content
    answers.append(answer)
    print(f"{competitor} - Zakończono.\n")

# Zapisanie odpowiedzi w jednym bloku do oceny
together = ""
for index, answer in enumerate(answers):
    together += f"# Odpowiedź Konkurenta {index + 1} ({competitors[index]})\n\n"
    together += answer + "\n\n"
```

### Krok 4: Ewaluator – Ocena i Ranking (Klucz do Automatyzacji)

Najważniejszy krok: używamy silnego modelu jako Sędziego i **żądając formatu JSON**, umożliwiamy programistyczne odczytanie wyników.

```python
# Wzorzec Ewaluator (Sędzia)
judge_prompt = f"""
Oceniasz rywalizację między {len(competitors)} konkurentami.
Pytanie zadane wszystkim modelom: {question_for_workers}

Twoim zadaniem jest ocena każdej odpowiedzi pod kątem jasności, logiki i siły argumentacji oraz uszeregowanie ich od najlepszej do najgorszej.
Odpowiedz w formacie JSON, i tylko JSON, w następującym formacie (używaj numerów Konkurentów: 1, 2, 3, ...):
{{"results": ["numer najlepszego konkurenta", "numer drugiego", "numer trzeciego", ...]}}

Oto odpowiedzi poszczególnych uczestników:

{together}

Teraz odpowiedz w formacie JSON, podając kolejność i nic więcej. Nie dodawaj formatowania Markdown ani bloków kodu.
"""

# Wywołanie Sędziego
completion_judge = client.chat.completions.create(
    model="openai/gpt-5", # Silny model do Ewaluacji
    messages=[{"role": "user", "content": judge_prompt}],
)

response_judge = completion_judge.choices[0].message.content

# 4. Programistyczne parsowanie wyników z JSON
try:
    results_dict = json.loads(response_judge)
    ranks = results_dict["results"]
    print("\n--- Ostateczny Ranking Orkiestratora ---")
    for index, result_number in enumerate(ranks):
        # Konwertujemy numer konkurenta (np. "1") na int i odejmujemy 1, aby uzyskać indeks listy
        competitor_name = competitors[int(result_number) - 1] 
        print(f"Miejsce {index + 1}: {competitor_name}")
except json.JSONDecodeError:
    print("\nBłąd parsowania JSON. Ewaluator nie zwrócił poprawnego formatu.")
    print(f"Surowa odpowiedź Ewaluatora:\n{response_judge}")
```

---

## 💡 Podsumowanie: Dlaczego to jest kluczowe?

Zdolność do orkiestracji modeli to nie tylko zabawa – to klucz do **zwiększonej solidności** i **masowej optymalizacji kosztów** w produkcji. Dzięki temu możesz świadomie routować zadania, zapewniając najlepszy wynik, przy jednoczesnym kontrolowaniu budżetu.