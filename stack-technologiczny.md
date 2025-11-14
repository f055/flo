# Stack Technologiczny Floowe

Data analizy: 2025-11-14

---

## 1. FLOOWE.COM - Strona Marketingowa

### Framework & CMS
- **WordPress** - System zarządzania treścią
- **Elementor v3.33.0** - Page builder (visual website builder)
  - `elementorFrontendConfig` widoczny w kodzie źródłowym
  - Elementor Frontend JavaScript dla interaktywności

### Frontend JavaScript
- **jQuery** - Główna biblioteka JavaScript
  - Zaawansowana konfiguracja event handling
  - Integracja z Elementor
- **RocketLazyLoadScripts v2.0.4** - Optymalizacja ładowania skryptów
  - Lazy loading dla poprawy wydajności
  - Delay execution strategia

### Performance & Optimization
- **WP Rocket** - Premium WordPress caching plugin
  - Page caching
  - Link prefetching
  - Script optimization
  - Minification
- **Lazy Loading**:
  - RLL YouTube Player (lazy load dla wideo)
  - Lazy loading obrazów
  - Deferred script loading

### CSS & Styling
- **Custom CSS** - Własne style (brak Bootstrap/Tailwind)
- **CSS Variables** (`:root`):
  - Preset colors and gradients
  - Theme consistency
- **Custom Animations**:
  ```css
  @keyframes ha_fadeIn { ... }
  @keyframes ha_zoomIn { ... }
  @keyframes ha_bounce { ... }
  ```
- **Responsive Design**:
  - Mobile breakpoint: 767px
  - Tablet breakpoint: 1500px
  - Desktop: 1500px+

### Analytics & Tracking
- **Google Analytics 4**
  - Tag ID: `GT-NNQSFRS`
  - Event tracking
  - Conversion tracking
- **Google Tag Manager**
  - Advanced tracking setup
  - DataLayer implementation
- **Facebook Pixel**
  - Pixel ID: `1489099021984797`
  - Conversion API
  - Event tracking
- **PixelYourSite v11.1.3**
  - WordPress plugin dla tracking
  - Multi-platform event management
  - WooCommerce integration ready

### Live Chat & Support
- **Tawk.to** - Live chat widget
  - Real-time customer support
  - Visitor monitoring

### SEO & Structured Data
- **Schema.org JSON-LD**:
  - `BreadcrumbList` - Navigation breadcrumbs
  - `Organization` - Company information
- **Open Graph** tags (domniemane)
- **Meta tags** optimization

### Multilingual & Localization
- **WPML** (WordPress Multilingual)
  - Polski (domyślny)
  - English
  - Deutsch (niemiecki)
  - Nederlands (holenderski)

### Hosting & Infrastructure
- **WordPress Hosting** (self-hosted lub managed)
- **CDN** - Content Delivery Network
  - Path: `floowe.com/wp-content/uploads/`
  - Static assets caching
  - Image optimization

### Performance Metrics
- Lazy loading enabled
- Script deferring
- Resource hints (prefetch)
- Optimized image delivery

---

## 2. FLOOWE.LEAWARE.COM - Aplikacja (Panel Użytkownika)

### Ograniczenia Analizy
⚠️ **Uwaga**: Strona aplikacji jest renderowana dynamicznie i wymaga autoryzacji, co utrudnia pełną analizę przez narzędzia web scraping. Analiza oparta na dostępnych fragmentach i wnioskach.

### Dostępne Informacje

#### Analytics
- **Google Analytics 4**
  - Property ID: `G-FG4S3JWFPP`
  - Tag Manager integration
  - DataLayer implementation

#### Domain & Hosting
- **Subdomena**: `floowe.leaware.com`
- **Leaware** - Polski software house (developer aplikacji)
  - Specjalizacja: Custom software development
  - Typowy stack: React, Vue.js, Node.js, .NET

### Prawdopodobny Stack (Na podstawie wzorców Leaware i analizy kontekstowej)

#### Frontend Framework (Wysokie prawdopodobieństwo)
- **React.js** lub **Vue.js**
  - SPA (Single Page Application) architektura
  - Component-based architecture
  - Virtual DOM dla wydajności

#### UI Component Library (Potencjalne)
- **Material-UI (MUI)** - jeśli React
- **Ant Design** - popularne w enterprise apps
- **Vuetify** - jeśli Vue.js
- **Custom components** - tailored design system

