# Krok 3: Event Storming - Kluczowe Procesy Floowe

Data analizy: 2025-11-14

---

## Wprowadzenie do Event Storming

Event Storming to technika warsztatowa do eksploracji złożonych domen biznesowych. Wizualizuje procesy poprzez:

**Legenda kolorów:**
- 🟧 **Domain Events** (pomarańczowe) - Co się wydarzyło (przeszły czas)
- 🟦 **Commands** (niebieskie) - Akcje użytkownika/systemu (tryb rozkazujący)
- 🟨 **Aggregates** (żółte) - Encje/obiekty domenowe
- 🟪 **Policies** (fioletowe) - Reguły biznesowe automatyczne
- 🟥 **Hotspots** (czerwone) - Problemy, pytania, ryzyka
- 🟩 **External Systems** (zielone) - Systemy zewnętrzne

---

## Event Storming 1: Content Creation & Publishing

### Kontekst
**Proces:** Użytkownik tworzy artykuł SEO i publikuje go na blog WordPress oraz social media

**Aktorzy:**
- User (content creator)
- System (Floowe backend)
- External APIs (OpenAI, WordPress, Facebook, LinkedIn, Twitter)

**Business Value:** Core feature - główna wartość produktu

---

### Diagram Event Storming

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                    CONTENT CREATION & PUBLISHING FLOW                                │
└─────────────────────────────────────────────────────────────────────────────────────┘

🟦 Create Article          🟧 ArticleCreationStarted        🟨 Article
   ├─ Topic                   ├─ userId                       ├─ id
   ├─ Keywords                ├─ topic                        ├─ title
   └─ Language                ├─ keywords                     ├─ content (draft)
                              └─ timestamp                    ├─ status
                                                              ├─ seoData
                                    ↓                         └─ metadata

                           🟩 OpenAI API
                           ├─ GPT-4 model
                           └─ Content generation

                                    ↓

                           🟧 ContentGenerated
                              ├─ articleId
                              ├─ generatedContent
                              ├─ wordCount
                              └─ seoScore

🟥 HOTSPOT: AI generation może failować
   - Rate limits
   - API downtime
   - Quality issues

                                    ↓

🟦 Edit Article              🟧 ArticleEdited              🟨 Article
   ├─ articleId                 ├─ articleId                 ├─ content (updated)
   ├─ updatedContent            ├─ changes                   ├─ version++
   └─ images                    └─ timestamp                 └─ lastModified

                                    ↓

🟦 Add Images                🟧 ImagesAdded
   ├─ articleId                 ├─ articleId
   ├─ imageSource               ├─ imageUrls[]
   └─ imageUrls[]               └─ sources

                           🟩 Stock Photo APIs
                           ├─ Unsplash
                           └─ Pexels

                                    ↓

🟦 Preview Article           🟧 ArticlePreviewed
   └─ articleId                 ├─ articleId
                                └─ previewData

                                    ↓

🟦 Select Channels           🟧 PublishChannelsSelected     🟨 PublishConfig
   ├─ articleId                 ├─ articleId                  ├─ channels[]
   ├─ channels[]                ├─ channels[]                 │  ├─ WordPress
   │  ├─ WordPress              │  ├─ WordPress               │  ├─ Facebook
   │  ├─ Facebook               │  ├─ Facebook                │  ├─ LinkedIn
   │  ├─ LinkedIn               │  └─ Twitter                 │  └─ Twitter
   │  └─ Twitter                └─ scheduledDate (optional)   └─ schedule
   └─ schedule

                                    ↓

🟦 Publish Article           🟧 PublishingInitiated
   └─ articleId                 ├─ articleId
                                ├─ channels[]
                                └─ timestamp

                                    ↓

                           🟪 POLICY: Multi-Channel Publisher
                              WHEN PublishingInitiated
                              THEN publish to all selected channels

                                    ↓
                    ┌───────────────┼───────────────┬───────────────┐
                    ↓               ↓               ↓               ↓

            🟩 WordPress      🟩 Facebook      🟩 LinkedIn     🟩 Twitter
            REST API          Graph API        API v2          API v2
            ├─ POST /posts    ├─ POST /feed    ├─ POST /ugc    ├─ POST /tweets
            └─ Auth: JWT      └─ OAuth 2.0     └─ OAuth 2.0    └─ OAuth 2.0

                    ↓               ↓               ↓               ↓

         🟧 PublishedToWordPress  🟧 PublishedToFacebook  🟧 PublishedToLinkedIn  🟧 PublishedToTwitter
            ├─ articleId            ├─ articleId             ├─ articleId             ├─ articleId
            ├─ wordpressPostId      ├─ facebookPostId        ├─ linkedinPostId        ├─ tweetId
            └─ postUrl              └─ postUrl               └─ postUrl               └─ tweetUrl

🟥 HOTSPOT: Publishing może failować na niektórych kanałach
   - API rate limits
   - OAuth token expired
   - Network issues
   → Partial success handling?

                                    ↓

                           🟪 POLICY: Publishing Status Aggregator
                              WHEN all channels published OR failed
                              THEN aggregate results

                                    ↓

                           🟧 ArticlePublished
                              ├─ articleId
                              ├─ successfulChannels[]
                              ├─ failedChannels[]
                              ├─ postUrls{}
                              └─ timestamp

                                    ↓

                           🟪 POLICY: Notify User
                              WHEN ArticlePublished
                              THEN send notification

                                    ↓

                           🟧 UserNotified               🟩 Email Service
                              ├─ userId                    (SendGrid/SES)
                              ├─ notificationType
                              └─ articleDetails

                                    ↓

