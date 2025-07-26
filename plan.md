# Plan migracji FalconsRevenge do Angular + TypeScript

## 1. Stan obecny projektu
- **HTML:** Jeden główny plik `FalconsEscape.html` w katalogu głównym. Zawiera całą strukturę gry, ładowanie stylów, SVG, oraz miejsce na osadzenie gry.
- **JavaScript:** 20 plików JS w katalogu `js/`, podzielonych na podkatalogi (`draw/`, `spawn/`, `update/`). Plik `index.js` pełni rolę „bootstrappera” gry – inicjuje zdarzenia, ładuje obrazy, steruje menu i pętlą gry.

## 2. Główne kroki refaktoryzacji do Angular + TypeScript

### a) Struktura projektu Angular
- Utwórz nową aplikację Angular (`ng new FalconsRevenge`).
- Katalogi:
  - `src/app/components` – komponenty UI (menu, HUD, ekrany końca gry, itp.)
  - `src/app/services` – logika gry, zarządzanie stanem, obsługa zdarzeń.
  - `src/assets` – obrazy, SVG, pliki statyczne.
  - `src/styles` – style globalne, Tailwind (możesz zintegrować z Angularem).

### b) Podział na komponenty Angular
- **GameAreaComponent** – główna plansza gry (canvas).
- **MenuComponent** – menu startowe, pauzy, instrukcji.
- **HudComponent** – wyświetlanie wyniku, życia, power-upów.
- **GameOverComponent** – ekran końca gry.
- **CutsceneComponent** – obsługa cutscenek.
- **SVGSpriteComponent** – ładowanie i zarządzanie SVG.

### c) Logika gry jako serwisy
- **GameStateService** – zarządzanie stanem gry, poziomami, resetami.
- **InputService** – obsługa klawiatury, myszki.
- **EnemyService** – zarządzanie przeciwnikami.
- **DrawService** – renderowanie na canvas.
- **AudioService** – efekty dźwiękowe (jeśli są).

### d) Konwersja JS do TypeScript
- Każdy plik JS zamień na plik `.ts` i stopniowo typuj zmienne, parametry funkcji i zwracane wartości.
- Zamiast globalnych zmiennych – używaj serwisów Angulara i dependency injection.
- Zamiast `document.getElementById` – korzystaj z referencji Angulara (`@ViewChild`, input/output).

### e) Integracja canvas
- Canvas powinien być osadzony w dedykowanym komponencie.
- Logika rysowania i pętla gry mogą być obsługiwane przez RxJS (`requestAnimationFrame` jako observable) i serwis.

### f) Zarządzanie stanem
- Rozważ użycie `@ngrx/store` lub prostego serwisu do zarządzania stanem gry.

## 3. Propozycja folderów i plików

```
src/
  app/
    components/
      game-area/
      menu/
      hud/
      game-over/
      cutscene/
      svg-sprite/
    services/
      game-state.service.ts
      input.service.ts
      enemy.service.ts
      draw.service.ts
      audio.service.ts
    models/
      enemy.model.ts
      player.model.ts
      powerup.model.ts
      level.model.ts
    assets/
      images/
      svg/
      ...
    styles/
      tailwind.css
      ...
```

## 4. Migracja krok po kroku

1. **Stwórz szkielet Angulara i przenieś HTML do komponentów.**
2. **Przenieś logikę JS do serwisów, konwertując do TypeScript.**
3. **Zintegruj canvas i rendering z Angularem.**
4. **Zamień obsługę DOM na Angularowe mechanizmy (eventy, referencje).**
5. **Dodaj typowanie i refaktor kodu pod TypeScript.**
6. **Testuj inkrementalnie – uruchamiaj grę po każdym etapie migracji.**

## 5. Dodatkowe wskazówki

- Angular wymusza modularność – każdy większy element gry powinien być osobnym komponentem/serwisem.
- Unikaj bezpośredniej manipulacji DOM – zawsze korzystaj z API Angulara.
- Rozbij duże pliki JS na mniejsze, logiczne moduły.
- Dobrą praktyką jest najpierw „owinąć” starą logikę w Angulara, a potem stopniowo refaktorować pod idiomy Angular/TS.

---

**Jeśli chcesz, mogę przygotować szczegółowy plan migracji lub przykładowy kod startowy dla jednego z komponentów/serwisów. Daj znać, co dalej!**