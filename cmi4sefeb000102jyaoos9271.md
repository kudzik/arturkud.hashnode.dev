---
title: "CrewAI dla Początkujących: Jak Zbudować Zespół AI Agentów w 10 Minut (Prawie) 🚀"
seoTitle: "Zbuduj Zespół AI Agentów w 10 Minut"
seoDescription: "Dowiedz się, jak szybko stworzyć zespół AI z agentami za pomocą CrewAI, ucz się podstaw i rozpocznij własne projekty"
datePublished: Tue Nov 18 2025 16:27:39 GMT+0000 (Coordinated Universal Time)
cuid: cmi4sefeb000102jyaoos9271
slug: crewai-dla-poczatkujacych-jak-zbudowac-zespol-ai-agentow-w-10-minut-prawie
tags: ai, crewai, agentic-ai

---

[Repozytorium github](https://github.com/kudzik/ai_agents_crewai_debate)

Cześć! Jeśli kiedykolwiek marzyłeś o tym, żeby mieć własny zespół AI agentów, którzy pracują razem jak prawdziwi koledzy z biura (tylko bez kawy i plotek), to trafiłeś we właściwe miejsce!

Dzisiaj nauczysz się używać **CrewAI** - frameworka, który pozwala tworzyć zespoły agentów AI, które mogą współpracować nad złożonymi zadaniami. Brzmi jak science fiction? To jest! Ale działa już teraz! 🎉

---

## Czym Jest CrewAI? (Wersja dla Ludzi, Nie Robotów)

Wyobraź sobie, że masz projekt do zrobienia. Zamiast robić wszystko sam (jak zwykle 😅), możesz stworzyć zespół specjalistów AI:

* **Badacz** - zbiera informacje
    
* **Pisarz** - tworzy treści
    
* **Redaktor** - poprawia i ulepsza
    
* **Krytyk** - ocenia jakość
    

I wszyscy pracują razem, jeden po drugim, jak w prawdziwym zespole! To właśnie robi CrewAI - organizuje agentów AI w spójną załogę (crew).

**Dlaczego to fajne?**

* Nie musisz ręcznie koordynować każdego agenta
    
* Każdy agent ma swoją rolę i specjalizację
    
* Wszystko działa automatycznie (prawie jak magia! ✨)
    

---

## Trzy Kluczowe Koncepcje (Krótko i Na Temat)

Zanim zaczniemy kodować, musisz zrozumieć trzy podstawowe pojęcia. Obiecuję, że to będzie szybkie!

### 1\. Agent = Twój Pracownik AI 🤖

Agent to jak pracownik w zespole. Ma:

* **Rolę** - kim jest (np. "Badacz", "Pisarz")
    
* **Cel** - co chce osiągnąć (np. "Zbadać temat X")
    
* **Historię** - kontekst o nim (np. "Jesteś doświadczonym dziennikarzem")
    
* **Model AI** - który LLM używa (np. GPT-4, Claude)
    

**Przykład z życia:** To jak zatrudnienie copywritera - mówisz mu kim jest, co ma zrobić, i dajesz mu narzędzia do pracy.

### 2\. Task = Zadanie Do Wykonania 📋

Task to konkretne zadanie, które agent musi wykonać. Ma:

* **Opis** - co dokładnie ma zrobić
    
* **Oczekiwany wynik** - jak powinien wyglądać rezultat
    
* **Przypisanie** - który agent to zrobi
    

**Przykład z życia:** "Napisz artykuł o AI" to zadanie. "Stwórz 500 słów o historii AI" to już konkretne zadanie z oczekiwanym wynikiem.

### 3\. Crew = Twój Zespół 👥

Crew to zespół agentów i zadań, które pracują razem. To jak manager, który:

* Przydziela zadania odpowiednim agentom
    
* Koordynuje przepływ pracy
    
* Upewnia się, że wszystko idzie w odpowiedniej kolejności
    

**Przykład z życia:** To jak projekt w firmie - masz zespół, zadania, i wszystko musi być zsynchronizowane.

---

## Instalacja: Pierwsze Kroki (Nie Bój Się!)

Zanim zaczniemy, musisz mieć:

* Python 3.10+ (sprawdź: `python --version`)
    
* Konto OpenAI z kluczem API (lub inny dostawca LLM)
    

### Krok 1: Zainstaluj CrewAI

```bash
pip install crewai
```

Albo jeśli używasz nowoczesnego narzędzia UV (polecam!):

```bash
pip install uv
uv pip install crewai
```

### Krok 2: Utwórz Projekt

CrewAI ma wygodne narzędzie CLI, które tworzy całą strukturę projektu za Ciebie:

```bash
crewai create crew my_first_crew
```

To jak magiczna różdżka - tworzy wszystkie potrzebne pliki i foldery! 🪄

### Krok 3: Skonfiguruj Klucz API

Utwórz plik `.env` w głównym katalogu projektu:

```python
OPENAI_API_KEY=twój_klucz_tutaj
```

**Uwaga:** Nigdy nie commituj tego pliku do Git! (Dodaj `.env` do `.gitignore`)

---

## Twój Pierwszy Projekt: Zespół Debatujący 🤝

Zbudujmy coś praktycznego - zespół, który prowadzi debatę! Będziemy mieli:

* **Debatanta** - przedstawia argumenty
    
* **Sędziego** - ocenia i wybiera zwycięzcę
    

### Struktura Projektu (Nie Panikuj!)

Po uruchomieniu `crewai create crew debate`, otrzymasz taką strukturę:

```python
debate/
└── src/
    └── debate/
        ├── config/
        │   ├── agents.yaml    # Tu definiujesz agentów
        │   └── tasks.yaml     # Tu definiujesz zadania
        ├── crew.py            # Tu łączysz wszystko razem
        └── main.py            # Tu uruchamiasz projekt
```

**Nie martw się** - większość pracy to edycja plików YAML (są proste, obiecuję!).

---

## Krok 1: Zdefiniuj Agentów (agents.yaml)

Otwórz plik `config/agents.yaml`. To jak lista pracowników - każdy ma swoje dane.

```yaml
debater:
  role: >
    Przekonujący Debatant
  goal: >
    Przedstaw jasny argument za lub przeciw wnioskowi. Wnioskowanie to: {motion}
  backstory: >
    Jesteś doświadczonym dyskutantem z talentem do podawania zwięzłych, 
    ale przekonujących argumentów. Wnioskowanie to: {motion}
  llm: gpt-4o-mini  # Tani model, wystarczy do debaty!

judge:
  role: >
    Wyłonienie zwycięzcy debaty na podstawie przedstawionych argumentów
  goal: >
    Przejrzyj argumenty za i przeciw wnioskowi: {motion} i wyłon stronę, 
    która jest bardziej przekonująca.
  backstory: >
    Jesteś uczciwym sędzią z reputacją oceniającym argumenty bez uwzględniania 
    własnych poglądów. Wnioskowanie to: {motion}
  llm: gpt-4o-mini
```

**Co się tutaj dzieje?**

* `role` - kim jest agent (jak stanowisko w firmie)
    
* `goal` - co ma osiągnąć (jak cel projektu)
    
* `backstory` - kontekst (jak CV pracownika)
    
* `llm` - który model AI używa (jak narzędzie pracy)
    
* `{motion}` - to szablon, który wypełnimy później (jak zmienna w kodzie)
    

**Pro Tip:** `{motion}` to szablon - możesz go używać w wielu miejscach, a później wypełnisz go wartością. To jak placeholder w dokumentach Word!

---

## Krok 2: Zdefiniuj Zadania (tasks.yaml)

Teraz otwórz `config/tasks.yaml`. To lista zadań do wykonania.

```yaml
propose:
  description: >
    Jesteś proponującym wnioskowanie: {motion}.
    Przedstaw jasny argument ZA wnioskowaniem.
    Bądź bardzo przekonujący!
  expected_output: >
    Twój jasny argument za wnioskowaniem, w zwięzły sposób.
  agent: debater
  output_file: output/propose.md

oppose:
  description: >
    Jesteś przeciwnikiem wnioskowania: {motion}.
    Przedstaw jasny argument PRZECIW wnioskowaniu.
    Bądź bardzo przekonujący!
  expected_output: >
    Twój jasny argument przeciw wnioskowaniu, w zwięzły sposób.
  agent: debater
  output_file: output/oppose.md

decide:
  description: >
    Przejrzyj przedstawione argumenty przez dyskutantów i wyłon stronę, 
    która jest bardziej przekonująca.
  expected_output: >
    Twoja decyzja na temat strony, która jest bardziej przekonująca, i dlaczego.
  agent: judge
  output_file: output/decide.md
```

**Co się tutaj dzieje?**

* `description` - co agent ma zrobić (instrukcja)
    
* `expected_output` - jak powinien wyglądać wynik (wymagania)
    
* `agent` - który agent to zrobi (przypisanie)
    
* `output_file` - gdzie zapisać wynik (automatycznie!)
    

**Ciekawe:** Jeden agent (`debater`) wykonuje dwa zadania (`propose` i `oppose`). To jak pracownik, który robi różne rzeczy w ciągu dnia!

---

## Krok 3: Połącz Wszystko ([crew.py](http://crew.py))

Teraz otwórz `src/debate/`[`crew.py`](http://crew.py). To serce projektu - tutaj łączysz wszystko razem.

```python
from crewai.project import CrewBase, agent, crew, task
from crewai import Agent, Crew, Process, Task
from typing import List
from crewai.agents.agent_builder.base_agent import BaseAgent

@CrewBase
class DebateCrew:
    """Zespół debatujący"""
    
    agents: List[BaseAgent]
    tasks: List[Task]
    
    # Definiujemy agenta "debater"
    @agent
    def debater(self) -> Agent:
        return Agent(
            config=self.agents_config["debater"],
            verbose=True,  # Pokazuje co robi (dla debugowania)
        )
    
    # Definiujemy agenta "judge"
    @agent
    def judge(self) -> Agent:
        return Agent(
            config=self.agents_config["judge"],
            verbose=True,
        )
    
    # Definiujemy zadanie "propose"
    @task
    def propose_task(self) -> Task:
        return Task(
            config=self.tasks_config["propose"],
        )
    
    # Definiujemy zadanie "oppose"
    @task
    def oppose_task(self) -> Task:
        return Task(
            config=self.tasks_config["oppose"],
        )
    
    # Definiujemy zadanie "decide"
    @task
    def decide_task(self) -> Task:
        return Task(
            config=self.tasks_config["decide"],
        )
    
    # Tworzymy załogę (to najważniejsze!)
    @crew
    def crew(self) -> Crew:
        return Crew(
            agents=self.agents,  # Automatycznie wypełnione przez @agent
            tasks=self.tasks,    # Automatycznie wypełnione przez @task
            process=Process.sequential,  # Zadania jedno po drugim
            verbose=True,
        )
```

**Co się tutaj dzieje?**

* `@CrewBase` - dekorator klasy (magia Python!)
    
* `@agent` - automatycznie dodaje agenta do listy
    
* `@task` - automatycznie dodaje zadanie do listy
    
* `@crew` - tworzy finalną załogę
    
* `Process.sequential` - zadania wykonują się jedno po drugim (jak kolejka)
    

**Dekoratory to magia!** 🪄 Automatycznie rejestrują agentów i zadania, więc nie musisz ręcznie ich dodawać do list. To jak automatyczna rejestracja pracowników!

---

## Krok 4: Uruchom Projekt ([main.py](http://main.py))

Otwórz `src/debate/`[`main.py`](http://main.py) i zobacz jak to działa:

```python
from debate.crew import DebateCrew
from datetime import datetime

def run():
    # To są dane, które wypełnią szablony {motion} i {current_year}
    inputs = {
        "motion": "Samoświadomość i AI LLMs",  # Temat debaty
        "current_year": str(datetime.now().year),  # Aktualny rok
    }
    
    # Uruchamiamy załogę!
    DebateCrew().crew().kickoff(inputs=inputs)

if __name__ == "__main__":
    run()
```

**Co się tutaj dzieje?**

* `inputs` - słownik z danymi dla szablonów
    
* `{motion}` w YAML zostanie zastąpione przez `"Samoświadomość i AI LLMs"`
    
* `kickoff()` - uruchamia całą załogę (jak "start" w wyścigu!)
    

**Uruchomienie:**

```bash
crewai run
```

Albo przez Python:

```bash
python -m debate.main
```

---

## Co Się Dzieje Pod Spodem? (Dla Ciekawskich)

Gdy uruchamiasz projekt, CrewAI:

1. **Wczytuje konfigurację** z plików YAML
    
2. **Tworzy agentów** z ich rolami, celami i historiami
    
3. **Tworzy zadania** z opisami i oczekiwanymi wynikami
    
4. **Wypełnia szablony** (`{motion}` → `"Samoświadomość i AI LLMs"`)
    
5. **Wykonuje zadania sekwencyjnie:**
    
    * Najpierw `propose` (argument za)
        
    * Potem `oppose` (argument przeciw)
        
    * Na końcu `decide` (decyzja sędziego)
        
6. **Zapisuje wyniki** do plików w folderze `output/`
    

**To jak automatyczna fabryka!** 🏭 Wszystko działa samo, a Ty tylko dajesz instrukcje.

---

## Wyniki: Co Otrzymasz?

Po uruchomieniu, w folderze `output/` znajdziesz:

* [`propose.md`](http://propose.md) - argument za wnioskowaniem
    
* [`oppose.md`](http://oppose.md) - argument przeciw wnioskowaniu
    
* [`decide.md`](http://decide.md) - decyzja sędziego z uzasadnieniem
    

**To jak raport z projektu!** 📊 Wszystko zapisane automatycznie, gotowe do przeczytania.

---

## Najczęstsze Błędy (I Jak Ich Uniknąć) 🐛

### Błąd 1: "Agent not found"

**Problem:** W `tasks.yaml` używasz nazwy agenta, której nie ma w `agents.yaml`

**Rozwiązanie:** Sprawdź, czy nazwy się zgadzają (case-sensitive!)

```yaml
# ❌ Źle
agent: Debater  # Duże D

# ✅ Dobrze
agent: debater  # Małe d (jak w agents.yaml)
```

### Błąd 2: "Template variable not found"

**Problem:** Używasz `{motion}` w YAML, ale nie podałeś go w `inputs`

**Rozwiązanie:** Dodaj wszystkie używane szablony do `inputs`:

```python
inputs = {
    "motion": "Twój temat",
    "current_year": "2025",  # Jeśli używasz {current_year}
}
```

### Błąd 3: "API Key not found"

**Problem:** Nie masz klucza API w `.env`

**Rozwiązanie:**

1. Utwórz plik `.env` w głównym katalogu
    
2. Dodaj: `OPENAI_API_KEY=twój_klucz`
    
3. Upewnij się, że plik nie jest w `.gitignore` (ale klucz nie powinien być w Git!)
    

---

## Zaawansowane: Różne Modele AI

CrewAI używa **LiteLLM**, co oznacza, że możesz używać różnych modeli AI!

### Format w YAML

```yaml
# Z prefiksem dostawcy
llm: openai/gpt-4o-mini
llm: anthropic/claude-3-haiku
llm: google/gemini-2.5-flash

# Bez prefiksu (domyślnie OpenAI)
llm: gpt-4o-mini
```

### Przykład: Różne Modele dla Różnych Agentów

```yaml
debater:
  llm: openai/gpt-4o  # Drogie, ale lepsze argumenty

judge:
  llm: gpt-4o-mini  # Tańsze, wystarczy do oceny
```

**To jak zatrudnienie eksperta i stażysty!** 💰 Jeden droższy, ale lepszy, drugi tańszy, ale wystarczający.

---

## Zaawansowane: Narzędzia (Tools) dla Agentów

Agenci mogą używać narzędzi! To jak dawać pracownikowi dodatkowe narzędzia do pracy.

### Przykład: Custom Tool

```python
from typing import Type
from crewai.tools import BaseTool
from pydantic import BaseModel, Field

class SearchInput(BaseModel):
    query: str = Field(..., description="Zapytanie do wyszukania")

class WebSearchTool(BaseTool):
    name: str = "web_search"
    description: str = "Wyszukuje informacje w internecie"
    args_schema: Type[BaseModel] = SearchInput
    
    def _run(self, query: str) -> str:
        # Tutaj twoja logika wyszukiwania
        return f"Wyniki dla: {query}"
```

### Użycie w Agencie

```python
@agent
def researcher(self) -> Agent:
    return Agent(
        config=self.agents_config["researcher"],
        tools=[WebSearchTool()],  # Dodajemy narzędzie!
        verbose=True,
    )
```

**To jak dawać badaczowi dostęp do Google!** 🔍

---

## Tryby Procesu: Sequential vs Hierarchical

CrewAI ma dwa tryby pracy:

### Sequential (Sekwencyjny) - Domyślny

```python
process=Process.sequential
```

Zadania wykonują się **jedno po drugim**, w kolejności zdefiniowanej w `tasks.yaml`.

**Kiedy używać:** Gdy zadania zależą od siebie (jak w debacie - najpierw argumenty, potem decyzja).

### Hierarchical (Hierarchiczny)

```python
process=Process.hierarchical
```

Specjalny **Manager LLM** przydziela zadania agentom dynamicznie.

**Kiedy używać:** Gdy potrzebujesz elastyczności i autonomicznych decyzji.

**To jak różnica między szefem, który daje listę zadań, a szefem, który decyduje na bieżąco!** 👔

---

## Praktyczne Wskazówki 💡

### 1\. Zaczynaj Małe

Nie próbuj od razu budować skomplikowanego systemu. Zacznij od 2 agentów i 2-3 zadań.

### 2\. Testuj Często

Po każdej zmianie w YAML, uruchom projekt i sprawdź wyniki.

### 3\. Używaj Verbose

Zawsze ustaw `verbose=True` podczas debugowania - zobaczysz co się dzieje!

### 4\. Czytaj Outputy

Wyniki w folderze `output/` to najlepsze źródło informacji o tym, co robią agenci.

### 5\. Eksperymentuj z Modelami

Różne modele dają różne wyniki. Testuj i znajdź najlepsze dla swojego przypadku.

---

## Co Dalej? 🚀

Gratulacje! Masz już podstawową wiedzę o CrewAI. Co możesz zrobić dalej?

### 1\. Rozbuduj Projekt Debaty

* Dodaj więcej agentów (np. moderator, publiczność)
    
* Użyj różnych modeli dla różnych agentów
    
* Dodaj narzędzia (np. wyszukiwanie informacji)
    

### 2\. Stwórz Własny Projekt

* Zespół badawczy (researcher → writer → editor)
    
* Zespół marketingowy (analyst → copywriter → designer)
    
* Zespół wsparcia (triage → specialist → reviewer)
    

### 3\. Eksploruj Dokumentację

* [Oficjalna dokumentacja CrewAI](https://docs.crewai.com)
    
* [Przykłady projektów](https://github.com/joaomdmoura/crewai)
    
* [Discord społeczności](https://discord.com/invite/X4JWnZnxPb)
    

---

## Podsumowanie: Co Wiesz Teraz? 📚

✅ **CrewAI** organizuje agentów AI w zespoły  
✅ **Agent** = pracownik z rolą, celem i historią  
✅ **Task** = zadanie do wykonania  
✅ **Crew** = zespół agentów i zadań  
✅ **YAML** = konfiguracja (proste!)  
✅ **Dekoratory** = automatyczna rejestracja  
✅ **Inputs** = wypełnianie szablonów  
✅ **Sequential** = zadania jedno po drugim

---

## Ostatnie Słowo (I Trochę Humoru) 😄

Pamiętaj: CrewAI to narzędzie, nie magiczna różdżka. Agenty AI są mądre, ale nie doskonałe. Czasem będą robić głupie rzeczy (jak prawdziwi pracownicy! 😅).

**Klucz do sukcesu:**

* Dobra konfiguracja (YAML)
    
* Jasne instrukcje (description, expected\_output)
    
* Testowanie i iteracja
    
* Cierpliwość (AI też potrzebuje czasu!)
    

**I najważniejsze:** Baw się dobrze! Eksperymentuj, testuj, ucz się. To jest przyszłość programowania - i Ty jesteś jej częścią! 🎉

---

**Masz pytania?** Napisz w komentarzach lub sprawdź dokumentację. Powodzenia w budowaniu swoich zespołów AI! 🚀