🟦 View Analytics            🟧 AnalyticsViewed
   └─ articleId                 ├─ articleId
                                └─ metrics
                                   ├─ views
                           🟩      ├─ engagement       Google Analytics
                           │       └─ rankings         Google Search Console
                           └─────────────┘
```

---

### Event List (Chronological)

| # | Event | Aggregate | Triggered By | External System |
|---|-------|-----------|--------------|-----------------|
| 1 | ArticleCreationStarted | Article | User: Create Article | - |
| 2 | ContentGenerated | Article | System (async) | OpenAI API |
| 3 | ArticleEdited | Article | User: Edit Article | - |
| 4 | ImagesAdded | Article | User: Add Images | Unsplash/Pexels |
| 5 | ArticlePreviewed | Article | User: Preview | - |
| 6 | PublishChannelsSelected | PublishConfig | User: Select Channels | - |
| 7 | PublishingInitiated | Article | User: Publish Article | - |
| 8 | PublishedToWordPress | Article | System (async) | WordPress API |
| 9 | PublishedToFacebook | Article | System (async) | Facebook API |
| 10 | PublishedToLinkedIn | Article | System (async) | LinkedIn API |
| 11 | PublishedToTwitter | Article | System (async) | Twitter API |
| 12 | ArticlePublished | Article | System (Policy) | - |
| 13 | UserNotified | User | System (Policy) | Email Service |
| 14 | AnalyticsViewed | Article | User: View Analytics | Google Analytics |

---

### Commands & Aggregates

**Commands:**
1. `CreateArticle` → Creates new Article (status: generating)
2. `EditArticle` → Updates Article content
3. `AddImages` → Adds images to Article
4. `PreviewArticle` → Read-only view
5. `SelectPublishChannels` → Creates PublishConfig
6. `PublishArticle` → Initiates publishing process
7. `ViewAnalytics` → Fetches analytics data

**Aggregates:**
1. `Article` (main entity)
   - Properties: id, title, content, status, seoData, images, metadata
   - States: draft, generating, ready, publishing, published, failed

2. `PublishConfig`
   - Properties: articleId, channels[], schedule, publishedChannels[], failedChannels[]

3. `User` (implicit)
   - Properties: id, subscription, usage limits

---

### Policies (Business Rules)

1. **Multi-Channel Publisher**
   - WHEN: PublishingInitiated
   - THEN: Publish to all selected channels in parallel
   - RETRY: Failed channels (3 attempts with exponential backoff)

2. **Publishing Status Aggregator**
   - WHEN: All channels completed (success OR fail)
   - THEN: Update Article status and create ArticlePublished event

3. **Notify User**
   - WHEN: ArticlePublished
   - THEN: Send email + in-app notification with results

4. **Usage Limit Enforcer**
   - WHEN: CreateArticle
   - THEN: Check user's monthly limit
   - IF: Limit exceeded → Reject with UpgradeRequired error

---

### Hotspots & Risks

🟥 **HOTSPOT 1: AI Generation Failure**
- **Problem:** OpenAI API może failować (rate limit, downtime, timeout)
- **Impact:** User czeka, nie dostaje contentu
- **Mitigation:**
  - Timeout handling (max 2 min)
  - Retry logic (3 attempts)
  - Fallback to different model (GPT-3.5 if GPT-4 fails)
  - Clear error message + option to regenerate

🟥 **HOTSPOT 2: Partial Publishing Success**
- **Problem:** WordPress succeeds, Facebook fails → Partial state
- **Impact:** User confusion, data inconsistency
- **Mitigation:**
  - Clear status per channel (✓ published, ✗ failed, ⏳ pending)
  - Retry button per failed channel
  - Save draft to failed channels for manual publish

🟥 **HOTSPOT 3: OAuth Token Expiration**
- **Problem:** Social media tokens expire (60 days Facebook)
- **Impact:** Publishing fails silently
- **Mitigation:**
  - Token refresh flow (automatic before publish)
  - Warning notifications (7 days before expiry)
  - Clear re-authorization UX

---

## Event Storming 2: User Registration & Onboarding

### Kontekst
**Proces:** Nowy użytkownik rejestruje się, weryfikuje email, przechodzi onboarding i tworzy pierwsze połączenia

**Aktorzy:**
- User (prospect → customer)
- System (Floowe backend)
- External APIs (Email service, Google/Facebook OAuth, WordPress, Social Media)

**Business Value:** Critical dla user acquisition i activation

---

### Diagram Event Storming

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                    USER REGISTRATION & ONBOARDING FLOW                               │
└─────────────────────────────────────────────────────────────────────────────────────┘

🟦 Visit Homepage            🟧 HomepageVisited
                                ├─ sessionId
                                ├─ referrer
                                └─ timestamp

                           🟩 Google Analytics
                              Track visitor

                                    ↓

🟦 Click "Przetestuj za darmo"   🟧 CTAClicked
                                     ├─ sessionId
                                     └─ ctaLocation

                                    ↓

🟦 Fill Registration Form    🟧 RegistrationAttempted      🟨 User
   ├─ email                     ├─ email                     ├─ id
   ├─ password                  ├─ hashedPassword            ├─ email
   └─ termsAccepted            └─ ipAddress                 ├─ status: unverified
                                                             ├─ plan: free
                                                             └─ createdAt

                                    ↓

                           🟪 POLICY: Email Uniqueness Check
                              IF email exists THEN reject

🟥 HOTSPOT: Spam/fake emails?
   - Email validation
   - Captcha consideration

                                    ↓

                           🟧 UserRegistered
                              ├─ userId
                              ├─ email
                              └─ timestamp

                                    ↓

                           🟪 POLICY: Send Verification Email
                              WHEN UserRegistered
                              THEN send verification link

                                    ↓

                           🟧 VerificationEmailSent      🟩 Email Service
                              ├─ userId                    (SendGrid/SES)
                              ├─ verificationToken         ├─ SMTP
                              └─ expiresAt (24h)           └─ Template

                                    ↓

🟦 Click Verification Link   🟧 EmailVerificationClicked
   └─ verificationToken         ├─ userId
                                └─ token

                                    ↓

                           🟪 POLICY: Validate Token
                              IF token valid & not expired
                              THEN verify user

                                    ↓

                           🟧 UserVerified               🟨 User
                              ├─ userId                    ├─ status: verified
                              └─ timestamp                 └─ verifiedAt

                                    ↓

                           🟪 POLICY: Start Onboarding
                              WHEN UserVerified
                              THEN redirect to onboarding

                                    ↓

                           🟧 OnboardingStarted          🟨 OnboardingSession
                              ├─ userId                    ├─ userId
                              └─ sessionId                 ├─ currentStep: 1/5
                                                           └─ completedSteps[]

                                    ↓

🟦 Enter Company Info        🟧 CompanyInfoProvided
   ├─ companyName               ├─ userId
   └─ industry                  ├─ companyName
                                └─ industry

                                    ↓

🟦 Enter Keywords            🟧 KeywordsProvided           🟨 User
   └─ keywords[]                ├─ userId                   ├─ profile
                                └─ keywords[]                   ├─ company
                                                                ├─ industry
                                                                └─ keywords[]

                                    ↓

🟦 Connect Social Media      🟧 SocialConnectionInitiated
   └─ platform                  ├─ userId
      (Facebook)                └─ platform

                                    ↓

                           🟩 Facebook OAuth
                              ├─ Authorization flow
                              └─ Permissions request

                                    ↓

🟦 Authorize Facebook        🟧 FacebookAuthorized
                                ├─ userId
                                ├─ facebookUserId
                                ├─ accessToken
                                └─ pages[]

                                    ↓

🟦 Select Facebook Pages     🟧 FacebookPagesSelected      🟨 Integration
   └─ pages[]                   ├─ userId                    ├─ userId
                                ├─ selectedPages[]           ├─ platform: facebook
                                └─ timestamp                 ├─ status: connected
                                                             └─ config

🟥 HOTSPOT: User może nie mieć Facebook pages
   - Skip option needed
   - Create page link?

                                    ↓

🟦 Connect WordPress         🟧 WordPressConnectionInitiated
   (optional - can skip)        └─ userId

                                    ↓
                              ┌─────┴─────┐
                              ↓           ↓

        Option A: Auto        🟧 WordPressCredsProvided    Option B: Manual
        ├─ wpUrl                 ├─ userId                  🟧 PluginDownloaded
        ├─ adminUser             ├─ wpUrl                      └─ userId
        └─ adminPass             └─ credentials

                 ↓                                              ↓

        🟩 WordPress API                              User installs manually
        ├─ Verify connection                                   ↓
        └─ Install plugin
                                                           🟦 Enter API Key
                 ↓                                            └─ apiKey
                                                                 ↓
                 └────────────────┬─────────────────────────────┘
                                  ↓

                           🟧 WordPressConnected         🟨 Integration
                              ├─ userId                    ├─ platform: wordpress
                              ├─ wpSiteUrl                 ├─ status: connected
                              └─ apiKey                    └─ siteUrl

                                    ↓

🟦 Skip WordPress            🟧 WordPressSkipped
   (if user chooses)            └─ userId

                                    ↓

                           🟪 POLICY: Start Guided Tutorial
                              WHEN onboarding steps completed
                              THEN show first article tutorial

                                    ↓

                           🟧 TutorialStarted
                              ├─ userId
                              └─ tutorialType: firstArticle

                                    ↓

🟦 Create First Article      🟧 FirstArticleCreated        🟨 Article
   (guided)                     ├─ userId                    ├─ isFirstArticle: true
   └─ suggestedTopic            ├─ articleId                 └─ tutorialMode: true
                                └─ topic

                                    ↓

                           (Same as Content Creation flow...)

                                    ↓

                           🟧 FirstArticlePublished
                              ├─ userId
                              ├─ articleId
                              └─ channels[]

                                    ↓

                           🟪 POLICY: Complete Onboarding
                              WHEN FirstArticlePublished
                              THEN mark onboarding as complete

                                    ↓

                           🟧 OnboardingCompleted        🟨 User
                              ├─ userId                    ├─ onboardingCompleted: true
                              └─ timestamp                 └─ activationDate

                                    ↓

                           🟪 POLICY: Send Welcome Email
                              WHEN OnboardingCompleted
                              THEN send success email + tips

                                    ↓

                           🟧 WelcomeEmailSent           🟩 Email Service
                              ├─ userId                    └─ Welcome template
                              └─ emailType: welcome

                                    ↓

                           🟧 UserActivated              🟨 User
                              ├─ userId                    └─ status: active
                              └─ timestamp

                           🎉 SUCCESS: User is activated!
```

