<div align="center">
  <h1>Scanvice</h1>
  <p><b>AI receipt and invoice scanner.</b><br/>Upload a receipt image and get structured, exportable data back — powered by Claude vision.</p>
  <p>
    <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/License-MIT-blue.svg"></a>
    <img alt="Next.js 16" src="https://img.shields.io/badge/Next.js-16-000000?logo=nextdotjs&logoColor=white">
    <img alt="React 19" src="https://img.shields.io/badge/React-19-61DAFB?logo=react&logoColor=black">
    <img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-3178C6?logo=typescript&logoColor=white">
    <img alt="Tailwind CSS v4" src="https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?logo=tailwindcss&logoColor=white">
  </p>
</div>

---

**Scanvice** is a landing site plus an AI-powered receipt and invoice scanner. Upload a
photo of a receipt, invoice or bill and the app calls Anthropic's Claude vision model to
read it and return clean, structured JSON — vendor, transaction details, totals, line
items, a category guess and a per-field confidence score — which is rendered in the UI
and can be exported (e.g. to XLSX).

## Features

- **Receipt/invoice extraction.** `POST /api/extract-receipt` sends the uploaded image
  to Claude Sonnet 4.5 (vision) and returns structured data matching a typed
  `ExtractedReceipt` schema.
- **Rich structured output.** Vendor info, transaction details, totals and an amount
  breakdown, itemised line items, a category guess and per-field confidence scores;
  low-quality images still extract with lowered confidence and warnings.
- **Rendered results.** Vendor card, transaction details, line-items table, amount
  breakdown, confidence indicators and warning banners.
- **Export.** One-click export of results, including XLSX (via [SheetJS](https://sheetjs.com/)).
- **Server-side guardrails.** Request-level rate limiting, image MIME-type and size
  validation (10 MB max), a request timeout, and metadata-only logging — receipt
  content is not retained.
- **Marketing site + blog.** Landing sections (hero, problem/solution, trust, CTA) and
  an MDX-style blog, with SEO built in (`sitemap.ts`, `robots.ts`, dynamic OG icon).

## Tech stack

- [Next.js 16](https://nextjs.org/) (App Router) with [React 19](https://react.dev/) and [TypeScript](https://www.typescriptlang.org/)
- [Tailwind CSS v4](https://tailwindcss.com/) with [shadcn/ui](https://ui.shadcn.com/) + Radix primitives
- [Anthropic Claude](https://www.anthropic.com/) (`claude-sonnet-4-5`) vision via the Messages API
- [SheetJS (`xlsx`)](https://sheetjs.com/) for spreadsheet export, [Recharts](https://recharts.org/), [Framer Motion](https://www.framer.com/motion/), [Vercel Analytics](https://vercel.com/analytics)

## Getting started

### Requirements

- Node.js 18.18+ (Next.js 16)
- An [Anthropic API key](https://console.anthropic.com/)

### 1. Install

```bash
npm install
```

### 2. Configure environment

Create a `.env.local` file in the project root:

```dotenv
ANTHROPIC_API_KEY=sk-ant-...
```

`ANTHROPIC_API_KEY` is **required** — the extraction endpoint returns a
`server_misconfigured` error without it.

### 3. Run

```bash
npm run dev      # http://localhost:3000
```

### Build

```bash
npm run build
npm run start
npm run lint
```

## How extraction works

`app/api/extract-receipt/route.ts` runs on the Node.js runtime. It validates the upload
(accepted image MIME types, 10 MB limit), applies a per-IP rate limit, and calls the
Anthropic Messages API with the image and a strict extraction prompt (`lib/receipt.ts`).
The model returns JSON only, which is parsed into the `ExtractedReceipt` type and sent
back to the client for rendering and export.

## License

Released under the [MIT License](LICENSE) © 2026 Olivier Lüthy. You're free to use, modify and distribute this
software, including commercially, as long as the copyright notice and license are included.

## Author

Built by **Olivier Lüthy** — [GitHub](https://github.com/olivierluethy).
