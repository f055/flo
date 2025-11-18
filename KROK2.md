# Krok 2: Kluczowe Przepływy Użytkownika i Procesy

Data analizy: 2025-11-14

---

## 1. Proces Rejestracji i Logowania

### 1.1 Rejestracja (Onboarding)

**Strona rejestracji:** https://floowe.leaware.com/register

#### Prawdopodobny przepływ (standard SaaS):
```
1. STRONA GŁÓWNA (floowe.com)
   ↓
   [Kliknięcie CTA: "Przetestuj za darmo"]
   ↓

2. FORMULARZ REJESTRACJI
   ↓
   Wymagane pola (typowe):
   - Email *
   - Hasło *
   - Potwierdzenie hasła *
   - Nazwa firmy / Imię
   - Akceptacja regulaminu i RODO *

   Opcje logowania:
   - Email/hasło (standard)
   - Potencjalnie: Google OAuth
   - Potencjalnie: Facebook OAuth
   ↓

3. WERYFIKACJA EMAIL
   ↓
   - Wysyłka email z linkiem aktywacyjnym
   - Kliknięcie link → Aktywacja konta
   ↓

4. WELCOME / ONBOARDING
   ↓
   - Welcome screen z intro video/message
   - Krótki tutorial (product tour)
   ↓

5. INITIAL SETUP
   ↓
   - Wybór branży
   - Wprowadzenie słów kluczowych
   - Połączenie social media (Facebook, LinkedIn, Twitter)
   - Instalacja WordPress plugin (opcjonalnie)
   ↓

6. PIERWSZE UŻYCIE
   ↓
   - Generowanie pierwszego artykułu (guided experience)
   - Edycja treści
   - Publikacja testowa
   ↓

7. DASHBOARD (Główny panel)
```

### 1.2 Logowanie

**Strona logowania:** https://floowe.leaware.com/login (domniemane)

```
Formularz logowania:
┌──────────────────────────────┐
│ Email:    [____________]    │
│ Hasło:    [____________]    │
│                              │
│ [ ] Zapamiętaj mnie          │
│                              │
│ [    ZALOGUJ SIĘ    ]       │
│                              │
│ Zapomniałeś hasła?           │
│ Nie masz konta? Zarejestruj  │
└──────────────────────────────┘

Opcje (prawdopodobne):
- Email + hasło (standard)
- Google OAuth
- Facebook OAuth
- Password reset flow
```

### 1.3 Reset Hasła

```
1. Kliknięcie "Zapomniałeś hasła?"
   ↓
2. Wprowadzenie email
   ↓
3. Wysłanie linku resetującego
   ↓
4. Kliknięcie link w email
   ↓
5. Ustawienie nowego hasła
   ↓
6. Automatyczne logowanie lub redirect do /login
```

---

## 2. Główne Przypadki Użycia (Use Cases)

### Use Case 1: Tworzenie i Publikacja Artykułu na Blog

**Aktorzy:** Użytkownik (właściciel firmy, marketer)

**Cel:** Stworzyć i opublikować artykuł SEO na blog firmowy

**Przepływ:**

