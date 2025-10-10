---
title: "ReCoder - AI, które tłumaczy Python na C++"
seoTitle: "ReCoder-vibe: Tłumacz AI z Python na C++ z Benchmarkiem Wydajności"
seoDescription: "Automatyczne narzędzie AI do tłumaczenia kodu z języka Python na C++ w celu optymalizacji wydajności. Przetestuj swój kod, porównaj wyniki i przyspiesz swoj"
datePublished: Fri Oct 10 2025 16:50:28 GMT+0000 (Coordinated Universal Time)
cuid: cmgl31jmr000102jm0ds21bvo
slug: recoder-ai-ktore-tlumaczy-python-na-c
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/p5VW_ZUon7o/upload/ddcb035054221c57a8541f5b09bc8b7e.jpeg
tags: ai, python, translate, huggingface, gradio, llm

---

### Zbudowałem AI, które tłumaczy Python na C++ i... jest SZYBCIEJ! 🚀

Cześć wszystkim! 👋 Dzisiaj chciałbym Was zabrać w podróż po jednym z moich ostatnich projektów: narzędziu, które wykorzystuje magię sztucznej inteligencji do tłumaczenia kodu z Pythona na C++. Ale to nie wszystko! Aplikacja od razu sprawdza, o ile szybszy stał się nasz kod. Brzmi ciekawie? Zaczynajmy!

#### Dlaczego w ogóle tłumaczyć Python na C++? 🤔

Każdy, kto programuje, wie, że **Python jest fantastyczny** – prosty, czytelny i ma ogromną społeczność. Idealny do szybkiego tworzenia prototypów i rozwijania projektów. Ma jednak jedną wadę: w porównaniu do języków kompilowanych, takich jak C++, bywa... wolniejszy. 🐢

Z drugiej strony mamy **C++ – demona prędkości**. Jest bliżej "metalu", co daje mu niesamowitą wydajność, ale jego składnia jest o wiele bardziej złożona. Pomyślałem: a co, gdyby połączyć prostotę Pythona z wydajnością C++? I tak narodził się pomysł na translator AI!

#### Krok 1: Plan i technologia 📝

