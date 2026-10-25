# simplysmart

> **Repozytorium zarchiwizowane.** Po wersji 0.3.0 rdzeń jest częścią paczki
> [`simplysmart/admiro`](https://github.com/marcinulezalka/simply-admiro) (razem ze strefą administratorów
> i narzędziami deweloperskimi); historia tego repozytorium jest tam scalona. Zamień w `composer.json`
> `simplysmart/mainboard` na `simplysmart/admiro`; mapę zmian nazw (namespace, komponenty, config, zmienne,
> komendy, trasy) opisuje `CHANGELOG.md` paczki admiro.

Modułowa płyta główna (mainboard) dla aplikacji simplysmart: podłączanie modułów, przestrzenie robocze do zarządzania i więcej.

Paczka Composer: `simplysmart/mainboard`, namespace `Simplysmart\Mainboard\`.

> Projekt na wczesnym etapie: `public/index.php` to na razie placeholder wygenerowany przez
> [Smartboard](https://smartboard.local).

## Wymagania

- PHP ^8.4
- Laravel 13 (`illuminate/support` ^13.0); provider `Simplysmart\Mainboard\MainboardServiceProvider` rejestruje się sam (auto-discovery)
- Pakiety zewnętrzne: zob. [docs/zaleznosci.md](docs/zaleznosci.md)

## Tłumaczenia i formatowanie

Helpery dla kodu i widoków Blade (`{{ text_action('save') }}`). Teksty szukane są kaskadowo:
**paczka strefy → mainboard → `lang/` aplikacji**, tylko w bieżącym języku. Gdy klucza nie ma
nigdzie, helper zwraca sam klucz (np. `actions.save`), a nie tekst w innym języku, więc od razu
widać brak tłumaczenia.

| Helper | Klucz | Przykład |
| --- | --- | --- |
| `text($key)` | dowolny, z namespace paczki lub bez | `text('zone::dashboard.title')` |
| `text_action($key)` | `actions.<key>` | `text_action('save')` → „Zapisz” |
| `text_data($model, $column)` | `data.<model>.<kolumna>`, potem `data.<kolumna>` | `text_data($user, 'first_name')` |
| `text_file($key)` | `files.<key>` (eksport CSV, Excel, PDF) | `text_file('headers.email')` |
| `text_email($key)` | `emails.<key>` | `text_email('welcome.subject', ['name' => $name])` |
| `local_money($kwota, ?$waluta)` | waluta domyślna: `APP_CURRENCY` (PLN) | `1 234,50 zł` |
| `local_number($liczba, $precyzja)` | | `9 876 543,21` |
| `local_date($data, ?$format)` | format ISO Carbona, domyślnie `L` | `20.10.2026`, `D MMMM YYYY` → „20 października 2026” |
| `local_day($dzień)` / `local_month($miesiąc)` | dzień ISO 1–7 / miesiąc 1–12 albo data | „poniedziałek”, „marzec” |

Namespace paczki przechodzi przez grupę: `text_email('zone::welcome.subject')` szuka
`zone::emails.welcome.subject`, potem `mainboard::emails.welcome.subject`, potem `emails.welcome.subject`.
Teksty Core (np. `lang/pl/actions.php`) aplikacja zmienia plikiem `lang/vendor/mainboard/<locale>/<plik>.php`,
a teksty paczki strefy plikiem `lang/vendor/<paczka>/<locale>/<plik>.php`. Kontrakty:
`Contracts\TextTranslator` i `Contracts\LocalFormatter`.

## Dokumentacja

| Dokument | Zawartość |
| --- | --- |
| [Struktura projektu](docs/struktura.md) | katalogi i ich rola |
| [Wdrożenie](docs/wdrozenie.md) | hosting współdzielony: co wysyłać, a czego nie |
| [Baza danych i modele](docs/baza-danych.md) | koncepcja schematu, modeli, historii i tłumaczeń danych |
| [Design system](docs/design-system.md) | koncepcja wyglądu interfejsu, tokenów i komponentów |
| [Pakiety zewnętrzne](docs/zaleznosci.md) | zależności Composer, licencje, zasady dodawania |
| [Marka](brand/README.md) | logo, sygnet, kolory, zasady użycia |
| [CONTRIBUTING.md](CONTRIBUTING.md) | zasady rozwoju i wydawania wersji |
| [CHANGELOG.md](CHANGELOG.md) | historia zmian |

Pełny spis: [docs/README.md](docs/README.md).

## Licencja

Simplysmart Restricted Source License 1.0 — © 2026 Marcin Ulezalka. Do celów niekomercyjnych
kod możesz bezpłatnie przeglądać, kopiować, uruchamiać i modyfikować. Użycie komercyjne (także
w projektach dla klientów) jest dozwolone po wpisaniu deklaracji do pliku `THIRD_PARTY.md` w
Twoim projekcie — wzór wpisu jest w [THIRD_PARTY.md](THIRD_PARTY.md). Sprzedaż,
sublicencjonowanie i komercyjna redystrybucja kodu jako produktu wymagają pisemnej zgody autora.
Kopie muszą zachować informację o prawach autorskich i treść licencji. Wiążąca jest angielska
treść pliku [LICENSE](LICENSE).