---

### Event List (Chronological)

| # | Event | Aggregate | Triggered By | External System |
|---|-------|-----------|--------------|-----------------|
| 1 | HomepageVisited | Session | User: Visit | Google Analytics |
| 2 | CTAClicked | Session | User: Click CTA | - |
| 3 | RegistrationAttempted | User | User: Fill Form | - |
| 4 | UserRegistered | User | System (validation) | - |
| 5 | VerificationEmailSent | User | System (Policy) | Email Service |
| 6 | EmailVerificationClicked | User | User: Click Link | - |
| 7 | UserVerified | User | System (Policy) | - |
| 8 | OnboardingStarted | OnboardingSession | System (Policy) | - |
| 9 | CompanyInfoProvided | User | User: Enter Info | - |
| 10 | KeywordsProvided | User | User: Enter Keywords | - |
| 11 | SocialConnectionInitiated | Integration | User: Connect | - |
| 12 | FacebookAuthorized | Integration | User: Authorize | Facebook OAuth |
| 13 | FacebookPagesSelected | Integration | User: Select Pages | - |
| 14 | WordPressConnectionInitiated | Integration | User: Connect WP | - |
| 15 | WordPressConnected | Integration | System (verification) | WordPress API |
| 16 | WordPressSkipped | OnboardingSession | User: Skip | - |
| 17 | TutorialStarted | Tutorial | System (Policy) | - |
| 18 | FirstArticleCreated | Article | User: Create | - |
| 19 | FirstArticlePublished | Article | User: Publish | Multi APIs |
| 20 | OnboardingCompleted | OnboardingSession | System (Policy) | - |
| 21 | WelcomeEmailSent | User | System (Policy) | Email Service |
| 22 | UserActivated | User | System | - |

