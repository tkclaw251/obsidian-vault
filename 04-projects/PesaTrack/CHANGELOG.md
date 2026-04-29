---
title: "PesaTrack Changelog"
date: 2026-04-02
tags: [project, venture, changelog]
created: 2026-04-02
---

# Changelog

## v0.6.0 (2026-04-02)

### Added
- Android transaction overlay service for incoming MoMo notifications with quick actions
- Overlay permission bridge methods on platform channel
- Settings toggle to enable/disable transaction overlay mode
- Overlay implementation spec document at docs/overlay_spec.md

### Changed
- Notification listener now throttles and deduplicates overlay prompts to avoid spam
- Ingestion review prompt falls back to in-app bottom sheet when overlay mode is disabled

## v0.5.0 (2026-03-30)

### Added
- Manrope + Plus Jakarta Sans typography system
- Glassmorphic bottom navigation bar with backdrop blur
- Gradient hero balance card on dashboard
- AI Learning Loop banner on uncategorized queue
- Date-grouped transaction list with section headers

### Changed
- Complete visual overhaul: "Digital Curator" design system, primary #006E2F
- All screens updated with new typography, tonal surface hierarchy, no-border design

## v0.4.0 (2026-03-29)

### Added
- iOS project scaffolding with deployment target 15.0
- Share Extension (PesaTrackShare): receives text shared from Messages app
- App Group container for main app ↔ extension communication
- iOS onboarding variant: "Share from Messages" flow

## v0.3.0 (2026-03-28)

### Added
- SMS Direct ingestion path
- NotificationListener ingestion path
- XML Import ingestion path
- All 5 ingestion sources implemented with TransactionSource interface

### Changed
- All ingestors follow the same TransactionSource interface
- Duplicate detection across all ingestion paths
