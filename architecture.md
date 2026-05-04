---
layout: default
title: Scan ERP Architecture — Tech Stack, Hardware, Data Flow
description: Complete architecture overview of Scan ERP — Next.js, Firebase, Raspberry Pi edge cache, ESP32-CAM scanners, ZKTeco biometric integration.
permalink: /architecture.html
---

# Architecture

[← Back to home](/scan-erp-docs/)

## High-Level Diagram

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

## Tech Stack

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

## Why Edge-Cached Architecture

Internet drops 3-5 times daily in Bangladesh and parts of India/Cambodia. Cloud-only systems pause production during outages. Scan ERP's Raspberry Pi maintains local cache and continues scanning — synced when connection returns.

## Why 3-Decoder Fallback

Different Android phones detect QR codes with different libraries best:
- High-end Pixel phones: BarcodeDetector (browser-native)
- Mid-tier Snapdragon 6xx: jsQR (JavaScript) most reliable
- Older devices: ZXing has best low-resolution handling

Running all 3 in parallel and taking the first success: 95-99% accuracy on $50 phones.

[Read more on QR scanning →](https://scanerp.pro/blog/qr-code-production-tracking-garment-factory.html)

[← Back to home](/scan-erp-docs/)
