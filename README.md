# Projeye Başlarken Rehberi

**Bu doküman, profesyonel bir frontend projesine adım adım nasıl başlanacağını ve sürdürülebilir bir yapı için izlenmesi gereken en iyi uygulamaları özetler.**

---

## 📖 İçindekiler

1. [Giriş ve Hedefler](#giriş-ve-hedefler)
2. [Proje Planlama](#proje-planlama)
3. [Teknoloji Seçimi](#teknoloji-seçimi)
4. [Repository ve Versiyon Kontrol](#repository-ve-versiyon-kontrol)
5. [Geliştirme Ortamı Kurulumu](#geliştirme-ortamı-kurulumu)
6. [Proje Scaffold ve Klasör Yapısı](#proje-scaffold-ve-klasör-yapısı)
7. [Kod Standartları ve Linting](#kod-standartları-ve-linting)
8. [İlk Özellik Geliştirme Akışı](#ilk-özellik-geliştirme-akışı)
9. [Test ve Kalite Güvencesi](#test-ve-kalite-güvencesi)
10. [CI/CD Entegrasyonu](#cicd-entegrasyonu)
11. [Deploy ve İzleme](#deploy-ve-izleme)
12. [Dokümantasyon ve Bakım](#dokümantasyon-ve-bakım)

---

## 1. Giriş ve Hedefler

* **Amaç**: Projenin çözmesi gereken sorun ve sağlanacak değer netleştirilmeli.
* **Kullanıcı**: Hedef kullanıcı kitlesi ve beklentileri tanımlanmalı.
* **Kapsam**: MVP (Minimum Viable Product) için öncelikli özellikler listelenmeli.

## 2. Proje Planlama

* **Kullanıcı Hikâyeleri**: ‘‘Kullanıcı sisteme giriş yapabilmeli’’ gibi madde bazlı ifadeler.
* **Fonksiyonel Gereksinimler**: Auth, veritabanı CRUD, UI akışları.
* **Zaman Çizelgesi**: Sprint veya iterasyon planı oluşturun.

## 3. Teknoloji Seçimi

* **Frontend**: Next.js (Pages Router) veya React App.
* **Dil & Stil**: TypeScript + Tailwind CSS.
* **State Yönetimi**: Redux Toolkit, Context API veya Zustand.
* **Backend/Veri**: Firebase (Auth, Firestore, Storage) veya REST/GraphQL.
* **Test**: Jest, React Testing Library, Cypress.
* **CI/CD**: GitHub Actions + Vercel/Netlify.

## 4. Repository ve Versiyon Kontrol

* **Repo Oluşturma**: `git init`, uzak repo (GitHub) ekleme.
* **.gitignore**: Node\_modules, env dosyaları, build klasörleri.
* **Branch Stratejisi**: `main` (prod), `develop` (test), `feature/<isim>`.
* **Commit Mesaj Kuralları**: Conventional Commits veya benzeri format.

## 5. Geliştirme Ortamı Kurulumu

* **Node & Paket Yöneticisi**: Node.js ≥16, npm veya yarn.
* **Env Dosyası**: `.env.local` şablonunu `.env.example`’den kopyalayın.
* **Editor Ayarları**: VSCode, ESlint, Prettier eklentileri.

## 6. Proje Scaffold ve Klasör Yapısı

Temel scaffold komutu:

```bash
npx create-next-app@latest . --typescript
```

Önerilen klasör yapısı:

```plaintext
project-root/
├── .firebaserc                     # Firebase proje alias’ları
├── firebase.json                   # Firebase CLI konfigürasyonu (hosting, functions, emulators)
├── functions/                      # Cloud Functions kodları (opsiyonel)
│   ├── package.json
│   └── src/
│       └── index.ts                # Function giriş noktası (ör. auth triggers)
│
├── public/                         # Statik olarak sunulan dosyalar
│   ├── favicon.ico
│   ├── favicon-16x16.png
│   ├── favicon-32x32.png
│   ├── robots.txt
│   ├── site.webmanifest
│   ├── locales/                    # i18n çeviri dosyaları
│   │   ├── en/
│   │   │   └── common.json
│   │   └── tr/
│   │       └── common.json
│   └── assets/                     # Doğrudan URL ile erişilen medya
│       ├── images/
│       ├── icons/
│       ├── fonts/
│       └── videos/
│
├── src/                            # Uygulama kodu
│   ├── firebase/                   # Firebase SDK wrapper’ları
│   │   ├── config.ts               # initializeApp, auth, db, storage
│   │   ├── auth.ts                 # signIn/up/out, onAuthStateChanged
│   │   ├── firestore.ts            # Firestore CRUD fonksiyonları
│   │   └── storage.ts              # Storage upload/download helper’ları
│   │
│   ├── pages/                      # Next.js Pages Router
│   │   ├── _app.tsx                # Global layout ve provider’lar
│   │   ├── _document.tsx           # HTML şablonu, meta, font preload
│   │   ├── index.tsx               # Ana sayfa (public)
│   │   ├── login.tsx               # Kullanıcı giriş sayfası
│   │   ├── register.tsx            # Kayıt sayfası
│   │   ├── forgot-password.tsx     # Şifremi unuttum akışı
│   │   ├── dashboard/              # Giriş sonrası kullanıcı paneli
│   │   │   ├── index.tsx           # Dashboard ana sayfası
│   │   │   ├── profile.tsx         # Profil düzenleme
│   │   │   ├── settings.tsx        # Hesap ayarları
│   │   │   └── ...                 # Diğer dashboard bölümleri
│   │   ├── products/               # Ürün liste + detay sayfaları
│   │   │   ├── index.tsx           # /products
│   │   │   └── [id].tsx            # /products/[id]
│   │   └── api/                    # Next.js API routes (opsiyonel)
│   │       └── auth/               # Örn. /api/auth/send-reset-email
│   │
│   ├── components/                 # Atomic Design hiyerarşisi
│   │   ├── atoms/                  # Button, Input, Icon, Text…
│   │   ├── molecules/              # Card, Modal, FormField…
│   │   ├── organisms/              # Navbar, Footer, Sidebar, Form…
│   │   └── templates/              # Layout, PageTemplate, GridTemplate
│   │
│   ├── features/                   # Feature-based modüller
│   │   ├── auth/                   # Kimlik doğrulama akışları
│   │   │   ├── components/         # LoginForm, RegisterForm…
│   │   │   ├── hooks/              # useLogin, useRegister…
│   │   │   └── services/           # authService wrapper’ları
│   │   ├── dashboard/              # Dashboard’a özel logic
│   │   │   ├── components/         # StatsCard, ActivityFeed…
│   │   │   ├── hooks/              # useUserData, useNotifications…
│   │   │   └── services/           # dashboardService
│   │   └── cart/                   # (opsiyonel) e-ticaret sepeti
│   │
│   ├── hooks/                      # Genel custom React hook’lar
│   │   ├── useAuth.ts              # AuthContext tüketimi + yönlendirme
│   │   ├── useFetch.ts             # HTTP istekleri
│   │   └── useTheme.ts             # Dark/Light mode management
│   │
│   ├── context/                    # React Context sağlayıcıları
│   │   ├── AuthContext.tsx         # Kullanıcı bilgilerini depolar
│   │   └── ThemeContext.tsx        # Tema ayarları
│   │
│   ├── services/                   # API & entegrasyon katmanı
│   │   ├── apiClient.ts            # axios/fetch ortak ayarlar
│   │   ├── productsService.ts
│   │   └── userService.ts          # Kullanıcı CRUD, profil güncelleme
│   │
│   ├── utils/                      # Yardımcı fonksiyonlar, validatörler
│   │   ├── formatDate.ts
│   │   ├── logger.ts
│   │   └── validators.ts
│   │
│   ├── store/                      # Global state (Redux, Zustand, vb.)  
│   │   ├── index.ts                # Store oluşturma
│   │   └── rootReducer.ts          # Redux reducers  
│   │
│   ├── styles/                     # Global & modul CSS/SCSS/Tailwind
│   │   ├── globals.css             # Reset & base
│   │   ├── tailwind.css            # Tailwind direktifleri
│   │   ├── variables.css           # Tema renk / font değişkenleri
│   │   └── components/             # Button.module.css, Card.module.css
│   │
│   ├── assets/                     # Bundler’ın işleyeceği varlıklar
│   │   ├── images/                 # import edilen SVG/PNG
│   │   ├── icons/
│   │   └── fonts/
│   │
│   ├── types/                      # TypeScript tip tanımları
│   │   ├── next.d.ts               # Next.js augmentations
│   │   └── models/                 # User.ts, Product.ts, Dashboard.ts
│   │
│   ├── data/                       # Statik/mock JSON veriler
│   │   └── mockUsers.json
│   │
│   └── tests/                      # Birim ve entegrasyon testleri
│       ├── unit/
│       │   ├── components/         # Button.test.tsx
│       │   └── pages/              # login.test.tsx
│       └── e2e/                    # Cypress/Playwright
│           └── auth.spec.ts
│
├── scripts/                        # Build/deploy yardımcı script’ler
│   ├── build-docs.sh
│   └── deploy.sh
│
├── docs/                           # Proje dokümantasyonu
│   ├── architecture.md
│   ├── code-style.md
│   └── contribution.md
│
├── .github/                        # CI/CD tanımları
│   └── workflows/
│       ├── ci.yml
│       └── cd.yml
│
├── .env                            # Çevresel değişkenler (gitignore)
├── .env.example                    # Paylaşılabilir örnek
├── .eslintrc.json                  # ESLint kuralları
├── .prettierrc                     # Prettier ayarları
├── .stylelintrc                    # Stylelint kuralları
├── jest.config.js                  # Jest konfigürasyonu
├── cypress.config.ts               # Cypress konfigürasyonu
├── next.config.mjs                 # Next.js yapılandırması
├── tsconfig.json                   # TypeScript ayarları
├── package.json                    # Bağımlılıklar & script’ler
├── package-lock.json               # Kilitlenmiş bağımlılıklar
└── README.md                       # Proje açıklaması, kurulum ve kullanım


## 7. Kod Standartları ve Linting

* **ESLint**: `eslint --init` veya topluluk ayarları.
* **Prettier**: `.prettierrc` ile kod biçimlendirme.
* **Husky & Commitlint**: Pre-commit hook, mesaj doğrulama.

## 8. İlk Özellik Geliştirme Akışı

1. Yeni bir feature branch açın: `git checkout -b feature/login`
2. Bileşen, sayfa ve stil dosyalarını ilgili klasöre ekleyin.
3. State ve API katmanını oluşturun.
4. Lokal olarak test edin, F5 ile UI akışını doğrulayın.
5. Commit mesajı yazın, PR açın.

## 9. Test ve Kalite Güvencesi

* **Unit Test**: Jest + React Testing Library.
* **E2E Test**: Cypress veya Playwright.
* **Coverage**: %80+ hedefleyin.
* **Lint & Type Check**: `npm run lint`, `npm run type-check`.

## 10. CI/CD Entegrasyonu

* **GitHub Actions**: CI workflow (lint, test, build).
* **Preview Deploy**: Her PR için preview URL.
* **Prod Deploy**: `main`’e merge’de otomatik deploy.

## 11. Deploy ve İzleme

* **Hosting**: Vercel veya Netlify.
* **Analytics**: Google Analytics, Sentry.
* **Lighthouse**: Performans izleme.

## 12. Dokümantasyon ve Bakım

* **README** güncel kalsın.
* **Storybook** veya benzeri component dokümantasyonu.
* **Kod Review**: Haftalık veya sprint sonunda.

---

**Bu rehber, projeyi planlama aşamasından deploy’a kadar tam bir süreç tanımı sağlar. İyi kodlamalar!**
