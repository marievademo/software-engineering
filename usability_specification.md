# Usability Specification — Restaurant Ordering System
> Web App | Group of 3 | Version 1.0  
> Based on: Guidelines & Principles v1.0 + Wireframes (Home, Menu, Winkelmandje)

---

## Purpose

This document defines **testable criteria** to verify that the implemented web app meets the guidelines and wireframe designs. Each requirement has a clear pass/fail condition so it can be checked during testing.

---

## 1. Consistency Requirements

| ID | Requirement | How to Test | Pass Condition |
|----|-------------|-------------|----------------|
| C-01 | Every screen has the same header (logo + table number) | Open all 3 screens and visually compare header | Header is pixel-consistent across all screens |
| C-02 | Bottom navigation bar is identical on all screens | Compare navbar on Home, Menu, Mandje | Same 4 icons (Home, Menu, Mandje, Profiel), same order, same style |
| C-03 | Active page is visually highlighted in the navbar | Navigate to each page | Active tab is clearly distinguishable from inactive tabs |
| C-04 | All buttons use the same style (rounded, filled/outlined) | Inspect Add (+), Bestel, Bekijk mandje buttons | No button deviates from the defined style |
| C-05 | Only one icon library is used throughout the app | Inspect all icons on all screens | All icons come from the same library (e.g., all Font Awesome) |
| C-06 | Font is consistent across all pages | Inspect headings and body text on all screens | Maximum 2 fonts, used consistently |

---

## 2. Layout Requirements

| ID | Requirement | How to Test | Pass Condition |
|----|-------------|-------------|----------------|
| L-01 | Home screen is full-width centered layout with hero banner | Open home screen | Hero banner spans full width, content is centered |
| L-02 | Menu screen has category pills/tabs at the top | Open menu screen | Category pills (Alle, Pizza, Pasta, Salade, etc.) are visible and functional |
| L-03 | Menu screen shows a product grid with + buttons | Open menu screen | Each item has a visible + button to add to cart |
| L-04 | Winkelmandje shows table number, item list, quantity controls, and order button | Open mandje screen | All 4 elements present and visible without scrolling (on a standard tablet) |
| L-05 | Footer is present and identical on all screens | Scroll to bottom of each screen | Footer contains restaurant name, address, and disclaimer on every page |
| L-06 | "Bekijk mandje" call-to-action is visible on the menu screen | Open menu screen | Mandje bar/button is visible without scrolling |

---

## 3. Navigation Requirements

| ID | Requirement | How to Test | Pass Condition |
|----|-------------|-------------|----------------|
| N-01 | Customer can go from Home to Menu in one tap | Tap "Bekijk menu →" on home screen | Menu screen opens immediately |
| N-02 | Customer can reach Mandje from Menu in one tap | Tap "Bekijk mandje →" on menu screen | Mandje screen opens immediately |
| N-03 | Customer can navigate between all main screens via bottom navbar | Tap each icon in navbar | Correct screen opens for each icon |
| N-04 | Customer can return to a previous screen without losing cart contents | Navigate away from mandje and return | Cart items are still present on return |
| N-05 | Table number can be changed from the Mandje screen | Tap "Wijzigen" next to table number (T4) | Table number can be updated |

---

## 4. Usability & Clarity Requirements

| ID | Requirement | How to Test | Pass Condition |
|----|-------------|-------------|----------------|
| U-01 | All labels are self-explanatory without prior instruction | Ask a test user unfamiliar with the app to place an order | User completes order without asking for help |
| U-02 | Empty cart shows a helpful message, not a blank screen | Open mandje with no items added | A message like "Je mandje is leeg" is displayed |
| U-03 | Adding an item gives visual feedback | Tap + on a menu item | Cart badge or counter updates immediately |
| U-04 | Submitting an order shows a confirmation | Tap "Bestelling plaatsen" in mandje | A confirmation screen or message is shown |
| U-05 | Error messages are human-readable | Trigger an error (e.g., order with 0 items) | Error is written in plain language, no technical codes |
| U-06 | Prices are always shown with € symbol and 2 decimal places | Inspect all menu items and cart totals | No price is shown without € or with incorrect decimals |

---

## 5. Mobile / Touch Requirements

| ID | Requirement | How to Test | Pass Condition |
|----|-------------|-------------|----------------|
| M-01 | All tap targets (buttons, icons, + controls) are at least 44px tall | Inspect element height in browser dev tools | No interactive element is smaller than 44px |
| M-02 | No interaction requires a hover state | Test on a touchscreen device | All functionality works without hover |
| M-03 | Body text is minimum 16px | Inspect font sizes in dev tools | No body text smaller than 16px |
| M-04 | Quantity controls (– 1 +) are easy to tap without mis-tapping | Test on a physical device | User can adjust quantity without accidental taps |

---

## 6. Performance Targets

These are the **quantitative performance criteria** for the user interface:

| ID | Metric | Target | Measurement Method |
|----|--------|--------|--------------------|
| P-01 | **Page load time** (Home screen) | ≤ 2 seconds on a standard Wi-Fi connection | Browser DevTools → Network tab → DOMContentLoaded |
| P-02 | **Page load time** (Menu screen, including images) | ≤ 3 seconds on standard Wi-Fi | Browser DevTools → Load event |
| P-03 | **Time to interactive** (user can tap a menu item) | ≤ 3 seconds after page load | Lighthouse → Time to Interactive metric |
| P-04 | **Cart update response time** (after tapping +) | ≤ 200ms visual feedback | Manual stopwatch or browser performance profiler |
| P-05 | **Order submission response** (after tapping "Bestelling plaatsen") | ≤ 1 second to confirmation screen | Manual timing from tap to confirmation visible |
| P-06 | **Largest Contentful Paint (LCP)** | ≤ 2.5 seconds | Lighthouse audit |
| P-07 | **Cumulative Layout Shift (CLS)** | Score ≤ 0.1 (no jumping elements) | Lighthouse audit |
| P-08 | **App works on screen widths from 360px to 1200px** | No layout breaks | Resize browser window and test at 360px, 768px, 1200px |

---

## 7. Content Requirements

| ID | Requirement | How to Test | Pass Condition |
|----|-------------|-------------|----------------|
| CT-01 | All menu items have: name, short description (max 2 lines), price, image | Inspect each item on menu screen | No item is missing any of the 4 elements |
| CT-02 | No placeholder text ("Lorem ipsum") appears anywhere | Read all text on all screens | Zero instances of placeholder text |
| CT-03 | Allergen disclaimer is present | Check footer or menu screen | Text like "Allergieën? Vraag ons personeel." is visible |
| CT-04 | Consistent language is used throughout (NL or EN, not mixed) | Read all text on all screens | No language mixing found |

---

## 8. Test Summary Checklist

Use this checklist during a final review session before submission:

- [ ] All C-0x consistency checks passed
- [ ] All L-0x layout checks passed
- [ ] All N-0x navigation checks passed
- [ ] All U-0x usability checks passed
- [ ] All M-0x mobile/touch checks passed
- [ ] All P-0x performance targets met (run Lighthouse audit)
- [ ] All CT-0x content checks passed
- [ ] Tested on at least one real mobile or tablet device
- [ ] All 3 wireframe screens implemented and match wireframe layout
- [ ] Feedback from all group members incorporated

---

*Last updated: February 2026 | Restaurant Ordering System Project*