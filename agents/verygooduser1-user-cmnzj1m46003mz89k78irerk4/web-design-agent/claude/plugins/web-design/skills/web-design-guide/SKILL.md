---
name: web-design-guide
description: Comprehensive guide for building websites with Next.js and Tailwind CSS. Covers project scaffolding, component patterns, typography and color systems, responsive layouts, section blueprints, and dev server management. Use when creating, designing, or iterating on a website.
version: 1.0.0
---

# Web Design Guide

## Quick Start — New Project

```bash
# 1. Scaffold
npx create-next-app@latest my-site --typescript --tailwind --eslint --app --src-dir --no-import-alias

# 2. Install common extras
cd my-site && npm install lucide-react

# 3. Clean boilerplate
# - Replace src/app/page.tsx with your home page
# - Replace src/app/globals.css with Tailwind directives + custom styles
# - Update src/app/layout.tsx with fonts and metadata
```

---

## Project Setup Patterns

### tailwind.config.ts — Custom Design Tokens

```typescript
import type { Config } from "tailwindcss";

const config: Config = {
  content: [
    "./src/pages/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/components/**/*.{js,ts,jsx,tsx,mdx}",
    "./src/app/**/*.{js,ts,jsx,tsx,mdx}",
  ],
  theme: {
    extend: {
      colors: {
        brand: {
          50: "#faf5ff",
          100: "#f3e8ff",
          500: "#a855f7",
          600: "#9333ea",
          700: "#7e22ce",
          900: "#581c87",
        },
        accent: "#f59e0b",
      },
      fontFamily: {
        display: ["var(--font-display)", "serif"],
        body: ["var(--font-body)", "sans-serif"],
      },
      animation: {
        "fade-in": "fadeIn 0.5s ease-out",
        "slide-up": "slideUp 0.6s ease-out",
      },
      keyframes: {
        fadeIn: {
          "0%": { opacity: "0" },
          "100%": { opacity: "1" },
        },
        slideUp: {
          "0%": { opacity: "0", transform: "translateY(20px)" },
          "100%": { opacity: "1", transform: "translateY(0)" },
        },
      },
    },
  },
  plugins: [],
};
export default config;
```

### layout.tsx — Font Setup with next/font

```tsx
import type { Metadata } from "next";
import { Playfair_Display, Source_Sans_3 } from "next/font/google";
import "./globals.css";

const displayFont = Playfair_Display({
  subsets: ["latin"],
  variable: "--font-display",
  display: "swap",
});

const bodyFont = Source_Sans_3({
  subsets: ["latin"],
  variable: "--font-body",
  display: "swap",
});

export const metadata: Metadata = {
  title: "My Website",
  description: "Built with Next.js and Tailwind CSS",
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en" className={`${displayFont.variable} ${bodyFont.variable}`}>
      <body className="font-body antialiased">{children}</body>
    </html>
  );
}
```

### globals.css — Clean Base

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  html {
    scroll-behavior: smooth;
  }
}
```

---

## Typography Pairing Cheatsheet

Pick **one display + one body** font. Never use Inter, Roboto, or Arial.

| Mood | Display Font | Body Font |
|------|-------------|-----------|
| **Elegant/Luxury** | Playfair Display | Source Sans 3 |
| **Modern/Clean** | Space Grotesk | DM Sans |
| **Bold/Startup** | Sora | Nunito Sans |
| **Creative/Artistic** | Cormorant Garamond | Lato |
| **Technical/Developer** | JetBrains Mono | IBM Plex Sans |
| **Friendly/Approachable** | Nunito | Open Sans |
| **Editorial/Magazine** | Fraunces | Inter Tight |
| **Retro/Vintage** | Bebas Neue | Poppins |

---

## Color Palette Patterns

### Generating from a Brand Color

```typescript
// In tailwind.config.ts — use a tool like https://uicolors.app
colors: {
  brand: {
    50:  "#eff6ff",   // Lightest (backgrounds)
    100: "#dbeafe",   // Light (hover states)
    200: "#bfdbfe",
    300: "#93c5fd",
    400: "#60a5fa",
    500: "#3b82f6",   // Primary (buttons, links)
    600: "#2563eb",   // Primary hover
    700: "#1d4ed8",   // Dark variant
    800: "#1e40af",
    900: "#1e3a8a",   // Darkest (text)
    950: "#172554",
  }
}
```

### Color by Mood

| Mood | Primary | Accent | Background |
|------|---------|--------|------------|
| **Tech/SaaS** | Indigo-600 | Cyan-400 | Slate-50 |
| **Organic/Natural** | Emerald-700 | Amber-500 | Stone-50 |
| **Luxury** | Neutral-900 | Gold (amber-400) | White |
| **Playful** | Violet-500 | Pink-400 | Slate-50 |
| **Corporate** | Blue-800 | Sky-500 | Gray-50 |
| **Bold/Creative** | Fuchsia-600 | Yellow-400 | Zinc-950 (dark) |

---

## Section Blueprints

### Hero Section

```tsx
export function Hero() {
  return (
    <section className="relative overflow-hidden bg-gradient-to-br from-brand-900 via-brand-800 to-brand-700 px-6 py-24 sm:py-32 lg:px-8">
      <div className="mx-auto max-w-2xl text-center">
        <h1 className="font-display text-4xl font-bold tracking-tight text-white sm:text-6xl animate-fade-in">
          Build something amazing
        </h1>
        <p className="mt-6 text-lg leading-8 text-brand-100 animate-slide-up">
          A compelling description that tells visitors exactly what you do
          and why they should care.
        </p>
        <div className="mt-10 flex items-center justify-center gap-x-6">
          <a
            href="#"
            className="rounded-full bg-white px-6 py-3 text-sm font-semibold text-brand-900 shadow-sm hover:bg-brand-50 transition-colors"
          >
            Get started
          </a>
          <a
            href="#"
            className="text-sm font-semibold leading-6 text-white hover:text-brand-200 transition-colors"
          >
            Learn more <span aria-hidden="true">&rarr;</span>
          </a>
        </div>
      </div>
    </section>
  );
}
```

### Features Grid

```tsx
import { Zap, Shield, Globe } from "lucide-react";