```
1. LOGOWANIE DO DASHBOARDU
   ↓
2. WYBÓR "Utwórz nowy artykuł"
   ↓
3. WYBÓR TEMATU
   - System sugeruje popularne tematy branżowe
   - Użytkownik wybiera z listy LUB
   - Użytkownik wpisuje własny temat
   ↓
4. ANALIZA SŁÓW KLUCZOWYCH
   - System analizuje popularne frazy
   - Wyświetla konkurencję i szanse rankingowe
   - Użytkownik zatwierdza słowa kluczowe
   ↓
5. GENEROWANIE TREŚCI (AI)
   - System tworzy artykuł (1-3 min)
   - Optymalizacja SEO automatyczna
   - Struktura: nagłówki, akapity, listy
   ↓
6. EDYCJA TREŚCI
   - Edytor WYSIWYG (Word-like)
   - Użytkownik modyfikuje tekst
   - Dostosowanie tonu komunikacji
   ↓
7. DODANIE GRAFIK
   - Upload własnych zdjęć LUB
   - Wybór z darmowych baz (Unsplash/Pexels) LUB
   - Generowanie AI (opcjonalnie)
   ↓
8. PREVIEW
   - Podgląd jak będzie wyglądać artykuł
   ↓
9. WYBÓR KANAŁÓW PUBLIKACJI
   - [✓] Blog WordPress
   - [✓] Facebook
   - [ ] LinkedIn
   - [ ] Twitter
   ↓
10. HARMONOGRAMOWANIE (opcjonalnie)
    - Publikuj teraz LUB
    - Zaplanuj datę i godzinę
    ↓
11. PUBLIKACJA (1 kliknięcie)
    ↓
12. POTWIERDZENIE
    - Status publikacji (success/fail)
    - Linki do opublikowanych treści
```

**Czas:** 10-15 minut (vs. 1-2 godziny manualnie)

---

### Use Case 2: Social Media Post Series (Kampania)

**Aktorzy:** Użytkownik (social media manager)

**Cel:** Stworzyć serię 5 postów na różne platformy social media

**Przepływ:**

```
1. DASHBOARD → "Nowa kampania" (lub podobna opcja)
   ↓
2. KONFIGURACJA KAMPANII
   - Temat/cel kampanii
   - Liczba postów (5)
   - Platformy (Facebook, LinkedIn, Twitter)
   - Tone of voice
   ↓
3. GENEROWANIE SERII POSTÓW
   - System generuje 5 różnych postów
   - Każdy dostosowany do platformy
   - Różne formaty (pytanie, fact, CTA, story, engagement)
   ↓
4. PRZEGLĄD I EDYCJA
   - Wyświetlenie wszystkich 5 w jednym widoku
   - Bulk editing możliwy
   - Indywidualne dostosowanie każdego
   ↓
5. DODANIE GRAFIK
   - Osobna grafika dla każdego posta LUB
   - Ta sama grafika dla wszystkich
   ↓
6. HARMONOGRAMOWANIE
   - Auto-schedule (system wybiera najlepsze godziny) LUB
   - Manualne ustawienie dat/godzin
   - Rozłożenie w czasie (np. 1 post/dzień przez tydzień)
   ↓
7. ZATWIERDZENIE I PUBLIKACJA
   ↓
8. MONITORING
   - Dashboard pokazuje status publikacji
   - Powiadomienia o udanych publikacjach
```

**Czas:** 15-20 minut dla 5 postów

---

### Use Case 3: SEO Content Optimization (Istniejący Artykuł)

**Aktorzy:** Użytkownik (SEO specialist)

**Cel:** Poprawić ranking istniejącego artykułu w Google

**Przepływ:**

```
1. DASHBOARD → "Optymalizuj istniejący artykuł"
   ↓
2. IMPORT ARTYKUŁU
   - Wklejenie URL (pobierz z WordPress) LUB
   - Upload pliku LUB
   - Copy-paste treści
   ↓
3. ANALIZA SEO
   - System analizuje obecną treść
   - Sprawdza słowa kluczowe
   - Porównuje z top-ranking konkurencją
   ↓
4. REKOMENDACJE
   Wyświetlenie:
   - Brakujące słowa kluczowe
   - Sugerowane nagłówki
   - Optymalna długość treści
   - Sugestie struktury
   - Meta description / title
   ↓
5. AI-ASSISTED REWRITE
   - System proponuje poprawioną wersję
   - Highlight zmian (diff view)
   ↓
6. AKCEPTACJA ZMIAN
   - Użytkownik zatwierdza/odrzuca sugestie
   - Manualna edycja
   ↓
7. PUBLIKACJA UPDATE
   - Nadpisanie istniejącego artykułu na WordPress LUB
   - Zapisanie jako draft do manualnej publikacji
   ↓
8. TRACKING
   - System monitoruje pozycję w Google
   - Porównanie before/after
```