#### State Management
- **Redux** lub **Redux Toolkit** (React)
- **Vuex** lub **Pinia** (Vue.js)
- **React Query/TanStack Query** - server state management
- **Context API** - local state management

#### Form Management & Validation
- **React Hook Form** (React)
- **Formik** (React)
- **Vee-Validate** (Vue)
- **Yup** lub **Zod** - schema validation

#### Authentication
- **JWT (JSON Web Tokens)** - token-based auth
- **OAuth 2.0** - social media integrations
- **Refresh token rotation**
- **Secure cookie storage**

#### HTTP Client
- **Axios** - HTTP requests
- **Fetch API** - native browser API
- Interceptors dla auth headers

#### Routing
- **React Router** (React)
- **Vue Router** (Vue.js)
- Protected routes
- Dynamic routing

#### Build Tools & Bundlers
- **Vite** (modern, fast)
- **Webpack** (traditional)
- **esbuild** - super fast bundling
- Code splitting
- Tree shaking

#### CSS Approach (Prawdopodobne)
- **CSS Modules**
- **Styled Components** (CSS-in-JS)
- **Tailwind CSS** - utility-first
- **Sass/SCSS** - CSS preprocessor

#### Backend (Prawdopodobny)
Na podstawie typowego stacku Leaware:
- **.NET Core / .NET 6+** (C#)
  - ASP.NET Core Web API
  - Entity Framework Core
  - RESTful API architecture
- Alternatywnie: **Node.js + Express**

#### Database
- **PostgreSQL** (najprawdopodobniejsze)
- **Microsoft SQL Server**
- **MongoDB** (dla części danych)

#### API Architecture
- **RESTful API**
  - JSON data format
  - HTTP status codes
  - CRUD operations
- Potencjalnie: **GraphQL** (mniej prawdopodobne dla MVP)

#### Authentication & Security
- **JWT Bearer tokens**
- **BCrypt** - password hashing
- **HTTPS/TLS** encryption
- **CORS** configuration
- **Rate limiting**
- **CSRF protection**

#### Cloud & Hosting
- **Azure** (popularny w .NET ecosystem)
- **AWS** (jeśli Node.js)
- **DigitalOcean** (cost-effective dla startups)
- **Docker** containerization

---

## 3. Porównanie Stacków

| Aspekt | floowe.com (Marketing) | floowe.leaware.com (App) |
|--------|------------------------|--------------------------|
| **Typ** | Static/WordPress | SPA/Dynamic |
| **Framework** | WordPress + Elementor | React/Vue (prawdopodobne) |
| **Rendering** | Server-side | Client-side |
| **Interaktywność** | Ograniczona (jQuery) | Wysoka (Modern JS) |
| **Purpose** | SEO, Lead generation | User functionality |
| **Performance** | WP Rocket + caching | Code splitting, lazy loading |
| **Analytics** | GT-NNQSFRS | G-FG4S3JWFPP |
| **Complexity** | Niższa (CMS) | Wyższa (Custom app) |
| **Update Frequency** | Rzadziej | Częściej (features) |

---

## 4. Architektura Systemu (Całość)

```
┌─────────────────────────────────────────────────────────────┐
│                      FLOOWE ECOSYSTEM                        │
└─────────────────────────────────────────────────────────────┘

┌──────────────────────┐         ┌──────────────────────────┐
│   FLOOWE.COM         │         │  FLOOWE.LEAWARE.COM      │
│   (WordPress)        │         │  (React/Vue SPA)         │
├──────────────────────┤         ├──────────────────────────┤
│ • Marketing pages    │         │ • User dashboard         │
│ • SEO content        │         │ • Content generation     │
│ • Lead capture       │         │ • Article editor         │
│ • Blog               │────────▶│ • Publishing tools       │
│ • Pricing            │ Sign up │ • Analytics              │
│ • Testimonials       │         │ • Settings               │
└──────────────────────┘         └──────────────────────────┘
         │                                   │
         │                                   │
         ▼                                   ▼
┌──────────────────────────────────────────────────────────────┐
│                    BACKEND API (.NET/Node.js)                │
├──────────────────────────────────────────────────────────────┤
│ • User authentication (JWT)                                  │
│ • Content generation (AI/GPT integration)                    │
│ • Publishing service (Social media APIs)                     │
│ • Analytics service                                          │
│ • Payment processing (Stripe/PayU)                           │
└──────────────────────────────────────────────────────────────┘
         │                                   │
         ▼                                   ▼
┌──────────────────┐            ┌──────────────────────────┐
│   DATABASE       │            │   EXTERNAL SERVICES      │
│   (PostgreSQL)   │            ├──────────────────────────┤
├──────────────────┤            │ • OpenAI API             │
│ • Users          │            │ • Facebook Graph API     │
│ • Articles       │            │ • LinkedIn API           │
│ • Subscriptions  │            │ • Twitter API            │
│ • Analytics      │            │ • WordPress API          │
└──────────────────┘            │ • Unsplash/Pexels        │
                                │ • Payment gateways       │
                                └──────────────────────────┘
```

---

## 5. Rekomendacje Techniczne

### Dla Strony Marketingowej (floowe.com)
1. **Performance**:
   - ✅ WP Rocket już zaimplementowany
   - Consider: Cloudflare CDN dla lepszego zasięgu globalnego
   - Optimize: Image compression (WebP format)

2. **SEO**:
   - ✅ Schema.org już zaimplementowane
   - Enhance: Rich snippets dla artykułów
   - Add: FAQ schema dla lepszych featured snippets

3. **Analytics**:
   - ✅ Multi-platform tracking już aktywny
   - Add: Heatmaps (Hotjar/Crazy Egg) dla UX insights
   - Implement: A/B testing (Google Optimize)

### Dla Aplikacji (floowe.leaware.com)
1. **Performance**:
   - Implement: Code splitting per route
   - Use: React.lazy() lub Vue dynamic imports
   - Add: Service Workers dla offline capability
   - Optimize: Bundle size analysis

2. **Security**:
   - ✅ JWT prawdopodobnie zaimplementowane
   - Enhance: Refresh token rotation
   - Add: 2FA (Two-Factor Authentication)
   - Implement: Rate limiting na API

3. **User Experience**:
   - Add: Progressive Web App (PWA) capabilities
   - Implement: Real-time updates (WebSockets)
   - Enhance: Offline mode dla edytora
   - Add: Auto-save functionality

4. **Monitoring**:
   - Add: Sentry/Rollbar dla error tracking
   - Implement: Performance monitoring (New Relic/DataDog)
   - Add: User session recording

---

## 6. Tech Debt & Future Considerations

### Potencjalne Tech Debt
1. **WordPress na stronie głównej**:
   - Pros: Szybkie do setupu, łatwe dla non-devs
   - Cons: Performance limitations, security vulnerabilities
   - Future: Consider migration do Next.js/Gatsby dla lepszego performance

2. **Separate domains**:
   - Pros: Separation of concerns
   - Cons: Session management complexity, SEO dilution
   - Consider: Unified domain (app.floowe.com) w przyszłości

### Skalowanie
1. **Backend**:
   - Implement: Microservices architecture
   - Add: Message queue (RabbitMQ/Redis) dla async tasks
   - Use: Caching layer (Redis) dla często używanych danych

2. **Frontend**:
   - Consider: Server-Side Rendering (Next.js/Nuxt)
   - Implement: Edge functions dla lepszego performance
   - Use: GraphQL dla efficient data fetching

3. **Infrastructure**:
   - Implement: Kubernetes dla container orchestration
   - Add: CI/CD pipeline automation
   - Use: Blue-green deployment strategy

---

## Podsumowanie

**floowe.com** używa tradycyjnego, sprawdzonego stacku (WordPress + Elementor) zoptymalizowanego pod SEO i lead generation. To rozsądny wybór dla strony marketingowej, która nie wymaga częstych zmian w kodzie.

**floowe.leaware.com** prawdopodobnie wykorzystuje nowoczesny stack SPA (React/Vue + .NET/Node.js), który oferuje wysoką interaktywność i responsywność niezbędną dla aplikacji produktywnej.

Rozdzielenie tych dwóch światów to powszechna praktyka:
- Marketing site: WordPress (SEO-friendly, easy content management)
- Application: Modern SPA (rich user experience, complex interactions)

Jest to pragmatyczne podejście dla early-stage SaaS produktów.
