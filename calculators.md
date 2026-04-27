---
layout: default
title: SMV, Piece-Rate & Bundle ID Calculators
description: Open-source calculators for garment industry SMV, piece-rate wages, and bundle IDs. Free NPM packages and online tools.
permalink: /calculators.html
---

# Calculators & Formulas

[← Back to home](/scan-erp-docs/)

## SMV (Standard Minute Value)

**Formula:** `SMV = Basic Time × (1 + Allowance%)`

Where:
- Basic Time = Observed Time × Performance Rating
- Allowance% = typically 30% (personal + fatigue + delay)

### Free Tools

- 🌐 [Online SAM/SMV Calculator](https://scan-erp.web.app/blog/sam-smv-calculation-garment-industry.html)
- 📦 [garment-smv-calculator NPM package](https://www.npmjs.com/package/garment-smv-calculator)

### Quick Code Example

```javascript
const { calculateSMV } = require('garment-smv-calculator');
const smv = calculateSMV({
  observedTimeSeconds: 18,
  performanceRating: 1.05,
  allowancePercent: 0.30,
});
// → 0.41 minutes per piece
```

[Read full SMV guide →](https://scan-erp.web.app/blog/sam-smv-calculation-garment-industry.html)

## Piece-Rate Wage

**Formula:** `pieces × rate × skill multiplier + machine bonus + efficiency bonus − quality penalty`

### Free Tools

- 🌐 [Online Piece-Rate Calculator](https://scan-erp.web.app/tools/piece-rate-calculator.html)
- 📦 [garment-piece-rate NPM package](https://www.npmjs.com/package/garment-piece-rate)

### Quick Code Example

```javascript
const { calculatePieceRate } = require('garment-piece-rate');
const wage = calculatePieceRate({
  pieces: 250,
  ratePerPiece: 2.50,
  skillLevel: 'expert',
  qualityScore: 95,
  efficiencyPercent: 110,
  machineType: 'OVERLOCK_5THREAD',
});
// → { gross: 906.95, breakdown: {...} }
```

[Read full piece-rate guide →](https://scan-erp.web.app/blog/piece-rate-payment-calculation-garment-factory.html)

## Bundle ID

**Format:** `STYLE-LOT-COLOR-SIZE-BUNDLE#-COMPONENT`

Example: `S27-8082-BLUE-M-001-FRT`

### Free Tools

- 📦 [garment-bundle-id NPM package](https://www.npmjs.com/package/garment-bundle-id)

### Quick Code Example

```javascript
const { generateBundleId, parseBundleId } = require('garment-bundle-id');
const id = generateBundleId({
  style: 'S27', lot: '8082', color: 'BLUE',
  size: 'M', bundleNumber: 1, component: 'FRT'
});
// → "S27-8082-BLUE-M-001-FRT"
```

[Read full QR bundle tracking guide →](https://scan-erp.web.app/blog/qr-code-production-tracking-garment-factory.html)

## Related Reading

- [How to Calculate SMV (Stopwatch Method)](https://scan-erp.web.app/blog/how-to-calculate-smv-stopwatch-method-garment-industry.html)
- [SMV Calculation Software](https://scan-erp.web.app/blog/smv-calculation-software-garment-industry.html)
- [SAM Values Reference Table](https://scan-erp.web.app/blog/sam-values-basic-garments-reference-table.html)
- [SMV Costing for CMT Pricing](https://scan-erp.web.app/blog/smv-costing-cmt-pricing-garment-factory.html)

[← Back to home](/scan-erp-docs/)