**Czas:** 10-15 minut

---

### Use Case 4: Monthly Content Calendar Planning

**Aktorzy:** Użytkownik (content manager)

**Cel:** Zaplanować i przygotować content na cały miesiąc z wyprzedzeniem

**Przepływ:**

```
1. DASHBOARD → "Content Calendar" / "Harmonogram"
   ↓
2. WYBÓR MIESIĄCA
   - Wybór miesiąca do zaplanowania
   ↓
3. AI CONTENT SUGGESTIONS
   - System analizuje trendy branżowe
   - Sugeruje 20-30 tematów na miesiąc
   - Uwzględnia sezonowość, eventy, święta
   ↓
4. SELEKCJA TEMATÓW
   - Użytkownik wybiera 12 artykułów (dla planu Standard)
   - Przeciągnij i upuść na kalendarz
   ↓
5. AUTO-SCHEDULE
   - System rozkłada artykuły równomiernie
   - Optymalne dni/godziny publikacji
   ↓
6. BULK GENERATION (opcja premium?)
   - "Wygeneruj wszystkie" - AI tworzy wszystkie 12 artykułów
   - Queue system (generowanie po kolei)
   ↓
7. PRZEGLĄD I EDYCJA
   - Lista wszystkich artykułów
   - Status: draft, ready, scheduled, published
   - Bulk editing możliwy
   ↓
8. PUBLIKACJA AUTOMATYCZNA
   - Artykuły publikują się zgodnie z harmonogramem
   - Użytkownik dostaje powiadomienia
   ↓
9. ANALYTICS
   - Po miesiącu: raport z wyników
   - CTR, views, engagement per article
```

**Czas:** 1-2 godziny na zaplanowanie całego miesiąca

---

### Use Case 5: WordPress Blog Setup & First Article

**Aktorzy:** Nowy użytkownik (pierwszy raz z Floowe)

**Cel:** Połączyć Floowe z blogiem WordPress i opublikować pierwszy artykuł

**Przepływ:**

```
1. ONBOARDING (po rejestracji)
   ↓
2. WYBÓR "Połącz stronę WordPress"
   ↓
3. INSTALACJA PLUGINU
   Opcja A - Automatyczna:
   - Podanie URL WordPress + admin credentials
   - System instaluje plugin automatycznie

   Opcja B - Manualna:
   - Download plugin .zip
   - Upload do WordPress (Plugins → Add New → Upload)
   - Aktywacja plugin
   ↓
4. AUTORYZACJA
   - Plugin generuje API key
   - User kopiuje key do Floowe
   - Floowe weryfikuje połączenie
   ↓
5. KONFIGURACJA
   - Wybór domyślnej kategorii postów
   - Wybór autora (WordPress user)
   - Ustawienia SEO (Yoast/Rank Math integration?)
   ↓
6. TEST CONNECTION
   - System publikuje testowy draft post
   - Weryfikacja że działa
   ↓
7. PIERWSZY ARTYKUŁ (guided)
   - "Stwórzmy Twój pierwszy artykuł!"
   - Wybór tematu z sugestii
   - Generowanie treści
   - Szybka edycja
   ↓
8. PUBLIKACJA
   - "Opublikuj na swoim blogu" (1 kliknięcie)
   ↓
9. SUCCESS SCREEN
   - Gratulacje!
   - Link do opublikowanego artykułu
   - "Co dalej?" - sugestie kolejnych kroków
```

**Czas:** 15-20 minut (pierwsze setup)

---

## 3. Przepływ Płatności

### 3.1 Plany i Trigger Płatności

```
Plany:
- Free: 3 artykuły/miesiąc (social media only) - BEZ PŁATNOŚCI
- Basic: 250 zł/msc (promo) / 500 zł/msc (regular) - 8 artykułów
- Standard: 350 zł/msc (promo) / 700 zł/msc (regular) - 12 artykułów
- Premium: 450 zł/msc (promo) / 900 zł/msc (regular) - 20 artykułów
```

