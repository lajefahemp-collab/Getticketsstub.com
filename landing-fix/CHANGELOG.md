# TicketStub launch-blocker fixes

Two patches for `Jevelopers-neatHack-2026/getticketstub.com`, based on `main` at `5e991cd`.
Apply in a checkout of that repo:

```bash
git checkout -b fix/launch-blockers
git am path/to/0001-*.patch path/to/0002-*.patch
```

## 0001 — Slide performers fully on screen
- `src/customer/landing/landing.script.ts`: `placeFigure` keeps every performer whole. The ballerina now fits fully between the menu and the info panel. Side-cropped figures shrink around their hand point, and the basketball player sits above the info panel.
- `src/customer/landing/landing.css`: the bottom fade now applies only to the photos that are cut off at the waist or shin.

## 0002 — Launch blockers from the fix brief
**P0-1 loader.** Root cause: when WebGL is unavailable, `useStatic()` read `sceneEls`, but that variable only exists inside the 3D code, so it threw a `ReferenceError`. The loader's safety timer had already been cleared at that point, so `#loader` covered the page forever.
- `landing.script.ts`: the fallback now uses `qsa('.scene .info')`. The loader lifts in a `finally` block, and also when `run()` fails for any reason. The safety timer is 8s → 6s, and dismissal now sets `aria-hidden`.
- `landing.css`: `#loader[hidden]` now really hides, and `.done` drops pointer events. A CSS-only failsafe lifts the loader and the headline if the script never runs at all.

**P0-2 legal.** `/privacy` and `/terms` already existed with real content.
- `next.config.ts`: `/privacy-policy`, `/terms-of-service` and `/terms-and-conditions` permanently redirect to them.
- Terms: new "A keepsake, not a ticket" section, plus "Refunds and reprints" and "Governing law" placeholders.
- Privacy: new "How long we keep it" section.

**P1-1.** New `src/app/robots.ts` (allows `/`, disallows share, claim, account, cart, studio, API and other private routes) and `src/app/sitemap.ts` (`/`, `/make`, `/help`, `/privacy`, `/terms`).

**P1-2.** The $9 price matches `priceMinor: 900`, and the CTA already says "keepsake, not an admission ticket".
- Landing footer: new Help column with Help & FAQ, `hello@getticketstub.com`, Privacy and Terms.
- `/order/*` temporarily redirects to `/make`.

**Owner items.** Open decisions are listed in `OWNER_DECISIONS.md` and marked `OWNER INPUT REQUIRED` in the code. No prices, shipping details or legal terms were invented.

## Verification
- `npm run lint`: 0 errors (1 warning, in an untouched fixture file). `npm run typecheck`: clean. `npm test`: 815 pass, 0 fail. `npm run build`: success.
- Production server smoke: `/`, `/privacy`, `/terms`, `/robots.txt` (text/plain), `/sitemap.xml` and `/api/health` return 200. `/privacy-policy` and `/terms-of-service` return 308 to the real pages. `/order/personal/` ends at `/make`. Unknown routes still show the 404 page.
- Loader time from load to gone, before → after:
  - WebGL disabled: never → about 2–3s
  - WebGL on: 4.6s (unchanged)
  - JS disabled: never → about 6s
- With WebGL disabled, the neatHack 2026, The Pale Hours and Harbour FC demos open `/studio/demo-*` on desktop and on 390px mobile. The paste field is visible, and no page errors appear.