---

### Commands & Aggregates

**Commands:**
1. `VisitHomepage` → Track session
2. `RegisterUser` → Create User (unverified)
3. `VerifyEmail` → Update User (verified)
4. `ProvideCompanyInfo` → Update User profile
5. `ProvideKeywords` → Update User profile
6. `ConnectSocialMedia` → Create Integration
7. `SelectPages` → Update Integration config
8. `ConnectWordPress` → Create Integration
9. `SkipWordPress` → Update OnboardingSession
10. `CreateFirstArticle` → Create Article (tutorial mode)
11. `PublishFirstArticle` → Publish Article
12. `CompleteOnboarding` → Update User (activated)

**Aggregates:**
1. `User`
   - States: unverified → verified → active
   - Properties: email, profile, plan, status, onboardingCompleted

2. `OnboardingSession`
   - Properties: userId, currentStep, completedSteps[], skipped[]

3. `Integration`
   - Properties: userId, platform, status, config, tokens

4. `Article` (tutorial mode)
   - Properties: isFirstArticle, tutorialMode

---

### Policies (Business Rules)

1. **Email Uniqueness Check**
   - WHEN: RegistrationAttempted
   - THEN: Check if email exists
   - IF: Exists → Reject with "Email already registered"

2. **Send Verification Email**
   - WHEN: UserRegistered
   - THEN: Generate token + send email
   - TOKEN: Expires in 24h

3. **Start Onboarding**
   - WHEN: UserVerified
   - THEN: Redirect to onboarding flow

4. **Start Guided Tutorial**
   - WHEN: Onboarding steps completed (min: company + keywords)
   - THEN: Show first article tutorial

5. **Complete Onboarding**
   - WHEN: FirstArticlePublished
   - THEN: Mark user as activated

6. **Send Welcome Email**
   - WHEN: OnboardingCompleted
   - THEN: Send tips, resources, next steps

---

### Hotspots & Risks

🟥 **HOTSPOT 1: Email Verification Abandonment**
- **Problem:** User nie weryfikuje email (24h expiry)
- **Impact:** Lost lead, user can't use product
- **Mitigation:**
  - Reminder email after 2h, 12h
  - "Resend verification" option
  - Temporary access (limited) without verification?

🟥 **HOTSPOT 2: Onboarding Drop-off**
- **Problem:** User quit podczas onboardingu (zbyt długi?)
- **Impact:** Low activation rate
- **Mitigation:**
  - Make steps optional (skip-able)
  - Progress bar (5/5 steps)
  - Save progress (resume later)
  - Minimum viable onboarding (tylko email → straight to product)

🟥 **HOTSPOT 3: Social Media Connection Failures**
- **Problem:** User nie ma Facebook business page, OAuth fails
- **Impact:** Frustracja, może zrezygnować
- **Mitigation:**
  - Clear skip option
  - "Connect later" path
  - Help resources (how to create FB page)

