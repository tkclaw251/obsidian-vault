---
title: "PesaTrack Project Specification"
date: 2026-04-02
tags: [project, venture, specification]
created: 2026-04-02
---

# PesaTrack - Project Specification for Claude Code

## What Is This Project

PesaTrack is a personal finance app for young professionals in East Africa who use mobile money (MTN MoMo) as their primary financial tool. It automatically parses MoMo SMS/notification receipts into structured transactions, categorizes spending, and presents a clear monthly financial dashboard. Zero manual data entry required.

**Target:** Salaried young professionals in Kigali, Rwanda, age 22-35, using MTN MoMo for 70%+ of transactions.
**Platform:** Android (API 26+). Flutter + native Kotlin module for SMS/notification access.
**Distribution:** Direct APK for pilot (20-50 users), Google Play later.

## Architecture

Five-layer architecture. Code in one layer only talks to the layer directly below it.

```
┌─────────────────────────────────────────┐
│  PRESENTATION (Flutter widgets/screens) │  lib/screens/, lib/widgets/
├─────────────────────────────────────────┤
│  STATE MANAGEMENT (Riverpod providers)  │  lib/providers/
├─────────────────────────────────────────┤
│  DOMAIN / BUSINESS LOGIC (pure Dart)   │  lib/services/
├─────────────────────────────────────────┤
│  DATA (Drift + SQLite)                  │  lib/data/, lib/models/
├─────────────────────────────────────────┤
│  PLATFORM (Kotlin native)               │  android/app/src/main/kotlin/
└─────────────────────────────────────────┘
```

**Critical rule:** The Domain layer (lib/services/) must have ZERO Flutter imports. It is pure Dart. This allows the parser, categorizer, and business logic to be unit tested without a device or emulator.

## Project Structure

```
pesatrack/
  android/
    app/src/main/
      kotlin/com/pesatrack/
        SmsReader.kt                    # SMS reading via ContentResolver (pilot)
        SmsHandlerSwap.kt              # Temporary default SMS handler logic
        NotificationService.kt          # NotificationListenerService
        PlatformBridge.kt              # MethodChannel bridge to Flutter
        XmlImporter.kt                 # SMS Backup & Restore XML parser
      AndroidManifest.xml

  lib/
    main.dart, app_router.dart
    models/ (transaction, category, merchant_alias, raw_message, enums)
    services/ (ingestion/, parser/, categorizer/, duplicate_detector, recurring_detector, budget_calculator, payday_detector)
    data/ (database.dart, daos/)
    providers/ (transaction, category, budget, dashboard, onboarding, settings)
    screens/ (onboarding/, dashboard/, transactions/, uncategorized/, budgets/, recurring/, manual_entry/, settings/)
    widgets/ (transaction_card, category_chip, budget_progress_bar, etc.)
    theme/ (app_theme, app_colors, category_icons)
    utils/ (currency_formatter, date_utils, constants)

  test/ (unit tests for parser, categorizer, duplicate_detector)
  pubspec.yaml
```

## Default Categories

| ID | Display Name | Type | Icon | Color |
|----|-------------|------|------|-------|
| transport | Transport | Expense | 🚗 | #3B82F6 |
| food | Food & Groceries | Expense | 🛒 | #22C55E |
| airtime | Airtime & Data | Expense | 📱 | #8B5CF6 |
| bills | Bills & Utilities | Expense | 💡 | #F59E0B |
| transfer_out | Transfers Out | Expense | ↗️ | #EF4444 |
| transfer_in | Transfers In | Income | ↙️ | #10B981 |
| cash_out | Cash Withdrawal | Expense | 🏧 | #6366F1 |
| cash_in | Cash Deposit | Income | 💵 | #14B8A6 |
| entertainment | Entertainment | Expense | 🎬 | #EC4899 |
| health | Health | Expense | ⚕️ | #06B6D4 |
| education | Education | Expense | 📚 | #F97316 |
| salary | Salary / Income | Income | 💰 | #059669 |
| uncategorized | Uncategorized | Expense | ❓ | #94A3B8 |

## SMS Parser Specification

### MTN MoMo Rwanda Templates

Each template is a named regex with capture groups. The parser tries templates in order and uses the first match.

**Payment to Merchant:**
```
Pattern: You have paid (?P<amount>[\d,]+) Frw to (?P<recipient>.+?)\..*?Fee: (?P<fee>[\d,]+) Frw\..*?Balance: (?P<balance>[\d,]+) Frw\..*?Ref: (?P<ref>\w+)
Type: MERCHANT_PAYMENT
```

**Transfer Out:**
```
Pattern: You have sent (?P<amount>[\d,]+) Frw to (?P<recipient>.+?)\((?P<phone>\d+)\)\..*?Fee: (?P<fee>[\d,]+) Frw\..*?Balance: (?P<balance>[\d,]+) Frw\.
Type: TRANSFER_OUT
```

**Transfer In:**
```
Pattern: You have received (?P<amount>[\d,]+) Frw from (?P<sender>.+?)\((?P<phone>\d+)\)\..*?Balance: (?P<balance>[\d,]+) Frw\.
Type: TRANSFER_IN
```

### Confidence Scoring
| Score | Meaning | Behavior |
|-------|---------|----------|
| 0.9-1.0 | Full match, all fields extracted | Auto-categorize, show normally |
| 0.7-0.89 | Core fields OK, some missing | Auto-categorize, flag for review |
| 0.5-0.69 | Amount OK but type/counterparty ambiguous | Show in uncategorized queue |
| Below 0.5 | Parse failure | Log to FailedParse table, do not create transaction |

## Technology Stack

- **Flutter/Dart:** Riverpod (state), Drift (database), go_router (nav), fl_chart (charts)
- **Kotlin (Android):** NotificationListenerService, ContentResolver, MethodChannel

## Build Phases

1. **Phase 1 (Weeks 1-2):** Foundation — Flutter project, data models, Drift DB, RawMessage interface
2. **Phase 2 (Weeks 2-3):** SMS Parser — MTN MoMo Rwanda regex templates, confidence scoring, FailedParse logging
3. **Phase 3 (Week 3):** Categorization — pipeline (5 levels), Kigali merchant dictionary (50+), duplicate detection
4. **Phase 4 (Weeks 4-5):** Dashboard and Core UI — charts, transaction list, detail sheet, budget screens
5. **Phase 5 (Weeks 6-7):** Onboarding and Ingestion — branching flow, Kotlin NotificationListener, SMS handler swap, XML import
6. **Phase 6 (Weeks 8-10):** Polish — notifications, manual entry, settings, recurring detection, app lock, E2E testing

## Important Notes

- **Local-first:** All data stays on device. No cloud sync in MVP.
- **Offline-first:** All core features work without internet.
- **Privacy:** Only read SMS from known MoMo sender addresses. Never read personal messages.
- **Performance:** SMS backfill should process 6 months of history in under 15 seconds.
- **The parser is the most important code in the entire app.** It must be robust, well-tested, and easy to update when MTN changes their SMS format.