const features = [
  { icon: Zap,    title: "Lightning Fast",  description: "Optimized for speed." },
  { icon: Shield, title: "Secure",          description: "Built-in security."   },
  { icon: Globe,  title: "Global Scale",    description: "Deploy worldwide."    },
];

export function Features() {
  return (
    <section className="py-24 px-6 lg:px-8">
      <div className="mx-auto max-w-7xl">
        <div className="mx-auto max-w-2xl text-center">
          <h2 className="font-display text-3xl font-bold tracking-tight text-gray-900 sm:text-4xl">
            Everything you need
          </h2>
          <p className="mt-4 text-lg text-gray-600">
            All the tools to get you from idea to launch.
          </p>
        </div>
        <div className="mt-16 grid grid-cols-1 gap-8 sm:grid-cols-2 lg:grid-cols-3">
          {features.map((feature) => (
            <div
              key={feature.title}
              className="group relative rounded-2xl border border-gray-200 p-8 hover:border-brand-500 hover:shadow-lg transition-all"
            >
              <feature.icon className="h-8 w-8 text-brand-600" />
              <h3 className="mt-4 text-lg font-semibold text-gray-900">
                {feature.title}
              </h3>
              <p className="mt-2 text-gray-600">{feature.description}</p>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

### Navbar

```tsx
"use client";

import { useState } from "react";
import { Menu, X } from "lucide-react";

const links = [
  { label: "Features", href: "#features" },
  { label: "Pricing",  href: "#pricing"  },
  { label: "About",    href: "#about"    },
];

export function Navbar() {
  const [mobileOpen, setMobileOpen] = useState(false);

  return (
    <header className="sticky top-0 z-50 border-b border-gray-200/50 bg-white/80 backdrop-blur-lg">
      <nav className="mx-auto flex max-w-7xl items-center justify-between px-6 py-4 lg:px-8">
        <a href="/" className="font-display text-xl font-bold text-gray-900">
          Brand
        </a>

        {/* Desktop */}
        <div className="hidden md:flex items-center gap-8">
          {links.map((l) => (
            <a key={l.href} href={l.href} className="text-sm font-medium text-gray-600 hover:text-gray-900 transition-colors">
              {l.label}
            </a>
          ))}
          <a href="#" className="rounded-full bg-brand-600 px-4 py-2 text-sm font-semibold text-white hover:bg-brand-700 transition-colors">
            Get Started
          </a>
        </div>

        {/* Mobile toggle */}
        <button className="md:hidden" onClick={() => setMobileOpen(!mobileOpen)}>
          {mobileOpen ? <X className="h-6 w-6" /> : <Menu className="h-6 w-6" />}
        </button>
      </nav>

      {/* Mobile menu */}
      {mobileOpen && (
        <div className="md:hidden border-t border-gray-200 bg-white px-6 py-4 space-y-3">
          {links.map((l) => (
            <a key={l.href} href={l.href} className="block text-sm font-medium text-gray-700">
              {l.label}
            </a>
          ))}
        </div>
      )}
    </header>
  );
}
```

### Footer

```tsx
export function Footer() {
  return (
    <footer className="border-t border-gray-200 bg-gray-50 px-6 py-12 lg:px-8">
      <div className="mx-auto max-w-7xl flex flex-col items-center gap-4 sm:flex-row sm:justify-between">
        <span className="font-display text-lg font-bold text-gray-900">Brand</span>
        <p className="text-sm text-gray-500">
          &copy; {new Date().getFullYear()} Brand. All rights reserved.
        </p>
      </div>
    </footer>
  );
}
```

### Pricing Section

```tsx
const plans = [
  { name: "Starter", price: "$9",  period: "/mo", features: ["5 projects", "Basic analytics", "Email support"],        highlighted: false },
  { name: "Pro",     price: "$29", period: "/mo", features: ["Unlimited projects", "Advanced analytics", "Priority support", "Custom domain"], highlighted: true },
  { name: "Team",    price: "$79", period: "/mo", features: ["Everything in Pro", "Team collaboration", "SSO", "Dedicated account manager"], highlighted: false },
];

export function Pricing() {
  return (
    <section id="pricing" className="py-24 px-6 lg:px-8 bg-gray-50">
      <div className="mx-auto max-w-7xl">
        <h2 className="font-display text-3xl font-bold tracking-tight text-center text-gray-900 sm:text-4xl">
          Simple, transparent pricing
        </h2>
        <div className="mt-16 grid grid-cols-1 gap-8 sm:grid-cols-2 lg:grid-cols-3">
          {plans.map((plan) => (
            <div
              key={plan.name}
              className={`rounded-2xl p-8 ${
                plan.highlighted
                  ? "bg-brand-600 text-white ring-2 ring-brand-600 shadow-xl scale-105"
                  : "bg-white text-gray-900 ring-1 ring-gray-200"
              }`}
            >
              <h3 className="text-lg font-semibold">{plan.name}</h3>
              <p className="mt-4 flex items-baseline gap-1">
                <span className="text-4xl font-bold">{plan.price}</span>
                <span className={plan.highlighted ? "text-brand-200" : "text-gray-500"}>{plan.period}</span>
              </p>
              <ul className="mt-8 space-y-3">
                {plan.features.map((f) => (
                  <li key={f} className="flex items-center gap-2 text-sm">
                    <span className={plan.highlighted ? "text-brand-200" : "text-brand-600"}>&#10003;</span>
                    {f}
                  </li>
                ))}
              </ul>
              <a
                href="#"
                className={`mt-8 block rounded-full px-4 py-2.5 text-center text-sm font-semibold transition-colors ${
                  plan.highlighted
                    ? "bg-white text-brand-600 hover:bg-brand-50"
                    : "bg-brand-600 text-white hover:bg-brand-700"
                }`}
              >
                Get started
              </a>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

### Testimonials

```tsx
const testimonials = [
  { quote: "This changed everything for our team.", author: "Sarah Chen", role: "CEO, TechCo" },
  { quote: "Best decision we made this year.",      author: "Marcus Liu", role: "CTO, StartupX" },
  { quote: "Incredible experience from day one.",   author: "Elena Ruiz", role: "PM, DesignLab" },
];

export function Testimonials() {
  return (
    <section className="py-24 px-6 lg:px-8">
      <div className="mx-auto max-w-7xl">
        <h2 className="font-display text-3xl font-bold tracking-tight text-center text-gray-900 sm:text-4xl">
          Loved by teams everywhere
        </h2>
        <div className="mt-16 grid grid-cols-1 gap-8 sm:grid-cols-2 lg:grid-cols-3">
          {testimonials.map((t) => (
            <blockquote key={t.author} className="rounded-2xl bg-gray-50 p-8">
              <p className="text-gray-700 italic">&ldquo;{t.quote}&rdquo;</p>
              <footer className="mt-4">
                <p className="font-semibold text-gray-900">{t.author}</p>
                <p className="text-sm text-gray-500">{t.role}</p>
              </footer>
            </blockquote>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## Dev Server Management

```bash
# Start dev server (default port 3000)
cd <project> && npm run dev

# If port 3000 is busy, Next.js auto-picks next available port — check output

# Stop dev server
# Use Ctrl+C or kill the process

# Build for production check
npm run build
```

**Always tell the user:** "Your site is running at http://localhost:3000 — open it in your browser to see the result!"

---

## Responsive Design Cheatsheet

| Breakpoint | Prefix | Min Width | Typical Device |
|------------|--------|-----------|----------------|
| Default | (none) | 0px | Mobile |
| sm | `sm:` | 640px | Large phone |
| md | `md:` | 768px | Tablet |
| lg | `lg:` | 1024px | Laptop |
| xl | `xl:` | 1280px | Desktop |
| 2xl | `2xl:` | 1536px | Large desktop |

**Pattern**: Always design mobile-first, then add larger breakpoint overrides.

```tsx
{/* 1 column on mobile, 2 on tablet, 3 on desktop */}
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-8">
```

---

## Common Tailwind Patterns

### Glassmorphism
```tsx
<div className="bg-white/10 backdrop-blur-lg border border-white/20 rounded-2xl shadow-xl" />
```

### Gradient Text
```tsx
<h1 className="bg-gradient-to-r from-brand-500 to-accent bg-clip-text text-transparent" />
```

### Card Hover Effect
```tsx
<div className="group rounded-2xl border p-6 hover:shadow-lg hover:border-brand-500 hover:-translate-y-1 transition-all duration-300" />
```

### Sticky Header with Blur
```tsx
<header className="sticky top-0 z-50 bg-white/80 backdrop-blur-lg border-b border-gray-200/50" />
```

### Aspect Ratio Container
```tsx
<div className="aspect-video rounded-2xl bg-gray-100 overflow-hidden" />
```
