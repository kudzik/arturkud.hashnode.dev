---
title: "CrewAI Część 3: Od Chaosu do Porządku: Jak Ustrukturyzowane Wyjścia oraz Pamię"
seoTitle: "Od chaosu do porządku: ustrukturyzowane wyniki sztucznej inteligencji"
seoDescription: "Odkryj, jak ustrukturyzowane wyjścia i zaawansowane techniki pamięci w CrewAI przekształcają chaos danych w uporządkowane rozwiązania"
datePublished: Wed Nov 19 2025 20:36:38 GMT+0000 (Coordinated Universal Time)
cuid: cmi6gqgzj000002ju7rse2on7
slug: crewai-czesc-3-od-chaosu-do-porzadku-jak-ustrukturyzowane-wyjscia-oraz-pamie
tags: ai, memory-management, ai-tools, ai-agents, crewai

---

[Repozytorium](https://github.com/kudzik/ai_agents_crew_stock_picker)

*Część 3 serii: "Budowanie Zaawansowanych Systemów Multi-Agentowych z CrewAI"*

---

## 🤔 Wyobraź Sobie To...

Wyobraź sobie, że masz zespół agentów AI, którzy pracują nad wyborem najlepszej firmy do inwestycji. Wszystko idzie świetnie - agenci znajdują firmy, analizują je, wybierają najlepszą... A potem dostajesz wyniki.

I co widzisz?

```python
"Firma Apple jest świetna, bo ma iPhone i jest w trendzie. 
Microsoft też jest dobry, ma Azure. Google ma AI. 
Wybór: Apple, bo iPhone."
```

Hmm... 🤔 To wygląda jak notatka z wykładu, a nie dane, które możesz użyć w kodzie! Jak masz przetworzyć to w Pythonie? Jak masz sprawdzić, czy agent rzeczywiście znalazł 3 firmy? Jak masz wyciągnąć symbol giełdowy?

**Właśnie tutaj wkraczają Ustrukturyzowane Wyjścia!** 🎉

---

## 🎯 Czym Są Ustrukturyzowane Wyjścia? (Prosty Wyjaśnienie)

Ustrukturyzowane wyjścia to jak **umowa z agentem AI**: "Słuchaj, możesz być kreatywny, ale wyniki musisz zwrócić w TYM konkretnym formacie JSON. Bez dyskusji."

To jak dać dziecku kolorowankę z konturami - może wybrać kolory, ale musi się zmieścić w liniach. 🖍️

### Dlaczego To Ważne?

**Bez ustrukturyzowanych wyjść:**

* ❌ Agent zwraca tekst w dowolnym formacie
    
* ❌ Musisz parsować tekst ręcznie (koszmar!)
    
* ❌ Nie wiesz, czy wszystkie dane są obecne
    
* ❌ Każde uruchomienie może dać inny format
    

**Z ustrukturyzowanymi wyjściami:**

* ✅ Agent ZAWSZE zwraca dane w tym samym formacie
    
* ✅ Automatyczna walidacja (Pydantic sprawdza wszystko)
    
* ✅ Łatwe przetwarzanie w Pythonie
    
* ✅ Przewidywalność i stabilność
    

---

## 🏗️ Jak To Działa? (Krok Po Kroku)

### Krok 1: Definiujesz "Kontrakt" (Schemat Pydantic)

To jest jak projekt budynku - zanim zaczniesz budować, musisz wiedzieć, jak ma wyglądać.

```python
from pydantic import BaseModel, Field
from typing import List

class TrendingCompany(BaseModel):
    """Pojedyncza firma w trendzie"""
    
    name: str = Field(description="Nazwa firmy")
    ticker: str = Field(description="Symbol giełdowy (np. AAPL, MSFT)")
    reason: str = Field(description="Dlaczego firma jest w wiadomościach")

class TrendingCompanyList(BaseModel):
    """Lista firm w trendzie"""
    
    companies: List[TrendingCompany] = Field(
        description="Lista 2-3 firm, które są popularne w wiadomościach"
    )
```

**Co się tutaj dzieje?**

1. `BaseModel` - To jest klasa z Pydantic, która mówi: "To jest schemat danych"
    
2. `Field` - To jest jak instrukcja dla LLM: "To pole powinno zawierać..."
    
3. `description` - To jest podpowiedź dla agenta AI, co ma wypełnić
    

**Prosta analogia:** To jak formularz Google Forms. Definiujesz pola (name, ticker, reason) i dajesz instrukcje (description), a agent wypełnia formularz. 📝

### Krok 2: Mówisz Agentowi: "Użyj Tego Schematu!"

W definicji zadania dodajesz `output_pydantic`:

```python
@task
def find_trending_companies(self) -> Task:
    return Task(
        config=self.tasks_config["find_trending_companies"],
        output_pydantic=TrendingCompanyList,  # 👈 To jest klucz!
    )
```

**Co się dzieje?**

Agent otrzymuje schemat i mówi: "Aha! Muszę zwrócić dane w formacie `TrendingCompanyList`, który zawiera listę `companies`, a każda firma ma `name`, `ticker` i `reason`. Rozumiem!"

### Krok 3: Agent Zwraca Dane w Formacie JSON

Zamiast chaotycznego tekstu, dostajesz:

```json
{
  "companies": [
    {
      "name": "Apple Inc.",
      "ticker": "AAPL",
      "reason": "Nowy iPhone 15 generuje duże zainteresowanie w mediach"
    },
    {
      "name": "Microsoft Corporation",
      "ticker": "MSFT",
      "reason": "Wzrost w chmurze Azure i integracja AI"
    },
    {
      "name": "NVIDIA Corporation",
      "ticker": "NVDA",
      "reason": "Zapotrzebowanie na karty graficzne do AI"
    }
  ]
}
```

**I to działa ZAWSZE!** 🎉

---

## 💡 Prawdziwy Przykład z Projektu Stock Picker

W naszym projekcie Stock Picker mamy trzy główne schematy:

### 1\. Schemat dla Firm w Trendzie

```python
class TrendingCompany(BaseModel):
    """Firma, która jest w wiadomościach i przyciąga uwagę"""
    
    name: str = Field(description="Nazwa firmy")
    ticker: str = Field(description="Symbol giełdowy firmy")
    reason: str = Field(description="Powód, dlaczego firma jest w wiadomościach")
```

**Użycie:** Agent `trending_company_finder` używa tego, aby zwrócić listę firm.

### 2\. Schemat dla Analizy Finansowej

```python
class TrendingCompanyResearch(BaseModel):
    """Szczegółowa analiza firmy"""
    
    name: str = Field(description="Nazwa firmy")
    market_position: str = Field(
        description="Aktualna pozycja rynkowa i analiza konkurencji"
    )
    future_outlook: str = Field(
        description="Perspektywy rozwoju i potencjał inwestycyjny"
    )
    investment_potential: str = Field(
        description="Potencjał inwestycyjny i odpowiedniość dla inwestycji"
    )
```

**Użycie:** Agent `financial_researcher` używa tego, aby zwrócić szczegółową analizę każdej firmy.

### 3\. Lista Analiz

```python
class TrendingCompanyResearchList(BaseModel):
    """Lista szczegółowych analiz wszystkich firm"""
    
    research_list: List[TrendingCompanyResearch] = Field(
        description="Kompleksowa analiza wszystkich firm w trendzie"
    )
```

**Użycie:** Agent zwraca analizę wszystkich firm w jednym obiekcie.

---

## 🎨 Jak To Wygląda w Praktyce?

### Przed (Bez Ustrukturyzowanych Wyjść)

```python
# Agent zwraca tekst
result = "Firma Apple jest świetna, bo ma iPhone. Microsoft ma Azure. 
          Google ma AI. Wybór: Apple."

# Musisz parsować ręcznie (koszmar!)
# Jak wyciągnąć nazwę? Jak symbol? Jak powód?
# Każde uruchomienie może dać inny format!
```

### Po (Z Ustrukturyzowanymi Wyjściami)

```python
# Agent zwraca obiekt Pythona
result = TrendingCompanyList(
    companies=[
        TrendingCompany(
            name="Apple Inc.",
            ticker="AAPL",
            reason="Nowy iPhone 15 generuje zainteresowanie"
        ),
        # ...
    ]
)

# Możesz od razu użyć!
for company in result.companies:
    print(f"{company.name} ({company.ticker}): {company.reason}")
    # Automatyczna walidacja - jeśli brakuje pola, Pydantic rzuci błąd!
```

**Różnica jest jak między chaosem a porządkiem!** 🎯

---

## 🚀 Proces Hierarchiczny: Kiedy Twój Manager AI Decyduje Za Ciebie

### Czym Jest Proces Hierarchiczny?

Wyobraź sobie, że masz zespół pracowników i szefa. W procesie **sekwencyjnym**, szef mówi: "Najpierw zrób A, potem B, potem C. Bez dyskusji."

W procesie **hierarchicznym**, szef mówi: "Mamy cel X. Ty decyduj, w jakiej kolejności wykonać zadania i kto je wykonuje. Ufam twojemu osądowi."

**Proces hierarchiczny = Manager AI z supermocą delegacji!** 🦸‍♂️

### Jak To Działa?

```python
@crew
def crew(self) -> Crew:
    # Tworzymy managera
    manager = Agent(
        config=self.agents_config["manager"],
        allow_delegation=True,  # 👈 To jest klucz!
    )
    
    return Crew(
        agents=self.agents,
        tasks=self.tasks,
        process=Process.hierarchical,  # 👈 Proces hierarchiczny
        manager_agent=manager,  # 👈 Kto zarządza?
        # ...
    )
```

**Co się dzieje?**

1. **Manager otrzymuje listę zadań i agentów**
    
2. **Manager analizuje:** "Mam zadania A, B, C. Mam agentów X, Y, Z. Które zadanie wykonać pierwsze? Który agent jest najlepszy?"
    
3. **Manager decyduje:** "Zadanie A jest najważniejsze. Agent X jest najlepszy do tego. Deleguję!"
    
4. **Agent X wykonuje zadanie**
    
5. **Manager analizuje wyniki i decyduje dalej**
    

**To jak mieć szefa, który nie śpi i zawsze podejmuje najlepsze decyzje!** 😄

### Przykład z Projektu Stock Picker

```python
Manager: "Mam trzy zadania:
         1. Znajdź firmy w trendzie
         2. Przeanalizuj firmy
         3. Wybierz najlepszą firmę
         
         Hmm... Muszę najpierw znaleźć firmy, żeby móc je analizować.
         Deleguję zadanie 1 do 'trending_company_finder'!"

Trending Company Finder: [Wykonuje zadanie, zwraca listę firm]

Manager: "Świetnie! Mam listę firm. Teraz potrzebuję analizy.
         Deleguję zadanie 2 do 'financial_researcher'!"

Financial Researcher: [Wykonuje zadanie, zwraca analizy]

Manager: "Doskonale! Mam analizy. Teraz potrzebuję wyboru.
         Deleguję zadanie 3 do 'stock_picker'!"

Stock Picker: [Wykonuje zadanie, wybiera firmę, wysyła powiadomienie]
```

**I wszystko działa automatycznie!** 🎉

### Zalety Procesu Hierarchicznego

✅ **Elastyczność** - Manager może zmieniać kolejność w zależności od sytuacji  
✅ **Optymalizacja** - Manager wybiera najlepszego agenta dla każdego zadania  
✅ **Adaptacyjność** - System reaguje na nieoczekiwane sytuacje  
✅ **Skalowalność** - Łatwo dodać nowe zadania i agentów

### Wady (Bo Nikt Nie Jest Idealny)

❌ **Nieprzewidywalność** - Kolejność może być różna przy każdym uruchomieniu  
❌ **Złożoność** - Wymaga bardziej zaawansowanego managera (np. GPT-4)  
❌ **Koszty** - Manager wykonuje dodatkowe wywołania LLM

**Ale warto!** Proces hierarchiczny daje twoim agentom autonomię, której potrzebują do rozwiązywania złożonych problemów. 🚀

---

## 🛠️ Niestandardowe Narzędzia: Daj Swoim Agentom Supermoce!

### Czym Są Niestandardowe Narzędzia?

CrewAI oferuje wiele wbudowanych narzędzi (wyszukiwanie, kalkulacje, itp.), ale czasami potrzebujesz czegoś specjalnego. **Niestandardowe narzędzie to twoja własna funkcja, którą agent może wywołać.**

**To jak dać agentowi magiczną różdżkę, którą sam stworzyłeś!** 🪄

### Przykład: Narzędzie do Powiadomień Push

W naszym projekcie Stock Picker stworzyliśmy narzędzie, które wysyła powiadomienia push do użytkownika. Oto jak to działa:

#### Krok 1: Definiujesz Schemat Wejściowy

```python
from pydantic import BaseModel, Field

class PushNotification(BaseModel):
    """Schemat danych wejściowych dla narzędzia"""
    
    message: str = Field(
        ...,
        description="Treść powiadomienia do wysłania do użytkownika."
    )
```

**To jest jak formularz dla agenta:** "Jeśli chcesz wysłać powiadomienie, wypełnij to pole."

#### Krok 2: Tworzysz Klasę Narzędzia

```python
from crewai.tools import BaseTool
from typing import Type
import os
import requests

class PushNotificationTool(BaseTool):
    """Narzędzie do wysyłania powiadomień push"""
    
    name: str = "Wyślij powiadomienie"
    description: str = (
        "Narzędzie do wysyłania powiadomień push do użytkownika. "
        "Użyj tego narzędzia, gdy chcesz powiadomić użytkownika o ważnej decyzji."
    )
    args_schema: Type[BaseModel] = PushNotification

    def _run(self, message: str) -> str:
        """Wykonuje wysłanie powiadomienia push"""
        
        # Pobierz dane z zmiennych środowiskowych
        pushover_user = os.getenv("PUSHOVER_USER")
        pushover_token = os.getenv("PUSHOVER_TOKEN")
        pushover_url = os.getenv(
            "PUSHOVER_URL", 
            "https://api.pushover.net/1/messages.json"
        )
        
        # Przygotuj dane
        payload = {
            "user": pushover_user,
            "token": pushover_token,
            "message": message,
        }
        
        # Wyślij
        response = requests.post(pushover_url, data=payload, timeout=10)
        response.raise_for_status()
        
        return '{"notification": "ok"}'
```

**Kluczowe elementy:**

1. `BaseTool` - Bazowa klasa dla wszystkich narzędzi
    
2. `name` - Nazwa widoczna dla agenta
    
3. `description` - Opis używany przez agenta do decyzji o użyciu
    
4. `args_schema` - Schemat Pydantic definiujący parametry
    
5. `_run()` - Faktyczna logika narzędzia
    

#### Krok 3: Przypisujesz Narzędzie do Agenta

```python
@agent
def stock_picker(self) -> Agent:
    return Agent(
        config=self.agents_config["stock_picker"],
        tools=[PushNotificationTool()],  # 👈 Dodajesz narzędzie!
        memory=True,
    )
```

**I gotowe!** Teraz agent `stock_picker` może autonomicznie zdecydować, kiedy wysłać powiadomienie push. 🎉

### Jak Agent Używa Narzędzia?

1. **Agent otrzymuje opis narzędzia** w swoim kontekście
    
2. **Podczas wykonywania zadania**, agent myśli: "Hmm, wybrałem firmę. Powinienem powiadomić użytkownika!"
    
3. **Agent wywołuje narzędzie** z odpowiednimi parametrami
    
4. **Narzędzie wykonuje się** i zwraca wynik
    
5. **Agent kontynuuje pracę** z informacją, że powiadomienie zostało wysłane
    

**To jak dać agentowi telefon i pozwolić mu dzwonić, kiedy uzna to za stosowne!** 📱

---

## 💾 Pamięć: Kiedy Twoi Agenci Pamiętają (I Dlaczego To Ważne)

### Problem: Agenci Nie Pamiętają

Wyobraź sobie, że za każdym razem, gdy rozmawiasz z kimś, ta osoba zapomina wszystko, co było wcześniej. Denerwujące, prawda? 😤

**To samo dotyczy agentów AI!** Bez pamięci, każda interakcja zaczyna się od zera. Agent nie pamięta, że wcześniej wybrał firmę X, więc może wybrać ją ponownie.

### Rozwiązanie: System Pamięci CrewAI

CrewAI oferuje trzy typy pamięci:

1. **Krótkoterminowa** - Pamięta ostatnie interakcje (jak pamięć RAM)
    
2. **Długoterminowa** - Pamięta ważne informacje (jak dysk twardy)
    
3. **Encji** - Pamięta informacje o konkretnych rzeczach (jak baza wiedzy)
    

### Jak To Działa? (Prosty Wyjaśnienie)

**Bez pamięci:**

```python
Agent: "Wybieram firmę Apple!"
[Koniec interakcji]
[Nowa interakcja]
Agent: "Wybieram firmę Apple!"  # Znowu?!
```

**Z pamięcią:**

```python
Agent: "Wybieram firmę Apple!"
[Pamięć zapisuje: "Wybrano Apple w dniu X"]
[Koniec interakcji]
[Nowa interakcja]
Pamięć: "Hej, wcześniej wybrałeś Apple. Może wybierz coś innego?"
Agent: "Aha! Wybieram Microsoft!"  # Pamięta!
```

### Konfiguracja Pamięci w Projekcie

```python
from crewai.memory import EntityMemory, LongTermMemory, ShortTermMemory
from crewai.memory.storage.ltm_sqlite_storage import LTMSQLiteStorage
from crewai.memory.storage.rag_storage import RAGStorage

@crew
def crew(self) -> Crew:
    # Pamięć długoterminowa (SQLite)
    long_term_memory = LongTermMemory(
        storage=LTMSQLiteStorage(
            db_path="./memory/long_term_mem_store.db"
        )
    )
    
    # Pamięć krótkoterminowa (RAG/ChromaDB)
    short_term_memory = ShortTermMemory(
        storage=RAGStorage(
            embedder_config={
                "provider": "openai",
                "model": "text-embedding-3-small",  # Model do embeddings
            },
            type="short_term",
            path="./memory",
        )
    )
    
    # Pamięć encji (RAG/ChromaDB)
    entity_memory = EntityMemory(
        storage=RAGStorage(
            embedder_config={
                "provider": "openai",
                "model": "text-embedding-3-small",
            },
            type="short_term",
            path="./memory",
        )
    )
    
    return Crew(
        # ...
        memory=True,  # 👈 Włącza system pamięci
        long_term_memory=long_term_memory,
        short_term_memory=short_term_memory,
        entity_memory=entity_memory,
    )
```

**I gotowe!** Teraz twoi agenci pamiętają! 🧠

### Włączanie Pamięci dla Konkretnych Agentów

Nie wystarczy włączyć pamięci w załodze - każdy agent musi mieć ją włączoną osobno:

```python
@agent
def trending_company_finder(self) -> Agent:
    return Agent(
        config=self.agents_config["trending_company_finder"],
        tools=[SerperDevTool()],
        memory=True,  # 👈 Włączona pamięć dla tego agenta
    )
```

**Dlaczego selektywnie?**

* Nie każdy agent potrzebuje pamięci
    
* Pamięć kosztuje (dodatkowe wywołania API)
    
* Niektóre zadania są jednorazowe (nie potrzebują pamięci)
    

**W naszym projekcie:**

* ✅ `trending_company_finder` - ma pamięć (aby unikać duplikatów)
    
* ❌ `financial_researcher` - bez pamięci (czysta analiza danych)
    
* ✅ `stock_picker` - ma pamięć (aby unikać powtórzeń)
    

---

## 🎯 Podsumowanie: Co Wiesz Teraz?

Po przeczytaniu tego wpisu wiesz:

✅ **Ustrukturyzowane wyjścia** - jak wymusić format JSON z Pydantic  
✅ **Proces hierarchiczny** - jak dać managerowi AI autonomię  
✅ **Niestandardowe narzędzia** - jak stworzyć własne funkcje dla agentów  
✅ **System pamięci** - jak sprawić, aby agenci pamiętali

**To jak przejście z podstaw do zaawansowanych technik!** 🚀

---

## 🚀 Co Dalej?

Gratulacje! Masz już wiedzę o zaawansowanych technikach CrewAI. Co możesz zrobić dalej?

### 1\. Eksperymentuj

* Dodaj nowe pola do schematów Pydantic
    
* Stwórz własne narzędzie (np. wysyłanie emaili)
    
* Testuj proces hierarchiczny vs sekwencyjny
    
* Eksperymentuj z pamięcią
    

### 2\. Stwórz Własny Projekt

* Zespół marketingowy (badanie → strategia → kampania)
    
* Zespół wsparcia (analiza → rozwiązanie → dokumentacja)
    
* Zespół contentowy (badanie → pisanie → edycja)
    

### 3\. Czytaj Dokumentację

* [Oficjalna dokumentacja CrewAI](https://docs.crewai.com)
    
* [Ustrukturyzowane wyjścia](https://docs.crewai.com/concepts/tasks#structured-outputs)
    
* [Proces hierarchiczny](https://docs.crewai.com/concepts/processes#hierarchical)
    
* [System pamięci](https://docs.crewai.com/concepts/memory)
    

---

## 💬 Ostatnie Słowo (I Trochę Humoru)

Pamiętaj: **Ustrukturyzowane wyjścia, proces hierarchiczny, niestandardowe narzędzia i pamięć** to jak dać swoim agentom:

* 📋 **Formularz** (ustrukturyzowane wyjścia)
    
* 👔 **Szefa** (proces hierarchiczny)
    
* 🛠️ **Narzędzia** (niestandardowe narzędzia)
    
* 🧠 **Pamięć** (system pamięci)
    

**Klucz do sukcesu:**

* Dobra konfiguracja schematów Pydantic
    
* Jasne opisy narzędzi
    
* Selektywne włączanie pamięci
    
* Testowanie i iteracja
    
* Cierpliwość (AI też potrzebuje czasu!)
    

**I najważniejsze:** Baw się dobrze! Eksperymentuj, testuj, ucz się. To jest przyszłość programowania - i Ty jesteś jej częścią! 🎉

---

**Masz pytania?** Napisz w komentarzach lub sprawdź dokumentację. W następnej części serii nauczymy się o jeszcze bardziej zaawansowanych technikach CrewAI! 🚀

---

*Ten wpis jest częścią serii o CrewAI. Sprawdź* [*pierwszą część*](https://arturkud.hashnode.dev/crewai-czesc-1-crewai-dla-poczatkujacych-jak-zbudowac-zespol-ai-agentow-w-10-minut-prawie) *o podstawach i* [*drugą część*](https://arturkud.hashnode.dev/crewai-czesc-2-daj-swoim-agentom-supermoce-narzedzia-i-kontekst) *o narzędziach i kontekście, jeśli jeszcze ich nie czytałeś!*