### 3.2 Ścieżka Upgrade (Free → Paid)

```
TRIGGER POINTS dla upgrade:
- Wykorzystanie 3/3 darmowych artykułów
- Próba publikacji na website (niedostępne w Free)
- Próba użycia premium feature
- Popup/banner "Upgrade to continue"

UPGRADE FLOW:
1. User klika "Upgrade" / "Wybierz plan"
   ↓
2. STRONA CENNIK
   - Porównanie planów (feature comparison table)
   - Highlight: 50% off przez 3 miesiące
   - Kalkulator ROI / oszczędności
   ↓
3. WYBÓR PLANU
   - Basic / Standard / Premium
   - Toggle: Monthly / Annual (annual = dodatkowa zniżka?)
   ↓
4. FORMULARZ BILLING
   Dane do faktury:
   - Firma / Imię i nazwisko
   - NIP (dla firm)
   - Adres
   - Email do faktury
   ↓
5. WYBÓR METODY PŁATNOŚCI
   Prawdopodobne opcje dla Polski:
   [●] Karta kredytowa/debetowa (Stripe)
   [ ] Przelewy24 (bank transfer)
   [ ] PayU
   [ ] BLIK
   [ ] Przelew tradycyjny
   ↓
6. DANE KARTY (jeśli karta)
   - Numer karty
   - Data ważności
   - CVV
   - Nazwa posiadacza

   Powered by: Stripe (secure, PCI compliant)
   ↓
7. POTWIERDZENIE
   - Podsumowanie zamówienia
   - Cena: 250 zł/miesiąc (50% off przez 3 msc)
   - Potem: 500 zł/miesiąc
   - Recurring billing info
   - [ ] Akceptuję regulamin
   ↓
8. PŁATNOŚĆ
   - Processing... (Stripe payment intent)
   - 3D Secure (jeśli wymagane)
   ↓
9. SUCCESS
   - Płatność potwierdzona
   - Email z fakturą
   - Redirect do dashboardu
   - "Twój plan został aktywowany!"
   ↓
10. UNLOCKED FEATURES
    - Zwiększony limit artykułów
    - Publikacja na website enabled
    - Dostęp do premium features
```

### 3.3 Recurring Billing

```
Automatyczne odnowienie:
- Pierwszy miesiąc: 250 zł (promo)
- Drugi miesiąc: 250 zł (promo)
- Trzeci miesiąc: 250 zł (promo)
- Czwarty miesiąc: 500 zł (REGULAR PRICE)
  ↓
  [⚠️ CRITICAL: Churn risk!]

  Email przypomnienia (7 dni przed):
  "Za tydzień Twój plan odnowi się w regularnej cenie.
   Nadal oszczędzasz [X zł] vs. copywriter!"

Niepowodzenie płatności:
1. Próba obciążenia karty (automatic retry)
   ↓
2. Email: "Payment failed - update your payment method"
   ↓
3. Grace period (3-7 dni)
   ↓
4. Downgrade do Free plan (zachowanie danych)
   ↓
5. Możliwość ponownej aktywacji
```

### 3.4 Faktury i Rozliczenia

```
Generowanie faktur:
- Automatyczne wystawienie po każdej płatności
- Email z PDF faktury
- Panel: "Billing" → Historia faktur
- Download faktur za poprzednie miesiące

Dane na fakturze:
- Floowe Sp. z o.o. (lub inna forma prawna)
- NIP, REGON, Adres
- Numer faktury
- Data wystawienia
- Usługa: "Subskrypcja Floowe - Plan [Basic/Standard/Premium]"
- Kwota netto + VAT 23% = Brutto
```

### 3.5 Anulowanie Subskrypcji