🟥 **HOTSPOT 4: WordPress Connection Complexity**
- **Problem:** Technical users struggle z plugin installation
- **Impact:** Can't publish to website (core feature!)
- **Mitigation:**
  - Video tutorial
  - Step-by-step guide z screenshots
  - Support chat readily available

---

## Event Storming 3: Subscription Payment & Upgrade Flow

### Kontekst
**Proces:** Free user wyczerpuje limit artykułów, decyduje się na upgrade, wybiera plan, płaci, subscription jest aktywowana

**Aktorzy:**
- User (free → paid customer)
- System (Floowe backend)
- External APIs (Stripe, Przelewy24, Email service)

**Business Value:** Critical dla revenue generation

---

### Diagram Event Storming

```
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                    SUBSCRIPTION PAYMENT & UPGRADE FLOW                               │
└─────────────────────────────────────────────────────────────────────────────────────┘

🟦 Create Article            🟧 ArticleCreationAttempted
   (4th article)                ├─ userId
                                └─ currentUsage: 3/3

                                    ↓

                           🟪 POLICY: Check Usage Limit
                              IF usage >= limit
                              THEN block with upgrade prompt

                                    ↓

                           🟧 UsageLimitReached          🟨 User
                              ├─ userId                    ├─ plan: free
                              ├─ currentUsage: 3/3         └─ usage: 3/3
                              └─ limitType: articles

🟥 HOTSPOT: Ograniczenie może być frustrujące
   - Timing important (pokazać value najpierw)

                                    ↓

                           🟪 POLICY: Show Upgrade Modal
                              WHEN UsageLimitReached
                              THEN display upgrade options

                                    ↓

🟦 View Pricing              🟧 PricingViewed
                                ├─ userId
                                └─ source: limitModal

                                    ↓

🟦 Select Plan               🟧 PlanSelected               🟨 Cart
   ├─ planType                  ├─ userId                    ├─ userId
   │  (Basic/Standard/Premium)  ├─ selectedPlan              ├─ planType
   └─ billingCycle              ├─ billingCycle              ├─ amount
      (monthly/annual)          └─ amount                    └─ discount

                                    ↓

🟦 Enter Billing Info        🟧 BillingInfoProvided
   ├─ companyName               ├─ userId
   ├─ nip                       ├─ billingDetails
   ├─ address                   │  ├─ companyName
   └─ email                     │  ├─ nip (VAT ID)
                                │  ├─ address
                                │  └─ invoiceEmail
                                └─ timestamp

                                    ↓

🟦 Select Payment Method     🟧 PaymentMethodSelected
   └─ method                    ├─ userId
      ├─ Card (Stripe)          └─ method
      ├─ Przelewy24                ├─ card
      ├─ PayU                      ├─ przelewy24
      └─ BLIK                      └─ blik

                                    ↓

              ┌─────────────────────┼─────────────────────┐
              ↓                     ↓                     ↓

    Option A: Card          Option B: P24           Option C: BLIK

🟦 Enter Card Details    🟦 Select Bank          🟦 Enter BLIK Code
   ├─ cardNumber           └─ bankId                └─ blikCode
   ├─ expiry                                            (6 digits)
   └─ cvv
       ↓                        ↓                         ↓

🟩 Stripe API           🟩 Przelewy24 API       🟩 Przelewy24 API
   ├─ Create              ├─ Redirect to           (BLIK)
   │  PaymentIntent       │  bank login
   └─ Process             └─ Confirm payment
      payment
       ↓                        ↓                         ↓

   3D Secure           Bank authorization         Mobile confirm
   (if required)       flow
       ↓                        ↓                         ↓

       └─────────────────────┬─────────────────────┘
                             ↓

                      🟧 PaymentInitiated          🟨 Payment
                         ├─ userId                   ├─ id
                         ├─ paymentId                ├─ userId
                         ├─ amount                   ├─ status: pending
                         ├─ method                   ├─ amount
                         └─ timestamp                └─ method

🟥 HOTSPOT: Payment może failować
   - Declined card
   - Insufficient funds
   - Network timeout
   - Bank rejection

                             ↓
                    ┌────────┴────────┐
                    ↓                 ↓

         SUCCESS PATH          FAILURE PATH

    🟧 PaymentSucceeded      🟧 PaymentFailed
       ├─ userId                ├─ userId
       ├─ paymentId             ├─ paymentId
       ├─ amount                ├─ reason
       └─ transactionId         └─ errorCode

           ↓                        ↓

    🟪 POLICY: Activate        🟪 POLICY: Notify Failure
       Subscription               & Retry Options
       WHEN PaymentSucceeded      WHEN PaymentFailed
       THEN upgrade user          THEN show error + retry

           ↓                        ↓

    🟧 SubscriptionActivated   🟧 UserNotified
       ├─ userId                  ├─ userId
       ├─ subscriptionId          └─ notificationType: paymentFailed
       ├─ plan
       ├─ startDate                    ↓
       ├─ nextBillingDate
       └─ status: active          🟦 Retry Payment
                                     └─ paymentId
           ↓
                                      (back to payment flow)
    🟨 User
       ├─ plan: basic/standard/premium
       ├─ status: paid
       ├─ articleLimit: 8/12/20
       └─ subscriptionId

           ↓

    🟪 POLICY: Generate Invoice
       WHEN SubscriptionActivated
       THEN create invoice + send email

           ↓

    🟧 InvoiceGenerated         🟩 Invoice Service
       ├─ userId                   ├─ Generate PDF
       ├─ invoiceId                └─ VAT calculation
       ├─ amount
       ├─ vat
       └─ invoiceNumber

           ↓

    🟧 InvoiceEmailed          🟩 Email Service
       ├─ userId                  └─ Send PDF
       └─ invoiceUrl                 attachment

           ↓

    🟪 POLICY: Unlock Premium Features
       WHEN SubscriptionActivated
       THEN enable paid features

           ↓

    🟧 FeaturesUnlocked
       ├─ userId
       └─ features[]
          ├─ increasedArticleLimit
          ├─ websitePublishing
          └─ advancedAnalytics

           ↓

    🟪 POLICY: Send Welcome Email
       WHEN SubscriptionActivated
       THEN thank + onboard to paid features

           ↓

    🟧 WelcomeEmailSent        🟩 Email Service
       ├─ userId                  └─ Paid user
       └─ emailType: upgrade         welcome template

           ↓

    🟧 UserUpgraded            🟨 User
       ├─ userId                  └─ lastUpgraded
       └─ timestamp

    ═══════════════════════════════════════════════════════════

    RECURRING BILLING (Next Month)

                             ↓

    🟪 POLICY: Schedule Recurring Billing
       WHEN nextBillingDate approaching (T-3 days)
       THEN send reminder email

                             ↓

    🟧 BillingReminderSent     🟩 Email Service
       ├─ userId                  └─ Upcoming charge
       ├─ amount                     notification
       └─ chargeDate

                             ↓

    🟪 POLICY: Charge Subscription
       WHEN nextBillingDate reached
       THEN attempt payment

                             ↓

    🟧 RecurringPaymentInitiated   🟨 Payment
       ├─ userId                      ├─ type: recurring
       ├─ subscriptionId              └─ attempt: 1
       └─ amount

                             ↓

    🟩 Stripe API
       ├─ Charge saved payment method
       └─ Subscription billing

                             ↓
                    ┌────────┴────────┐
                    ↓                 ↓

         SUCCESS              FAILURE

    🟧 RecurringPaymentSucceeded   🟧 RecurringPaymentFailed
       └─ subscriptionId              ├─ subscriptionId
                                      └─ reason

           ↓                              ↓

    🟧 InvoiceGenerated            🟪 POLICY: Retry Failed Payment
       (same as initial)              WHEN RecurringPaymentFailed
                                      THEN retry after 3 days (x3)
           ↓
                                          ↓
    🟧 SubscriptionRenewed
       ├─ subscriptionId              🟧 PaymentRetryScheduled
       └─ nextBillingDate                ├─ subscriptionId
                                         └─ retryDate

🟥 HOTSPOT: Month 4 - Price jump!                 ↓
   250 zł → 500 zł (2x)
   HIGH CHURN RISK                            (retry payment)

           ↓                                      ↓

    (continue active)                  🟪 POLICY: Final Failure → Downgrade
                                          IF all retries failed
                                          THEN downgrade to free

                                              ↓

                                       🟧 SubscriptionDowngraded
                                          ├─ userId
                                          ├─ reason: paymentFailed
                                          └─ newPlan: free

                                              ↓

                                       🟧 DowngradeEmailSent     🟩 Email Service
                                          ├─ userId                 └─ Account suspended
                                          └─ retainData: true          notification

                                              ↓

                                       🟨 User
                                          ├─ plan: free
                                          ├─ status: payment_failed
                                          └─ graceEnded

    ═══════════════════════════════════════════════════════════

    SUBSCRIPTION CANCELLATION (User-initiated)

                             ↓

🟦 Cancel Subscription       🟧 CancellationRequested
   └─ subscriptionId            ├─ userId
                                ├─ subscriptionId
                                └─ reason (optional)

                                    ↓

                           🟪 POLICY: Retention Flow
                              WHEN CancellationRequested
                              THEN show retention offers

                                    ↓

                           🟧 RetentionOfferShown
                              ├─ userId
                              └─ offers[]
                                 ├─ discount (20% off)
                                 └─ downgrade (lower plan)

                                    ↓
                          ┌─────────┴─────────┐
                          ↓                   ↓

            🟦 Accept Offer           🟦 Reject Offer
               └─ offerId                └─ proceed

                  ↓                         ↓

            🟧 OfferAccepted          🟧 CancellationConfirmed
               ├─ userId                 ├─ subscriptionId
               └─ newTerms               └─ endDate

                  ↓                         ↓

            (subscription continues)   🟪 POLICY: Cancel at Period End
                                          WHEN CancellationConfirmed
                                          THEN set cancelAtPeriodEnd

                                              ↓

                                       🟧 SubscriptionCanceled
                                          ├─ subscriptionId
                                          ├─ activeUntil
                                          └─ status: canceled

                                              ↓

                                       🟩 Stripe API
                                          └─ Cancel subscription

                                              ↓

                                       🟧 CancellationEmailSent   🟩 Email Service
                                          ├─ userId                  └─ Sad to see you go
                                          └─ activeUntil                + reactivation link

                                              ↓

                                       (User continues until end date)

                                              ↓

                                       🟪 POLICY: Downgrade at End
                                          WHEN subscription period ends
                                          THEN downgrade to free

                                              ↓

                                       🟧 SubscriptionEnded
                                          ├─ userId
                                          └─ endedAt

                                              ↓

                                       🟨 User
                                          ├─ plan: free
                                          ├─ status: inactive
                                          └─ canceledAt
```

