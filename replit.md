# CertFlow

A Russian-language certificate management and generation system for beauty studios, courses, and service businesses.

## Run & Operate

- `pnpm --filter @workspace/certflow run dev` — run the CertFlow frontend (port assigned by workflow)
- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- Frontend: React 18 + Vite + Tailwind CSS v4
- State: React useState + localStorage persistence
- Image generation: HTML5 Canvas API (client-side)
- Fonts: Google Fonts (Caveat, Bad Script, Marck Script, Montserrat, Playfair Display)
- API: Express 5 (not used by frontend yet — app is client-side only)

## Where things live

- `artifacts/certflow/src/App.tsx` — main app with tab routing (dashboard, builder, generator, ledger)
- `artifacts/certflow/src/components/Dashboard.tsx` — template gallery
- `artifacts/certflow/src/components/Builder.tsx` — canvas-based template editor with drag-to-position
- `artifacts/certflow/src/components/Generator.tsx` — real-time preview + PNG generation
- `artifacts/certflow/src/components/Ledger.tsx` — searchable certificate registry
- `artifacts/certflow/src/utils.ts` — SVG templates, Canvas rendering, localStorage helpers
- `artifacts/certflow/src/types.ts` — shared TypeScript types

## Architecture decisions

- Pure client-side: all data stored in localStorage (`cf_templates`, `cf_issued`); no backend calls
- SVGs for demo templates use base64 encoding (`btoa(unescape(encodeURIComponent(svg)))`) to handle Cyrillic characters in data URLs
- Builder uses HTML5 Canvas for drag-to-position field calibration; Generator uses CSS overlays for real-time preview and Canvas for final PNG export
- Web Share API used for native sharing on mobile; falls back to download link on desktop
- Verification codes follow format `CF-XXX-XXX` (random alphanumeric)

## Product

- **Dashboard**: Gallery of certificate templates (2 built-in demo blanks + user-created)
- **Builder**: Upload a certificate image, drag to position Name/Service/Expiry text fields, set font size/family/color, save as named preset
- **Generator**: Select a template, fill in client details, see real-time CSS overlay preview, generate high-quality PNG with verification code, download or share
- **Ledger**: Searchable registry of all issued certificates with active/redeemed status toggle and delete

## User preferences

_Populate as you build — explicit user instructions worth remembering across sessions._

## Gotchas

- After changing `utils.ts` DEFAULT_TEMPLATES, the app hot-reloads but localStorage-saved user templates persist separately
- SVG data URLs with Cyrillic/Unicode must use base64 encoding, not `utf8,` scheme
- The Builder canvas drag events need `passive: false` on touchmove to prevent scroll interference