```
1. Panel → Settings → Billing → "Anuluj subskrypcję"
   ↓
2. RETENTION FLOW
   "Przykro nam, że odchodzisz. Możemy pomóc?"

   - Ankieta: Dlaczego anulujesz?
     [ ] Za drogie
     [ ] Nie używam wystarczająco
     [ ] Brak potrzebnych funkcji
     [ ] Przechodzę do konkurencji
     [ ] Inne: ___________
   ↓
3. RETENTION OFFERS
   Jeśli "Za drogie":
   - "Możemy zaoferować 20% zniżki na następne 3 miesiące"

   Jeśli "Nie używam":
   - "Może przejście na niższy plan? (Basic zamiast Standard)"

   [Przyjmij ofertę] [Anuluj mimo to]
   ↓
4. POTWIERDZENIE ANULOWANIA
   "Na pewno chcesz anulować?"
   - Twoja subskrypcja będzie aktywna do [data końca okresu]
   - Potem downgrade do Free plan (3 artykuły/msc)
   - Twoje dane będą zachowane

   [Zatrzymaj subskrypcję] [Tak, anuluj]
   ↓
5. ANULOWANIE CONFIRMED
   - Email potwierdzający
   - "Będziemy za Tobą tęsknić"
   - "Możesz wrócić w każdej chwili"
```

---

## 4. Integracje z Systemami Zewnętrznymi

### 4.1 Social Media Platforms

#### Facebook Integration

```
SETUP FLOW:
1. Dashboard → "Połącz konta" → Facebook
   ↓
2. OAuth Authorization
   - Redirect do Facebook
   - "Floowe chce uzyskać dostęp do:"
     • Zarządzanie stronami
     • Publikowanie w Twoim imieniu
     • Dostęp do insights
   - [Zaloguj się przez Facebook]
   ↓
3. WYBÓR STRON
   - Lista wszystkich stron, którymi zarządza user
   - Checkbox selection (multi-select możliwe)
   - [Zatwierdź]
   ↓
4. POŁĄCZENIE AKTYWNE
   - Status: "Połączono z [nazwa strony]"
   - Możliwość publikacji

API ENDPOINTS (Facebook Graph API):
- POST /me/accounts - lista stron użytkownika
- POST /{page-id}/feed - publikacja posta
- GET /{page-id}/insights - analytics

PERMISSIONS REQUIRED:
- pages_manage_posts
- pages_read_engagement
- publish_to_groups (opcjonalnie)

TOKEN MANAGEMENT:
- Refresh token co 60 dni
- Automatyczne odnowienie
- Powiadomienie jeśli wygasł
```

#### LinkedIn Integration

```
SETUP FLOW:
1. Dashboard → "Połącz konta" → LinkedIn
   ↓
2. OAuth Authorization
   - Redirect do LinkedIn
   - Scope: w_member_social, r_organization_social
   ↓
3. WYBÓR PROFILU
   - Personal profile LUB
   - Company page (jeśli admin)
   ↓
4. POŁĄCZENIE AKTYWNE

API ENDPOINTS (LinkedIn API v2):
- POST /ugcPosts - publikacja posta
- GET /organizations - lista company pages
- GET /organizationalEntityShareStatistics - analytics

LIMITATIONS:
- Rate limits: 100 requests/day (personal), 500/day (company)
- Character limits: 3,000 characters
```

#### Twitter/X Integration

```
SETUP FLOW:
1. Dashboard → "Połącz konta" → Twitter
   ↓
2. OAuth 2.0 Authorization
   - Twitter login
   - Approve permissions
   ↓
3. POŁĄCZENIE AKTYWNE

API ENDPOINTS (Twitter API v2):
- POST /tweets - publikacja tweeta
- GET /users/me - user info
- POST /tweets (with media) - tweet z obrazem

LIMITATIONS:
- Character limit: 280 (lub więcej dla premium accounts)
- Rate limits: 300 tweets/3h
```

---

### 4.2 CMS Integration (WordPress)

#### WordPress Plugin Architecture

```
INSTALLATION OPTIONS:

Opcja A - Automatyczna:
1. User podaje WordPress URL + credentials
2. Floowe używa XML-RPC lub REST API
3. Plugin instaluje się automatycznie
4. Aktywacja

Opcja B - Manualna:
1. Download plugin z Floowe
2. Upload do WordPress (Plugins → Add New → Upload)
3. Activate
4. Settings → API Key generation
5. Copy API key do Floowe dashboard
```

