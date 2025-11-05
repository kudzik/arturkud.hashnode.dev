---
title: "💻 Pierwszy Projekt Fluttera: Struktura i Przygotowanie Narzędzi"
seoTitle: "Flutter Project Basics: Tools and Structure"
seoDescription: "Stwórz swój pierwszy projekt Fluttera, korzystając z naszego przewodnika po strukturze projektu i narzędziach, w tym Visual Studio Code"
datePublished: Wed Nov 05 2025 19:53:21 GMT+0000 (Coordinated Universal Time)
cuid: cmhmf0w0p000102gz4du48ief
slug: pierwszy-projekt-fluttera-struktura-i-przygotowanie-narzedzi
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/SymZoeE8quA/upload/9f89e99dde19b4f2ea715b65112e3b19.jpeg
tags: flutter

---

Po pomyślnym zainstalowaniu Flutter SDK i narzędzi specyficznych dla platform, przechodzimy do najważniejszego etapu: **inicjacji pierwszego projektu**. Jest to moment, w którym po raz pierwszy zetkniemy się z kodem.

### 1\. Inicjowanie Projektu z Terminala

Zarządzanie projektami we Flutterze rozpoczynamy zawsze od konsoli (terminala/PowerShella), używając specjalnego narzędzia `flutter`.

#### A. Przygotowanie Folderu Projektów

Zaleca się stworzenie jednego centralnego folderu, w którym będziemy trzymać wszystkie nasze projekty.

* **Lokalizacja:** Wybierz dowolną wygodną lokalizację.
    
* **Nazewnictwo:** Przy nadawaniu nazwy folderowi (np. `flutter_projects`) **zawsze używaj podkreślników (**`_`) zamiast spacji lub myślników (`-`) do oddzielania słów. Jest to konwencja, której należy przestrzegać w nazwach folderów związanych z Flutterem.
    

#### B. Tworzenie Projektu

1. **Nawigacja:** Otwórz terminal i przejdź do folderu `flutter_projects` (lub jakkolwiek go nazwałeś).
    
2. **Polecenie:** Użyj polecenia `flutter create`, po którym podajesz nazwę nowej aplikacji:
    
    Bash
    
    ```plaintext
    flutter create nazwa_twojego_projektu
    ```
    
    > **Przykład:** `flutter create first_app`
    
3. **Ważna Zasada Nazewnictwa:** Podobnie jak w przypadku folderu nadrzędnego, **nazwa projektu (np.** `first_app`) musi używać tylko małych liter, cyfr oraz podkreślników (`_`) – nigdy spacji ani myślników.
    

Wykonanie tego polecenia tworzy wewnątrz głównego folderu nowy podfolder (`first_app`) zawierający pełną, działającą, przykładową aplikację Fluttera.

---

### 2\. Edytor Kodu: Wybór Narzędzia Pracy

Chociaż Android Studio oferuje możliwość pisania kodu Fluttera, zdecydowanie poleca się **Visual Studio Code (VS Code)**. Jest to darmowy, lekki i niezwykle wydajny edytor kodu, dostępny dla Windowsa, macOS i Linuxa.

#### A. Otwarcie Projektu w VS Code

1. Po zainstalowaniu VS Code, otwórz go.
    
2. Użyj opcji **"Open Folder"** (`Otwórz Folder`).
    
3. Wybierz nowo utworzony folder projektu (w naszym przykładzie `first_app`).
    

VS Code wyświetli strukturę Twojego projektu, umożliwiając wygodną pracę ze wszystkimi plikami i folderami.

#### B. Obowiązkowe Rozszerzenie

Aby Visual Studio Code w pełni wspierał rozwój we Flutterze, musisz zainstalować jedno kluczowe rozszerzenie: **Rozszerzenie Flutter**.

* **Jak znaleźć:** Przejdź do zakładki rozszerzeń (Extensions, ikona kostki).
    
* **Instalacja:** Wyszukaj i zainstaluj oficjalne **rozszerzenie Flutter** (dostarczone przez zespół Flutter).
    

To rozszerzenie jest absolutną koniecznością. Zapewnia ono podświetlanie składni Darta, autouzupełnianie, narzędzia do debugowania i wiele innych funkcji, które drastycznie zwiększają produktywność podczas pracy z Dartem i Flutterem.

## 🚀 Uruchamiamy Maszyny! Pierwsza Aplikacja Fluttera i Magia Hot Reload

Skoro mamy już skonfigurowany VS Code i stworzony nowy projekt, nadszedł czas, aby zobaczyć efekty naszej pracy. Ten etap polega na uruchomieniu domyślnej aplikacji, a następnie wprowadzeniu w niej pierwszej, dynamicznej zmiany.

