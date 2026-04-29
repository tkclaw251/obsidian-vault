---
title: "PesaTrack README"
date: 2026-04-02
tags: [project, venture]
created: 2026-04-02
---

# PesaTrack

Personal finance tracker for mobile money users in East Africa. Automatically parses MTN MoMo SMS receipts into structured transactions, categorizes spending, and presents a monthly financial dashboard.

## Features

- Automatic SMS parsing (6 MTN MoMo Rwanda formats)
- Smart categorization with learning
- Monthly dashboard with charts
- Budget tracking with alerts
- Transaction tagging
- Recurring transaction detection

## Setup

```bash
flutter pub get
dart run build_runner build --delete-conflicting-outputs
flutter run
```

## Tech Stack

- Flutter + Dart
- Drift (SQLite)
- Riverpod (state management)
- fl_chart (charts)
- Kotlin (Android native SMS/notification access)
