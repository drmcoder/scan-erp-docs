# Scan ERP — Documentation & Resources

> Public documentation, architecture notes, and resources for [Scan ERP](https://scanerp.pro/) — a QR-based ERP system for CMT garment factories.

[![Website](https://img.shields.io/badge/Website-scan--erp.web.app-2563eb?logo=firefox)](https://scanerp.pro/)
[![Blog](https://img.shields.io/badge/Blog-50%2B%20guides-2563eb?logo=read.cv)](https://scanerp.pro/blog/)

## What is Scan ERP?

Scan ERP is a purpose-built ERP for CMT (Cut-Make-Trim) garment manufacturing factories. Operators scan bundle QR codes with cheap Android phones; the system auto-calculates piece-rate pay, tracks WIP across stations, and runs production line announcements over a Raspberry Pi-based print server.

**Production data:** 115,370+ pieces tracked across our own factory floor with 95-99% scan accuracy on $50 Android phones.

**Built by:** [Santosh Rijal](https://github.com/drmcoder), MBBS doctor and family garment business owner in Nepal. Scan ERP started as our own factory's solution after evaluating PROTRACKER, FastReact, WFX, and Stitch-MES.

## Quick Links

- 🌐 [Live site (scanerp.pro)](https://scanerp.pro/)
- 📰 [Blog with 50+ guides](https://scanerp.pro/blog/)
- 🧮 [Free piece-rate calculator](https://scanerp.pro/tools/piece-rate-calculator.html)
- 📊 [Free SMV/SAM calculator](https://scanerp.pro/blog/sam-smv-calculation-garment-industry.html)

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    Operator Mobile (Browser)                     │
│  Cheap Android Phone — jsQR + BarcodeDetector + ZXing fallback   │
└────────────────────────┬────────────────────────────────────────┘
                         │ HTTPS
                         ↓
┌─────────────────────────────────────────────────────────────────┐
│                Next.js 15 (Vercel) — Operator App                │
│         Bundle scan workflow, work pool, payments               │
└──────┬──────────────────────────────────────────┬───────────────┘
       │                                          │
       ↓                                          ↓
┌──────────────────┐                  ┌────────────────────────┐
│ Firebase Firestore│                  │ Raspberry Pi 4 (Edge)  │
│ + Realtime DB     │←───────SSE──────→│ Cache, print server,   │
│                   │                  │ ZKTeco biometric ADMS  │
└───────────────────┘                  └─────────┬──────────────┘
                                                 │
                                                 ↓
                                       ┌─────────────────────┐
                                       │ Hardware Stations   │
                                       │ - TSC label printer │
                                       │ - Brother A4 printer│
                                       │ - ESP32-CAM scanner │
                                       │ - PA + TTS speaker  │
                                       └─────────────────────┘
```

## Key Features

### 1. Real-Time Bundle Tracking
Every bundle has a unique QR code. Each scan records: who, what, where, when. Bundle locations update across stations within seconds.

[Read more →](https://scanerp.pro/blog/qr-code-production-tracking-garment-factory.html)

### 2. Automated Piece-Rate Payment
Wages calculate from scan data automatically. Eliminates 90%+ of payment disputes that consume traditional wage clerk afternoons.

Formula: `pieces × rate × skill multiplier + bonuses − quality penalty`

[Read more →](https://scanerp.pro/blog/piece-rate-payment-calculation-garment-factory.html)

### 3. Marriage / Convergence Tracking
A single garment is cut into 5-15 components (front, back, sleeves, collar) sewn separately, then joined. Scan ERP tracks all components and confirms when a complete garment is ready.

### 4. Edge-Cached Operation
Raspberry Pi maintains local cache of work pool data. When internet drops (3-5 times daily in Bangladesh), operators continue scanning. Pi syncs when connection returns.

### 5. Multi-Language Operator UI
English + Nepali (Devanagari script). TTS announcements in factory dialect. Designed for low-literacy operators.

### 6. Hardware Integration
- ZKTeco biometric attendance (ADMS protocol)
- TSC label printers (TSPL)
- Brother A4 printers (PCL)
- ESP32-CAM scanner stations
- Factory TV display for line leaderboards

## Technology Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Next.js 15.5, TypeScript, Tailwind CSS |
| State | Zustand, React Context, IndexedDB (Dexie) |
| Backend | Firebase Firestore + Realtime DB + Auth |
| Edge | Raspberry Pi 4 (Express + pm2), Cloudflare Tunnel |
| Hardware | ESP32-CAM, ZKTeco, TSC TSPL, Brother PCL |
| Analytics | Firebase Analytics, GA4 |
| QR Decoding | jsQR + BarcodeDetector + ZXing (3-decoder fallback) |
| i18n | next-intl (English + Nepali Devanagari) |

## Documentation Index

### Getting Started
- [Garment Factory ERP System: Complete 2026 Guide](https://scanerp.pro/blog/garment-factory-erp-system-complete-guide.html)
- [Real-Time Garment Production Tracking System Guide](https://scanerp.pro/blog/garment-production-tracking-system-2026-guide.html)
- [Cheap ERP for Garment Factory: $200/mo Alternative](https://scanerp.pro/blog/cheap-erp-garment-factory.html)

### Implementation
- [Bundle Tracking System for Garment Factory](https://scanerp.pro/blog/bundle-system-garment-production-tracking.html)
- [QR Code Production Tracking](https://scanerp.pro/blog/qr-code-production-tracking-garment-factory.html)
- [Piece Rate Calculation Software](https://scanerp.pro/blog/piece-rate-payment-calculation-garment-factory.html)
- [WIP Tracking in Garment Factory](https://scanerp.pro/blog/wip-tracking-garment-factory.html)

### SAM/SMV Reference
- [SAM & SMV Formula](https://scanerp.pro/blog/sam-smv-calculation-garment-industry.html)
- [How to Calculate SMV (Stopwatch Method)](https://scanerp.pro/blog/how-to-calculate-smv-stopwatch-method-garment-industry.html)
- [SMV Calculation Software Comparison](https://scanerp.pro/blog/smv-calculation-software-garment-industry.html)
- [SAM Values Reference Table](https://scanerp.pro/blog/sam-values-basic-garments-reference-table.html)
- [SMV Costing for CMT Pricing](https://scanerp.pro/blog/smv-costing-cmt-pricing-garment-factory.html)

### Country Guides
- [Bangladesh RMG Factories](https://scanerp.pro/blog/garment-erp-bangladesh-rmg-factories.html)
- [India (Tirupur, Ludhiana, NCR)](https://scanerp.pro/blog/garment-factory-erp-india-tiruppur.html)
- [Vietnam](https://scanerp.pro/blog/garment-factory-erp-vietnam.html)
- [Cambodia](https://scanerp.pro/blog/garment-factory-erp-cambodia.html)
- [Ethiopia](https://scanerp.pro/blog/garment-factory-erp-ethiopia.html)

### Compliance
- [Digital Product Passport](https://scanerp.pro/blog/digital-product-passport-garment-factory.html)
- [CSRD Reporting](https://scanerp.pro/blog/csrd-garment-factory-reporting.html)
- [CBAM Guide](https://scanerp.pro/blog/cbam-garment-factory-guide.html)
- [Carbon Footprint Tracking](https://scanerp.pro/blog/garment-factory-carbon-footprint-tracking.html)

### Competitor Comparisons
- [PROTRACKER vs Scan ERP](https://scanerp.pro/blog/protracker-alternative-scan-erp-comparison.html)
- [FastReact vs Scan ERP](https://scanerp.pro/blog/fastreact-alternative-scan-erp-comparison.html)
- [WFX Smart Factory vs Scan ERP](https://scanerp.pro/blog/wfx-alternative-scan-erp-comparison.html)
- [ApparelMagic vs Scan ERP](https://scanerp.pro/blog/apparelmagic-alternative-scan-erp-comparison.html)
- [Centric Software vs Scan ERP](https://scanerp.pro/blog/centric-software-alternative-scan-erp.html)
- [Stitch MES vs Scan ERP](https://scanerp.pro/blog/stitch-mes-alternative-scan-erp.html)
- [AIMS360 vs Scan ERP](https://scanerp.pro/blog/aims360-alternative-scan-erp-comparison.html)
- [goRMG vs Scan ERP](https://scanerp.pro/blog/gormg-alternative-scan-erp-bangladesh.html)

## Pricing

| Plan | Monthly | Best fit |
|------|---------|----------|
| Starter | $200/mo | 10-50 sewing machines |
| Growth | $500-1,200/mo | 50-150 machines |
| Scale | $1,500-3,000/mo | 150-300+ machines |

[Detailed pricing →](https://scanerp.pro/pricing.html)

## Contact

- 🌐 [scanerp.pro](https://scanerp.pro/)
- 📱 WhatsApp: +977-9863618347
- 💬 [Free Demo](https://scanerp.pro/#contact)

## Founder Background

[Santosh Rijal](https://github.com/drmcoder) is an MBBS medical doctor and family garment business owner in Nepal. Scan ERP started as Trishakti Apparel's internal tool after evaluating existing options. Featured in [Setopati](https://www.setopati.com/) (2025) for the journey from medicine to garment manufacturing entrepreneurship.

## License

Code: Closed source (commercial product).
Documentation in this repository: [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/).