#### WordPress REST API Integration

```
ENDPOINTS UŻYWANE:

Authentication:
- Application Passwords (WordPress 5.6+)
- JWT tokens

Publishing:
- POST /wp-json/wp/v2/posts - create new post
  Body:
  {
    "title": "Article Title",
    "content": "<p>HTML content...</p>",
    "status": "publish" | "draft",
    "categories": [1, 2],
    "featured_media": 123,
    "meta": {
      "seo_title": "...",
      "seo_description": "..."
    }
  }

- GET /wp-json/wp/v2/categories - lista kategorii
- POST /wp-json/wp/v2/media - upload obrazu
- PUT /wp-json/wp/v2/posts/{id} - update existing post

SEO PLUGINS INTEGRATION:
- Yoast SEO: custom fields dla meta title/description
- Rank Math: API dla SEO data
- All in One SEO: meta fields support
```

#### Security & Authentication

```
API Security:
- SSL/TLS required (HTTPS)
- Application Password lub JWT
- Whitelisted IP addresses (optional)
- Rate limiting

Data Flow:
Floowe → HTTPS POST → WordPress REST API → Database
```

---

### 4.3 Grafika i Media

#### Stock Photos Integration

```
UNSPLASH API:
- Search photos by keyword
- Download high-res images
- Attribution automatic

Endpoint:
GET https://api.unsplash.com/search/photos?query={keyword}

PEXELS API:
- Similar functionality
- Free to use

Integration w edytorze:
1. User klika "Dodaj grafikę" → "Stock photos"
2. Wpisuje keyword
3. Galeria wyników z Unsplash + Pexels
4. Wybór zdjęcia → automatyczny download i insert
```

#### AI Image Generation (Opcjonalnie)

```
DALL-E / Midjourney / Stable Diffusion API:
- User opisuje jaki obraz chce
- AI generuje obraz
- Preview i akceptacja
- Insert do artykułu

Limitations:
- Koszt per image generation
- Może być paid add-on
```

---

### 4.4 Analytics & Tracking

#### Google Analytics Integration

```
SETUP:
1. Dashboard → Settings → Integrations → Google Analytics
2. OAuth login Google
3. Wybór property
4. Connected

DATA SYNCED:
- Pageviews dla published articles
- Bounce rate
- Time on page
- Traffic sources

DISPLAY:
- Per-article analytics dashboard
- "Your article has 1,234 views"
```

#### Google Search Console Integration

```
SETUP:
1. Settings → Integrations → Google Search Console
2. OAuth
3. Wybór property

DATA SYNCED:
- Keyword rankings
- Impressions in search
- Click-through rate
- Average position

DISPLAY:
- SEO Performance dashboard
- "Your article ranks #5 for [keyword]"
- Before/after optimization comparison
```

---

### 4.5 Payment Gateway Integration

#### Stripe

```
INTEGRATION TYPE:
- Stripe Checkout (hosted payment page) LUB
- Stripe Elements (embedded forms)

FEATURES USED:
- Subscriptions (recurring billing)
- Payment Intents
- Webhooks dla payment events
- Customer Portal (manage subscription)

WEBHOOKS HANDLED:
- payment_succeeded → Activate subscription
- payment_failed → Send email, retry
- customer.subscription.deleted → Downgrade to Free
- invoice.payment_succeeded → Send invoice email
```

#### Przelewy24 (through Stripe)

```
Polish payment method aggregator
- Integrated via Stripe
- Supports bank transfers, BLIK, cards
- Currency: PLN lub EUR
- Auto-redirect flow
```

---

### 4.6 Email & Notifications

#### Transactional Email Service