Każdy dobry projekt zaczyna się od planu. Mój, zapisany w pliku [`PLAN.md`](http://PLAN.md), zakładał kilka faz: od konfiguracji, przez logikę tłumaczenia, benchmarking, aż po interfejs użytkownika i testy.

Oto stos technologiczny, na który się zdecydowałem:

* **Języki**: Python 3 i C++
    
* **Mózg operacji (AI)**: Model `deepseek-ai/deepseek-coder-6.7b-instruct` z Hugging Face. To potężne narzędzie zoptymalizowane do zadań programistycznych.
    
* **Interfejs użytkownika**: Biblioteka **Gradio**, która pozwala w banalnie prosty sposób tworzyć interaktywne aplikacje webowe dla modeli AI.
    
* **Optymalizacja**: `bitsandbytes` i `accelerate` do wydajnego ładowania modelu, nawet na słabszym sprzęcie.
    

#### Proces Tworzenia z Wykorzystaniem AI 🤖

Warto zaznaczyć, że projekt ten powstał przy intensywnym wsparciu narzędzi opartych na sztucznej inteligencji. Głównym środowiskiem programistycznym był **Cursor** – edytor kodu zintegrowany z modelami językowymi. Taki model pracy, określany czasem jako "AI-assisted development", pozwala na znaczące przyspieszenie procesu tworzenia oprogramowania. Zamiast koncentrować się na implementacji niskopoziomowych detali, mogłem skupić się na architekturze i logice systemu, delegując zadania takie jak generowanie standardowego kodu, refaktoryzacja czy debugowanie do asystenta AI.

#### Krok 2: Jak to działa pod maską? ⚙️

Sercem aplikacji jest funkcja `translate_and_benchmark` w pliku [`main.py`](http://main.py). Prześledźmy jej działanie krok po kroku:

##### 1\. Ładujemy model AI

Na początku musimy załadować nasz "mózg". Funkcja `load_model()` dba o to, żeby model i tokenizer były ładowane tylko raz. Używamy tu kwantyzacji (ładowanie w 4 bitach), aby zaoszczędzić VRAM karty graficznej.

```python
# main.py
def load_model():
    """Ładuje model i tokenizer tylko gdy są potrzebne."""
    global model, tokenizer
    if model is not None and tokenizer is not None:
        return model, tokenizer

    model_id = "deepseek-ai/deepseek-coder-6.7b-instruct"
    
    bnb_config = transformers.BitsAndBytesConfig(
        load_in_4bit=True,
        bnb_4bit_quant_type="nf4",
        bnb_4bit_use_double_quant=True,
        bnb_4bit_compute_dtype=torch.bfloat16
    )
    model = transformers.AutoModelForCausalLM.from_pretrained(
        model_id,
        trust_remote_code=True,
        quantization_config=bnb_config
    )
    
    tokenizer = transformers.AutoTokenizer.from_pretrained(model_id, trust_remote_code=True)
    return model, tokenizer
```

##### 2\. Tłumaczymy kod

Bierzemy kod Pythona z edytora Gradio i wrzucamy go do modelu z odpowiednim poleceniem (promptem).

```python
# main.py
prompt = f"""Translate the following Python code to C++.
Provide only the complete and correct C++ code...
Do not include any explanations, comments, a 'main()' function, or any '#include' statements.
\n\nPython code:\n```python\n{python_code}\n```\n\nC++ code:"""

inputs = tokenizer(prompt, return_tensors="pt").to(model.device)
outputs = model.generate(inputs.input_ids, max_new_tokens=512)
generated_code = tokenizer.decode(outputs[0], skip_special_tokens=True)
```

AI przetwarza nasze polecenie i zwraca kod C++.

##### 3\. Sprzątamy po AI

Modele językowe bywają "gadatliwe". Funkcja `_parse_model_output` wyciąga z odpowiedzi modelu sam, czyściutki kod C++ za pomocą wyrażeń regularnych.

```python
# main.py
def _parse_model_output(generated_code: str) -> Optional[str]:
    """Przetwarza surową odpowiedź modelu, aby wyodrębnić czysty kod C++."""
    # Szukaj bloku kodu C++ oznaczonego ```cpp ... ```
    match = re.search(r'```cpp\n(.*?)\n```', generated_code, re.DOTALL)
    if match:
        cpp_code = match.group(1).strip()
        return cpp_code
    
    # ... (awaryjne metody parsowania) ...
    return None
```

##### 4\. Benchmark w Pythonie 🐍

Teraz czas na testy! Najpierw mierzymy, jak szybko oryginalny kod w Pythonie liczy 35. wyraz ciągu Fibonacciego.

```python
# main.py
exec_scope = {}
exec(python_code, exec_scope)
py_func = next(v for v in exec_scope.values() if callable(v)) # Znajdź pierwszą funkcję

start_time_py = time.time()
result_py = py_func(35)
end_time_py = time.time()
python_duration = end_time_py - start_time_py
```

##### 5\. Kompilacja i Benchmark w C++ 🚀

Zapisany kod C++ (`fibonacci.cpp`) jest kompilowany za pomocą `g++` razem z plikiem `main.cpp`, który uruchamia testy.

```python
# main.py
compile_command = ["g++", "-O3", "main.cpp", "fibonacci.cpp", "-o", "benchmark_cpp"]
subprocess.run(compile_command, capture_output=True, text=True)

run_command = ["./benchmark_cpp"]
run_process = subprocess.run(run_command, capture_output=True, text=True)
```

Następnie uruchamiamy skompilowany program i mierzymy czas wykonania.

##### 6\. Porównanie wyników 📊

Na koniec aplikacja zbiera wszystkie dane i wyświetla czytelne podsumowanie.

```python
# main.py
summary = f"Python: {python_duration:.6f}s (wynik: {result_py})\n"
summary += f"C++:      {cpp_duration:.6f}s (wynik: {cpp_result})\n\n"

if cpp_duration > 0 and python_duration > cpp_duration:
    speedup = python_duration / cpp_duration
    summary += f"✅ C++ jest {speedup:.2f} razy szybszy!"
```

#### Interfejs, czyli jak to wygląda? ✨

Dzięki Gradio, interfejs jest prosty i intuicyjny. Zdefiniowanie całego UI to zaledwie kilka linijek kodu:

```python
# main.py
with gr.Blocks() as demo:
    gr.Markdown("# 🤖 Tłumacz Kodu i Benchmark AI")
    with gr.Row():
        with gr.Column():
            py_code_input = gr.Code(label="Kod w Pythonie", language="python")
            run_button = gr.Button("Tłumacz i Porównaj!")
        with gr.Column():
            status_output = gr.Textbox(label="Status")
            cpp_code_output = gr.Code(label="Przetłumaczony kod C++", language="cpp")
            summary_output = gr.Textbox(label="Wyniki Benchmarku")
```

Wystarczy wkleić kod, kliknąć przycisk i patrzeć na magię!

#### Podsumowanie i co dalej?

Ten projekt to świetny przykład, jak można praktycznie wykorzystać duże modele językowe do optymalizacji kodu. Oczywiście, AI nie zawsze jest idealne i czasem wymaga drobnych poprawek, ale potencjał jest ogromny!

Cały projekt jest w pełni udokumentowany, od instrukcji instalacji ([`INSTALL.md`](http://INSTALL.md)), po szczegółowe API ([`API.md`](http://API.md)). Stworzyłem też zestaw testów jednostkowych (`test_`[`main.py`](http://main.py), `test_`[`examples.py`](http://examples.py)), aby upewnić się, że wszystko działa jak należy.

Mam nadzieję, że ten wpis był dla Was ciekawy i może zainspiruje kogoś do własnych eksperymentów z AI. Dajcie znać w komentarzach, co myślicie!

---

**Repozytorium projektu**: [https://github.com/kudzik/recoder-vibe](https://github.com/kudzik/recoder-vibe)