### 1\. Przygotowanie Kodu Startowego

Zanim uruchomisz aplikację, musisz upewnić się, że masz poprawny kod początkowy, aby móc śledzić kurs.

* **Lokalizacja Kodu:** W każdym projekcie Fluttera kluczowy kod aplikacji znajduje się w folderze `lib`. Głównym plikiem jest `main.dart`.
    

> **Pamiętaj:** Na razie nie przejmuj się tym, co jest w pliku `main.dart`. Wkrótce omówimy ten kod linijka po linijce, ucząc się Darta i Fluttera od podstaw.

### 2\. Uruchamianie Wirtualnego Urządzenia (Emulatora)

Aby aplikacja mogła działać, musi mieć cel – wirtualne lub fizyczne urządzenie.

#### A. Wygodne Uruchamianie Emulatora z VS Code

Zamiast przechodzić do Android Studio czy Xcode, możemy użyć VS Code do łatwego uruchomienia emulatora Androida lub symulatora iOS.

1. Otwórz **Paletę Poleceń** (View &gt; Command Palette lub użyj skrótu, np. `Ctrl+Shift+P` na Windowsie).
    
2. Wpisz: `flutter` i zacznij pisać `emulator`.
    
3. Wybierz opcję `Flutter: Launch Emulator` i wybierz stworzone wcześniej wirtualne urządzenie (np. emulator Androida).
    

> **Wskazówka:** W trakcie kursu często będziemy polegać na **emulatorze Androida**, ponieważ działa on na wszystkich głównych systemach operacyjnych (Windows, macOS, Linux), zapewniając spójność przykładów dla wszystkich użytkowników.

#### B. Wybór Urządzenia Docelowego

Po uruchomieniu wirtualnego urządzenia, spójrz na **Pasek Stanu** (Status Bar) na dole VS Code. Powinien tam być widoczny **wybrany emulator**. Możesz na niego kliknąć, aby zmienić cel (np. na przeglądarkę webową, aplikację desktopową lub inne urządzenie mobilne).

### 3\. Uruchamianie Aplikacji Fluttera

Mając otwarty plik `main.dart` i wybrane urządzenie docelowe, możesz uruchomić aplikację na trzy sposoby:

1. **Górny Pasek (Run/Uruchom):** Naciśnij opcję `Run` (Uruchom) widoczną tuż nad funkcją `main()` w pliku `main.dart`.
    
2. **Terminal:** Otwórz terminal w VS Code i wpisz polecenie: `flutter run`.
    
3. **Główny Pasek Menu:** Przejdź do **Run** i wybierz `Run Without Debugging` (Uruchom bez debugowania). Ta opcja jest często preferowana, ponieważ startuje aplikację bez dodatkowego obciążenia procesem debugowania.
    

Po uruchomieniu, Flutter zacznie kompilować kod i przekazywać go na wirtualne urządzenie. Po chwili zobaczysz prostą, ale działającą aplikację mobilną!

### 4\. Magia Hot Reload

Jedną z najbardziej kochanych przez deweloperów funkcji Fluttera jest **Hot Reload** (Gorące Przeładowanie).

#### Czym jest Hot Reload?

To mechanizm, który pozwala na **natychmiastowe odzwierciedlenie zmian w kodzie w uruchomionej aplikacji**, bez konieczności jej restartowania. Jest to klucz do szybkiego i komfortowego rozwoju.

**Jak to działa?**

1. **Zmień Kod:** W pliku `main.dart` odszukaj widget tekstowy (np. `Text('Flutter Demo')`).
    
2. **Wprowadź Zmianę:** Zmień tekst na np. `Text('Flutter - Hello World')`.
    
3. **Zapisz Plik:** Naciśnij **Zapisz** (`Ctrl+S` lub `Command+S`).
    

Po zapisaniu pliku, rozszerzenie Fluttera w VS Code natychmiast wykryje zmianę i prześle ją do aplikacji, aktualizując interfejs użytkownika w ciągu ułamka sekundy.

> **Ważne:** Jeśli Hot Reload (ikona błyskawicy ⚡) nie zadziała (co zdarza się rzadko przy bardziej złożonych zmianach stanu), możesz użyć opcji **Hot Restart** (ikona restartu 🔄), która uruchomi aplikację od nowa, ale znacznie szybciej niż pełny restart kompilacji.

Możliwość natychmiastowego zobaczenia zmian drastycznie przyspiesza pracę i sprawia, że tworzenie interfejsów użytkownika jest bardziej interaktywne i przyjemne.