```
Prawdopodobnie używane:
- SendGrid LUB
- Amazon SES LUB
- Mailgun

EMAIL TYPES:
- Welcome email (po rejestracji)
- Email verification
- Password reset
- Payment confirmation
- Invoice delivery
- Monthly usage summary
- Failed payment warning
- Subscription renewal reminder
- Article published notification
```

#### Push Notifications (opcjonalnie)

```
W aplikacji web:
- Browser notifications (Web Push API)
- "Your article was published successfully"
- "You have 2 articles left this month"
```

---

### 4.7 CRM Integration (przyszłość)

**Potencjalne integracje:**

```
HubSpot:
- Sync contacts
- Track content engagement
- Lead scoring based on article interactions

Salesforce:
- Customer data sync
- Content performance in CRM

Mailchimp/GetResponse:
- Newsletter generation
- Automated email campaigns z published articles
```

---

### 4.8 Zapier/Make Integration (przyszłość)

```
Możliwości:
- Trigger: "New article published" → Action: Post to Slack
- Trigger: "Article reaches 1000 views" → Action: Send email
- Trigger: "Monthly limit reached" → Action: Create task in Trello

ZAPIER TRIGGERS (potential):
- New article created
- Article published
- Article updated
- Monthly report ready

ZAPIER ACTIONS (potential):
- Create article
- Schedule article
- Get analytics
```

---

## 5. Przepływ Danych - Architektura

### 5.1 High-Level Data Flow

```
┌─────────────┐
│   USER      │
│  (Browser)  │
└──────┬──────┘
       │
       ↓
┌──────────────────────────────────┐
│   FLOOWE FRONTEND (SPA)          │
│   React/Vue + UI Components      │
└──────────────┬───────────────────┘
               │
               ↓ HTTPS/REST API
┌──────────────────────────────────┐
│   FLOOWE BACKEND                 │
│   (.NET Core / Node.js)          │
├──────────────────────────────────┤
│  • Authentication Service        │
│  • Content Generation Service    │
│  • Publishing Service            │
│  • Analytics Service             │
│  • Billing Service               │
└──────┬───────────────────────────┘
       │
       ├─────────────┬──────────────┬───────────────┐
       ↓             ↓              ↓               ↓
┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐
│PostgreSQL│  │  OpenAI  │  │  Stripe  │  │  Social  │
│          │  │   API    │  │   API    │  │   APIs   │
│(Database)│  │          │  │          │  │          │
└──────────┘  └──────────┘  └──────────┘  └──────────┘
                                                │
                             ┌──────────────────┼────────────────┐
                             ↓                  ↓                ↓
                      ┌──────────┐      ┌──────────┐    ┌──────────┐
                      │LinkedIn  │      │ Twitter  │    │Facebook  │
                      │   API    │      │   API    │    │   API    │
                      └──────────┘      └──────────┘    └──────────┘
```

### 5.2 Content Creation Flow (Technical)

```
1. User Request: "Create article on [topic]"
   ↓
2. Backend → OpenAI API:
   POST https://api.openai.com/v1/chat/completions
   {
     "model": "gpt-4",
     "messages": [
       {"role": "system", "content": "You are an SEO copywriter..."},
       {"role": "user", "content": "Write article about [topic]..."}
     ]
   }
   ↓
3. OpenAI Response (AI-generated content)
   ↓
4. Backend Processing:
   - SEO optimization (keyword injection)
   - Formatting (headings, lists)
   - Meta data generation
   ↓
5. Save to Database (status: draft)
   ↓
6. Return to Frontend → Display in editor
   ↓
7. User edits
   ↓
8. User clicks "Publish"
   ↓
9. Backend → Publishing Service:
   - Parallel API calls:
     * WordPress REST API
     * Facebook Graph API
     * LinkedIn API
     * Twitter API
   ↓
10. Status updates → Database
    ↓
11. Notifications → User
```

---

## 6. Podsumowanie - Kluczowe Przepływy

### 6.1 Critical User Journeys

