# TracePay — landing page (waiting list)

Statyczna, jednoplikowa landing page zoptymalizowana pod SEO i szybkość.
Zbudowana z makiety `TracePay Landing (standalone).html` — usunięto ~1,5 MB
narzędziowego JS (React/Babel/edytor „Tweaks"), treść jest w czystym HTML
(w pełni indeksowalna), fonty ładowane z Google Fonts bez blokowania renderu.

## Pliki

| Plik | Rola |
|------|------|
| `index.html` | Cała strona (~75 KB): treść + inline CSS + ~7 KB vanilla JS + baner cookies. Bez zależności do zbudowania. |
| `polityka-prywatnosci.html` | Strona polityki prywatności (RODO). |
| `og-image.png` | Social card 1200×630 (Open Graph / Twitter). |
| `favicon.ico` | Favicon (16/32/48 px) dla starszych przeglądarek. |
| `icon.svg` | Logo/favicon (wektorowe). |
| `apple-touch-icon.png` · `icon-192.png` · `icon-512.png` | Ikony PWA / iOS. |
| `site.webmanifest` | Manifest PWA. |
| `robots.txt` | Reguły dla botów + wskazanie sitemapy. |
| `sitemap.xml` | Mapa strony dla Google. |

## ⚠️ Przed publikacją podmień 2 placeholdery

1. **Web3Forms (formularz listy oczekujących)** — w `index.html`:
   ```html
   <input type="hidden" name="access_key" value="YOUR_WEB3FORMS_ACCESS_KEY">
   ```
   Załóż darmowy klucz na https://web3forms.com (podajesz tylko e-mail, na który
   mają spływać zapisy) i wklej go w `value`. Bez tego formularz nie wyśle danych.

2. **Google Search Console (token weryfikacji)** — w `index.html`:
   ```html
   <meta name="google-site-verification" content="PODMIEN_NA_TOKEN_Z_GSC">
   ```
   (patrz sekcja GSC niżej). Jeśli weryfikujesz domenę przez DNS — możesz ten meta tag usunąć.

3. **Dane firmy w polityce prywatności** — w `polityka-prywatnosci.html` uzupełnij sekcję
   „Administrator danych": `[NAZWA FIRMY / IMIĘ I NAZWISKO]`, `[ADRES]`, `[NIP]` oraz
   ewentualnie adres e-mail kontaktowy (domyślnie `hej@tracepay.pl`).

> Domena w całym projekcie ustawiona na `https://tracepay.pl`. Jeśli się zmieni,
> podmień `tracepay.pl` w `index.html` (canonical, og:url, hreflang), `polityka-prywatnosci.html`,
> `sitemap.xml` i `robots.txt`.

### Cookies i analityka

Strona ma baner zgody (RODO): domyślnie ładują się tylko niezbędne pliki. Analityka uruchamia się
**dopiero po kliknięciu „Akceptuję wszystkie"**. Aby podłączyć GA4, odkomentuj i uzupełnij `GA_ID`
w funkcji `loadAnalytics()` w inline-skrypcie `index.html` — reszta (zgoda + event `waitlist_signup`)
zadziała automatycznie.

## Wdrożenie

Dowolny hosting statyczny — wgraj zawartość katalogu do roota domeny.

- **Netlify / Vercel / Cloudflare Pages:** przeciągnij folder lub podłącz repo. Zero konfiguracji.
- **Własny serwer (nginx/Apache):** wrzuć pliki do katalogu strony. Włącz gzip/brotli i `Cache-Control` dla `.png/.svg`.

Sprawdź po wdrożeniu, że działają (kod 200):
`tracepay.pl/`, `tracepay.pl/sitemap.xml`, `tracepay.pl/robots.txt`, `tracepay.pl/og-image.png`.

## Podłączenie pod Google Search Console (GSC)

1. Wejdź na https://search.google.com/search-console i dodaj zasób
   **Prefiks URL** = `https://tracepay.pl/` (albo zasób **Domena**, jeśli wolisz weryfikację DNS).
2. Weryfikacja metodą **„Tag HTML"** → skopiuj wartość `content="..."` i wklej w meta
   `google-site-verification` w `index.html`, opublikuj, kliknij **Zweryfikuj**.
3. **Sitemapy** → dodaj `sitemap.xml`.
4. (Opcjonalnie) **Sprawdzanie adresu URL** → `https://tracepay.pl/` → **Poproś o zindeksowanie**.
5. Po kilku dniach sprawdź **Strony** (indeksacja) i **Wyniki z elementami rozszerzonymi**
   (powinny pojawić się FAQ i SoftwareApplication z danych strukturalnych JSON-LD).

## Co jest zrobione pod SEO

- Treść server-side w HTML (bez JS do wyrenderowania) → pełna indeksowalność.
- `<title>` + `meta description` + canonical + `robots` (`max-image-preview:large`).
- Open Graph + Twitter Card z obrazem 1200×630.
- Dane strukturalne JSON-LD: `Organization`, `SoftwareApplication` (z cennikiem), `FAQPage`, `WebSite`.
- `hreflang` (pl + x-default), `lang="pl"`, `theme-color`.
- Poprawna hierarchia nagłówków (1× H1, sekcyjne H2/H3), semantyczne `<section>`, natywne `<details>` w FAQ.
- Wydajność: ~73 KB HTML, zero obrazów rastrowych w treści (UI w CSS), fonty `display=swap` i ładowane niblokująco.

## Analityka (opcjonalnie)

JS formularza wywołuje `gtag('event','waitlist_signup', …)` przy udanym zapisie — jeśli
wkleisz GA4 (gtag.js), konwersje „zapis na listę" policzą się automatycznie.
