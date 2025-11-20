---
title: "LangGraph Część 2:  Twój Pierwszy Graf (i Dlaczego Twój Bot Ma Amnezję)"
seoTitle: "Tworzenie Pierwszego Grafu w LangGraph"
seoDescription: "Twórz swoje pierwsze grafy w LangGraph i dowiedz się, dlaczego Twój bot nie pamięta poprzednich rozmów. Rozwiązania w następnej części!"
datePublished: Thu Nov 20 2025 23:00:43 GMT+0000 (Coordinated Universal Time)
cuid: cmi81bm3p000502juf0rffio9
slug: langgraph-czesc-2-twoj-pierwszy-graf-i-dlaczego-twoj-bot-ma-amnezje
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/rH8O0FHFpfw/upload/3f4a08a0566e355803f09ec64cdd09aa.jpeg
tags: tutorial, ai, ai-agents, langgraph

---

Cześć ponownie! 👋

W [pierwszej części](https://arturkud.hashnode.dev/langgraph-czesc-1-dlaczego-graf-a-nie-lancuch) rozmawialiśmy o tym, **dlaczego** LangGraph istnieje i czym różni się od prostych łańcuchów. Dzisiaj wchodzimy w "laboratorium" - zbudujemy razem Twój pierwszy działający graf!

Nie martw się, jeśli kod wygląda na początku dziwnie. Przejdziemy przez wszystko krok po kroku, a na końcu zrozumiesz, dlaczego Twój bot ma amnezję (i jak to naprawić w następnej części).

## Od teorii do praktyki: 5 kroków do Twojego pierwszego grafu

Pamiętasz z pierwszej części, że LangGraph działa w dwóch fazach? Najpierw definiujemy graf (rysujemy plan), potem go uruchamiamy (budujemy dom).

Budowanie grafu to zawsze te same **5 kroków**. Zapamiętaj je, bo będziesz je powtarzać przy każdym projekcie:

1. **Zdefiniuj Stan** - Co Twój agent ma pamiętać?
    
2. **Uruchom Graph Builder** - Stwórz pusty szkielet
    
3. **Dodaj Węzły** - Funkcje, które wykonują pracę
    
4. **Dodaj Krawędzie** - Połącz węzły w logiczny przepływ
    
5. **Skompiluj** - Zamień definicję w działającą maszynę
    

Brzmi prosto? Jest! Przejdźmy przez to razem.

## Krok 1: Zdefiniuj Stan - Pamięć Twojego agenta

Stan to "pamięć" Twojego agenta. Musisz powiedzieć LangGraphowi, co ma przechowywać. W naszym przypadku będzie to lista wiadomości z rozmowy.

```python
from typing import Annotated
from langgraph.graph.message import add_messages
from pydantic import BaseModel

class State(BaseModel):
    messages: Annotated[list, add_messages]
```

"Chwila, co to jest `Annotated`?" - zapytasz. Dobra obserwacja! To kluczowa część, która na początku może być myląca.

### Annotated: Żółta karteczka na pudełku

Wyobraź sobie, że masz pudełko z napisem "Książki" (to jest typ - `list`). `Annotated` pozwala dokleić do tego pudełka żółtą karteczkę z instrukcją "Nie wyrzucać, układać jedną na drugiej" (to jest reduktor - `add_messages`).

**Dlaczego to potrzebne?**

Bez reduktora, gdy agent zwróci nową wiadomość, mogłaby ona **nadpisać** całą historię rozmowy. To jakbyś za każdym razem, gdy ktoś coś powie, kasował całą poprzednią rozmowę i zostawiał tylko to jedno zdanie.

Z reduktorem `add_messages`, LangGraph wie: "Hej, jeśli dostaniesz nową wiadomość, nie kasuj starych! **Dopisz** (append) nową na końcu listy".

To brzmi skomplikowanie, ale w praktyce to tylko jedna linia kodu. LangGraph załatwia resztę za Ciebie.

## Krok 2: Uruchom Graph Builder - Pusta tablica

Gdy mamy już zdefiniowany plan pamięci (klasę `State`), tworzymy "szkielet" grafu:

```python
from langgraph.graph import StateGraph

graph_builder = StateGraph(State)
```

**Ważna uwaga:** Przekazujemy **klasę** (`State`), a nie konkretny obiekt. Mówimy: "Buduj grafy w oparciu o ten schemat", a nie "Użyj tego jednego konkretnego obiektu".

Na razie nasz graf jest pusty - jak pusta tablica, na której za chwilę będziemy rysować węzły i krawędzie.

## Krok 3: Dodaj Węzeł - Miejsce, gdzie dzieje się magia

Węzeł to po prostu funkcja Pythona. Może być **dowolną** funkcją - nie musi być związana z AI!

Żeby to udowodnić, zbudujmy najpierw "głupiego" bota, który losuje słowa. To pokaże, że LangGraph to nie tylko narzędzie do LLM-ów - to ogólny silnik do zarządzania przepływem pracy.

```python
import random

nouns = ["Pingwiny", "Jednorożce", "Tostery", "Banany", "Zombie"]
adjectives = ["błyszczące", "smutne", "pedantyczne", "nastrojowe", "sarkastyczne"]

def our_first_node(old_state: State) -> State:
    reply = f"{random.choice(nouns)} są {random.choice(adjectives)}"
    messages = [{"role": "assistant", "content": reply}]
    
    new_state = State(messages=messages)
    return new_state

graph_builder.add_node("first_node", our_first_node)
```

Co się tutaj dzieje?

1. Funkcja pobiera **stary stan** (lista wiadomości)
    
2. Losuje rzeczownik i przymiotnik (np. "Pingwiny są błyszczące")
    
3. Tworzy **nowy stan** z odpowiedzią
    
4. Zwraca go
    

**Kluczowe:** Zauważ, że funkcja **nie modyfikuje** starego stanu. Tworzy nową wiadomość i ją zwraca. Reduktor `add_messages` zajmie się resztą (dopisaniem jej do listy).

## Krok 4: Dodaj Krawędzie - Połącz kropki

Teraz musimy połączyć kropki. Definiujemy prosty przepływ liniowy:

```python
START → Nasz Pierwszy Węzeł → END
```

```python
from langgraph.graph import START, END

graph_builder.add_edge(START, "first_node")
graph_builder.add_edge("first_node", END)
```

`START` i `END` to specjalne węzły w LangGraph:

* `START` - Punkt wejścia. Każdy graf musi wiedzieć, gdzie zacząć
    
* `END` - Punkt wyjścia. Gdy proces dotrze tutaj, graf kończy pracę
    

## Krok 5: Skompiluj - Zamień plan w maszynę

To moment, w którym LangGraph zamienia naszą definicję w gotową do uruchomienia maszynę:

```python
graph = graph_builder.compile()
```

Teraz możemy uruchomić nasz graf:

```python
initial_state = State(messages=[{"role": "user", "content": "Cześć!"}])
result = graph.invoke(initial_state)
print(result.messages[-1].content)
# Output: "Pingwiny są błyszczące" (lub inne losowe słowa)
```

**Gratulacje!** 🎉 Masz działający graf! Na razie jest "głupi" (losuje słowa), ale struktura jest gotowa. To dowód na to, że LangGraph to nie tylko narzędzie do AI - to ogólny silnik do zarządzania przepływem pracy.

## Czas na prawdziwe AI: Podłączamy OpenAI

Dobra, koniec z losowaniem przymiotników. Czas podłączyć prawdziwy mózg!

Zamieniamy naszą funkcję losującą na wywołanie do OpenAI:

```python
from langchain_openai import ChatOpenAI

llm = ChatOpenAI(model="gpt-4o-mini")

def chatbot_node(old_state: State) -> State:
    response = llm.invoke(old_state.messages)
    new_state = State(messages=[response])
    return new_state

graph_builder.add_node("chatbot", chatbot_node)
graph_builder.add_edge(START, "chatbot")
graph_builder.add_edge("chatbot", END)

graph = graph_builder.compile()
```

Teraz Twój bot używa prawdziwego AI! Możesz z nim rozmawiać:

```python
initial_state = State(messages=[{"role": "user", "content": "Cześć! Jak się masz?"}])
result = graph.invoke(initial_state)
print(result.messages[-1].content)
# Bot odpowiada jak prawdziwy AI!
```

## Problem: Twój bot ma amnezję! 🤦

Świetnie, masz działającego bota z AI! Ale jest jeden problem...

Spróbuj tego:

```python
# Pierwsza wiadomość
state1 = State(messages=[{"role": "user", "content": "Nazywam się Batman."}])
result1 = graph.invoke(state1)
print(result1.messages[-1].content)
# Bot: "Cześć Batman! Miło Cię poznać."

# Druga wiadomość (w "nowej rozmowie")
state2 = State(messages=[{"role": "user", "content": "Jak mam na imię?"}])
result2 = graph.invoke(state2)
print(result2.messages[-1].content)
# Bot: "Nie wiem, nie mam dostępu do Twoich danych." 😱
```

**Co się stało?!** Bot zapomniał, że powiedziałeś mu swoje imię!

### Diagnoza: Problem "Stateless"

Nasz graf działa poprawnie technicznie (łączy się z AI), ale brakuje mu **Persistence (Trwałości)**.

Gdy uruchamiamy `graph.invoke()`, tworzymy **nowy proces**. Graf przetwarza dane, zwraca wynik i... **zapomina wszystko**.

Jeśli wywołasz `graph.invoke()` drugi raz z nowym pytaniem, graf nie wie, co stało się za pierwszym razem. Dlatego bot nie pamięta że jesteś "Batmanem".

**Jak to działa (i dlaczego zapomina):**

1. **Uruchomienie 1:**
    
    * Ty: "Nazywam się Batman."
        
    * Graf: Dostaje wiadomość → Wysyła do OpenAI → OpenAI widzi "Nazywam się Batman" → Odpowiada "Cześć Batman"
        
    * **Koniec procesu.** Pamięć RAM jest czyszczona (w uproszczeniu).
        
2. **Uruchomienie 2:**
    
    * Ty: "Jak mam na imię?"
        
    * Graf: Tworzy **nowy** stan. W tym nowym stanie jest tylko jedna wiadomość: "Jak mam na imię?"
        
    * OpenAI: Widzi tylko "Jak mam na imię?" (bez kontekstu Batmana)
        
    * Odpowiedź: "Nie wiem, nie mam dostępu do twoich danych"
        

### Dlaczego to się dzieje?

W obecnej formie przekazujemy historię tylko w ramach **jednego** wywołania. Nie zapisujemy nigdzie historii "pomiędzy" wywołaniami (np. w bazie danych czy pamięci sesji).

To jak rozmowa z kimś, kto po każdej wymianie zdań zapomina, co było wcześniej. Technicznie działa, ale praktycznie bezużyteczne.

## Podsumowanie: Co zbudowaliśmy?

Przeszliśmy długą drogę:

1. ✅ Zrozumieliśmy teorię grafów i stanu
    
2. ✅ Zbudowaliśmy "głupi" graf losujący słowa (dowód, że LangGraph to nie tylko AI)
    
3. ✅ Podłączyliśmy prawdziwe AI (OpenAI)
    
4. ✅ Zidentyfikowaliśmy problem amnezji
    

Mamy działającego bota, ale cierpi on na amnezję. To idealny punkt wyjścia do **następnej części**, gdzie zajmiemy się **Pamięcią (Memory/Checkpointers)** oraz **Narzędziami (Tools)**. To tam nasz agent stanie się naprawdę użyteczny.

## Kluczowe wnioski z dzisiejszej lekcji

1. **LangGraph to nie tylko AI** - Możesz używać go do dowolnych funkcji Pythona
    
2. **Reduktor to magia** - `add_messages` automatycznie zarządza historią rozmowy
    
3. **Niezmienność stanu** - Zawsze tworzymy nowy stan, nie modyfikujemy starego
    
4. **Problem amnezji** - Bez mechanizmu pamięci, graf zapomina między wywołaniami
    

Do zobaczenia w następnej części! 🚀

---

*Masz pytania? Zostaw komentarz poniżej! Chętnie pomogę i wyjaśnię wątpliwości.*