---

### Event List (Chronological - Happy Path)

| # | Event | Aggregate | Triggered By | External System |
|---|-------|-----------|--------------|-----------------|
| 1 | ArticleCreationAttempted | User | User: Create Article | - |
| 2 | UsageLimitReached | User | System (Policy) | - |
| 3 | PricingViewed | Session | User: View Pricing | - |
| 4 | PlanSelected | Cart | User: Select Plan | - |
| 5 | BillingInfoProvided | Cart | User: Enter Info | - |
| 6 | PaymentMethodSelected | Payment | User: Select Method | - |
| 7 | PaymentInitiated | Payment | User: Confirm | Stripe/P24 |
| 8 | PaymentSucceeded | Payment | System (webhook) | Stripe |
| 9 | SubscriptionActivated | Subscription | System (Policy) | - |
| 10 | InvoiceGenerated | Invoice | System (Policy) | Invoice Service |
| 11 | InvoiceEmailed | User | System (Policy) | Email Service |
| 12 | FeaturesUnlocked | User | System (Policy) | - |
| 13 | WelcomeEmailSent | User | System (Policy) | Email Service |
| 14 | UserUpgraded | User | System | - |
| ... | (Month later) | | | |
| 15 | BillingReminderSent | User | System (Scheduler) | Email Service |
| 16 | RecurringPaymentInitiated | Payment | System (Scheduler) | Stripe |
| 17 | RecurringPaymentSucceeded | Payment | System (webhook) | Stripe |
| 18 | InvoiceGenerated | Invoice | System (Policy) | Invoice Service |
| 19 | SubscriptionRenewed | Subscription | System | - |