| Journey | Frequency | Importance | Complexity |
|---------|-----------|------------|------------|
| Rejestracja + Onboarding | Once | ⭐⭐⭐⭐⭐ | Medium |
| Pierwsze połączenie WordPress | Once | ⭐⭐⭐⭐⭐ | High |
| Tworzenie artykułu | Weekly | ⭐⭐⭐⭐⭐ | Low |
| Publikacja multi-channel | Weekly | ⭐⭐⭐⭐⭐ | Medium |
| Upgrade Free → Paid | Once | ⭐⭐⭐⭐⭐ | Low |
| Social media connection | Once per platform | ⭐⭐⭐⭐ | Medium |
| Content calendar planning | Monthly | ⭐⭐⭐ | Medium |
| SEO optimization | Ad-hoc | ⭐⭐⭐ | High |

### 6.2 Integration Priorities

**Must-Have (MVP):**
1. ✅ WordPress (REST API)
2. ✅ Facebook (Graph API)
3. ✅ LinkedIn (API v2)
4. ✅ Twitter/X (API v2)
5. ✅ Stripe (Payments)
6. ✅ OpenAI (Content generation)
7. ✅ Email service (SendGrid/SES)

**Should-Have (Phase 2):**
8. Google Analytics
9. Google Search Console
10. Unsplash/Pexels APIs
11. Przelewy24/PayU

**Nice-to-Have (Future):**
12. HubSpot CRM
13. Zapier/Make
14. Instagram (Meta API)
15. TikTok (if API available)

---

## 7. Rekomendacje UX/UI

### 7.1 Onboarding Improvements

**Current issues (domniemane):**
- Brak progressive disclosure (zbyt dużo info naraz)
- Długi formularz rejestracji może zniechęcać

**Rekomendacje:**
1. **Skrócić rejestrację do minimum:**
   - Email + hasło TYLKO
   - Wszystko inne w onboardingu (po weryfikacji)

2. **Guided onboarding (krok po kroku):**
   ```
   Krok 1/5: Powiedz nam o swojej firmie
   Krok 2/5: Wybierz branżę
   Krok 3/5: Połącz social media
   Krok 4/5: Połącz WordPress (opcjonalnie - "Skip")
   Krok 5/5: Stwórz pierwszy artykuł (tutorial)
   ```

3. **Progress bar** - użytkownik wie jak daleko

4. **Możliwość "Skip"** - nie zmuszać do wszystkiego od razu

### 7.2 Publishing Flow Improvements

**Rekomendacje:**
1. **Preview przed publikacją** - jak będzie wyglądać na każdej platformie
2. **Bulk actions** - publikuj 5 artykułów jednocześnie
3. **Smart scheduling** - AI sugeruje najlepsze godziny
4. **Revision history** - możliwość cofnięcia zmian

### 7.3 Error Handling

**Critical scenarios:**
- **Payment failed:** Jasny komunikat + link do update payment method
- **Publishing failed:** Szczegóły błędu + retry button
- **API rate limit:** "Spróbuj ponownie za 10 minut"
- **Token expired:** Auto-refresh lub prompt do re-authorize

---

## 8. Technical Debt & Risks

### 8.1 Identified Risks

1. **Social Media API Changes:**
   - Risk: Twitter, Facebook mogą zmienić API
   - Mitigation: Modular architecture, monitoring

2. **WordPress Plugin Compatibility:**
   - Risk: Konflikty z innymi pluginami
   - Mitigation: Extensive testing, fallback methods

3. **Payment Processing:**
   - Risk: Failed payments → churn
   - Mitigation: Dunning management, grace periods

4. **AI Generation Costs:**
   - Risk: High OpenAI API costs per article
   - Mitigation: Caching, optimize prompts, consider cheaper models

### 8.2 Scalability Considerations

**Potential bottlenecks:**
- OpenAI API calls (sequential → slow)
  - Solution: Queue system, parallel processing
- Database queries (analytics)
  - Solution: Caching layer (Redis)
- Publishing to multiple platforms
  - Solution: Background jobs (RabbitMQ/Celery)

---

**Koniec analizy Krok 2**