---

### Commands & Aggregates

**Commands:**
1. `CreateArticle` (when limit reached) → Trigger upgrade flow
2. `ViewPricing` → Track intent
3. `SelectPlan` → Create Cart
4. `ProvideBillingInfo` → Update Cart
5. `SelectPaymentMethod` → Update Payment
6. `InitiatePayment` → Process through gateway
7. `ActivateSubscription` → Update User + Subscription
8. `GenerateInvoice` → Create Invoice
9. `UnlockFeatures` → Update User permissions
10. `ChargeRecurringPayment` → Process monthly charge
11. `CancelSubscription` → Update Subscription status
12. `DowngradeUser` → Revert to free plan

**Aggregates:**
1. `User`
   - States: free → paid → canceled → inactive
   - Properties: plan, status, subscriptionId, articleLimit, usage

2. `Subscription`
   - Properties: id, userId, plan, status, startDate, nextBillingDate, cancelAtPeriodEnd
   - States: active, past_due, canceled, ended

3. `Payment`
   - Properties: id, userId, subscriptionId, amount, method, status, transactionId
   - States: pending, succeeded, failed

4. `Invoice`
   - Properties: id, userId, invoiceNumber, amount, vat, pdfUrl, status
   - States: draft, sent, paid

5. `Cart` (transient)
   - Properties: userId, selectedPlan, amount, billingInfo

---

### Policies (Business Rules)

1. **Check Usage Limit**
   - WHEN: ArticleCreationAttempted
   - THEN: IF usage >= limit → Block + Show upgrade modal

2. **Activate Subscription**
   - WHEN: PaymentSucceeded
   - THEN:
     - Create Subscription record
     - Update User (plan, status, limits)
     - Set nextBillingDate (T+30 days)

3. **Generate Invoice**
   - WHEN: SubscriptionActivated OR SubscriptionRenewed
   - THEN: Create invoice + Send email with PDF

4. **Unlock Premium Features**
   - WHEN: SubscriptionActivated
   - THEN: Enable features based on plan tier

5. **Send Welcome Email**
   - WHEN: SubscriptionActivated (first time)
   - THEN: Thank user + guide to paid features

6. **Schedule Recurring Billing**
   - WHEN: nextBillingDate - 3 days
   - THEN: Send reminder email

7. **Charge Subscription**
   - WHEN: nextBillingDate reached
   - THEN: Attempt charge via Stripe

8. **Retry Failed Payment**
   - WHEN: RecurringPaymentFailed
   - THEN: Retry after 3, 7, 14 days (3 attempts total)

9. **Final Failure → Downgrade**
   - WHEN: All payment retries failed
   - THEN: Downgrade to free (keep user data)

10. **Retention Flow**
    - WHEN: CancellationRequested
    - THEN: Show offers (discount, downgrade)

11. **Cancel at Period End**
    - WHEN: CancellationConfirmed
    - THEN: Mark cancelAtPeriodEnd = true (not immediate)

12. **Downgrade at End**
    - WHEN: Subscription period ends (canceled)
    - THEN: Downgrade user to free plan

---

### Hotspots & Risks

🟥 **HOTSPOT 1: Payment Failure**
- **Problem:** Card declined, insufficient funds, technical issues
- **Impact:** Lost revenue, poor UX
- **Mitigation:**
  - Clear error messages with actionable steps
  - Multiple payment methods (card, bank transfer, BLIK)
  - Retry mechanism with user notification
  - Support contact readily available

🟥 **HOTSPOT 2: Recurring Payment Failure**
- **Problem:** Monthly charge fails (expired card, insufficient funds)
- **Impact:** Churn, revenue loss
- **Mitigation:**
  - **Dunning management:** Smart retry logic (3, 7, 14 days)
  - Email notifications (payment failed, retry scheduled, final warning)
  - Grace period (7-14 days before downgrade)
  - Easy payment method update flow

🟥 **HOTSPOT 3: Price Jump After Promo (Month 4)**
- **Problem:** 250 zł → 500 zł (2x increase!) = MASSIVE CHURN RISK
- **Impact:** Users cancel after promo ends
- **Mitigation:**
  - **Graduated pricing** (recommended earlier):
    - Month 1-3: 50% off (250 zł)
    - Month 4-6: 25% off (375 zł)
    - Month 7+: Regular (500 zł)
  - Email campaign before price change:
    - T-30 days: "Your promo ends in 1 month"
    - T-7 days: "Reminder: pricing changes next week"
    - T-1 day: "Tomorrow your price updates to X"
  - Value reminder emails ("You've saved X hours, published X articles")
  - Retention offer at cancellation (additional discount)

🟥 **HOTSPOT 4: Invoice Generation for Polish VAT**
- **Problem:** Polish VAT compliance is strict (NIP required, specific format)
- **Impact:** Legal issues, accounting problems for users
- **Mitigation:**
  - Use compliant invoice service (e.g., InFakt, Fakturownia)
  - Validate NIP format
  - Clear VAT display (netto + VAT 23% = brutto)
  - Monthly invoice auto-generation
  - User access to all historical invoices

🟥 **HOTSPOT 5: Partial Subscription State**
- **Problem:** Payment succeeds but subscription activation fails (system error)
- **Impact:** User paid but doesn't get access
- **Mitigation:**
  - **Idempotency:** Stripe webhook replay handling
  - Transaction wrapping (all-or-nothing)
  - Manual reconciliation tools for support
  - Monitoring + alerts for mismatched states

🟥 **HOTSPOT 6: Cancellation Regret**
- **Problem:** User cancels impulsively, regrets later
- **Impact:** Lost customer
- **Mitigation:**
  - **Cancel at period end** (not immediate) - user keeps access
  - Reactivation flow (easy to undo cancel)
  - Win-back campaigns (email after 30, 60, 90 days)

---

## Summary: Key Insights from Event Storming

### 1. Critical Events (Across All 3 Flows)

**Make-or-Break Moments:**
1. **ArticlePublished** (Content Creation)
   - Core value delivery
   - Must be reliable across all channels

2. **UserVerified** → **OnboardingCompleted** (Onboarding)
   - Activation metric
   - Determines if user becomes active

3. **PaymentSucceeded** → **SubscriptionActivated** (Payment)
   - Revenue generation
   - Must be seamless

### 2. External System Dependencies

**Critical integrations (single point of failure):**
1. **OpenAI API** - content generation (core feature)
2. **Stripe** - payment processing (revenue)
3. **Facebook/LinkedIn/Twitter APIs** - publishing (core value)
4. **WordPress REST API** - website publishing (core value)
5. **Email Service** - communications (user engagement)

**Risk Mitigation:**
- Redundancy (backup AI provider)
- Monitoring + alerts
- Graceful degradation
- Clear error messaging

### 3. User Experience Pain Points (Hotspots)

**Highest Priority Fixes:**
1. **Partial Publishing Success** - confusing UX when some channels fail
2. **Onboarding Drop-off** - too long/complex
3. **Price Jump Shock** - month 4 churn risk (250 → 500 zł)
4. **Payment Failure Handling** - poor retry UX
5. **OAuth Token Expiration** - silent publishing failures

### 4. Policy Opportunities (Automation)

**High-Value Policies to Implement:**
1. **Smart Retry Logic** - payment failures, API failures
2. **Progressive Engagement** - drip emails based on usage
3. **Churn Prevention** - retention offers, win-back campaigns
4. **Usage Optimization** - remind users of unused article quota
5. **Renewal Reminders** - reduce involuntary churn

### 5. Data Consistency Challenges

**Eventual Consistency Scenarios:**
1. Multi-channel publishing (some succeed, some fail)
2. Payment webhook delays (payment → activation gap)
3. OAuth token refresh (async background job)

**Solution:**
- Saga pattern for distributed transactions
- Status tracking per channel/operation
- Reconciliation jobs (nightly)

---

## Next Steps

Based on Event Storming analysis, recommended priorities:

### Immediate (Week 1-2)
1. ✅ Document current state (this document)
2. Implement monitoring for critical events
3. Add status tracking for multi-channel publishing

### Short-term (Month 1)
4. Improve partial publishing UX (per-channel status)
5. Implement graduated pricing (prevent month 4 churn)
6. Enhanced payment failure handling

### Mid-term (Month 2-3)
7. Onboarding simplification (A/B test)
8. OAuth token monitoring + auto-refresh
9. Dunning management (smart retry)

### Long-term (Month 3-6)
10. Saga pattern for distributed operations
11. Advanced retention policies
12. Analytics dashboard per aggregate

---

**Koniec analizy Event Storming**
