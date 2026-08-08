# Building Apps with Lit Element in 2026

Presented by **Quin Carter**

This slide deck was presented at:
- **RVA.js** (August 2026)
- **Async Live 2026** (Capital One Internal Conference)

---

## 📋 Presentation Overview

This presentation explores modern application architecture using **Lit Element** and Web Components in 2026, demonstrating how to build production-grade web applications and micro-frontends with zero library lock-in.

### Key Topics Covered

- 🌐 **The Framework Landscape in 2026**: comparing React, Angular, Vue, Svelte, and Web Components / Lit.
- 🏗️ **App Shell Architecture**: structuring project directories (`src/app-shell.ts`, `components/`, `views/`, `shared/`).
- 🛣️ **Client-Side Routing**:
  - `@lit-labs/router`: the official, lightweight, declarative Lit routing solution.
  - **Vaadin Router**: standalone router supporting nested routes and transitions.
- 🔄 **Data Flow & Dependency Injection**: using `@lit/context` to decouple components and manage context across the component tree.
- 🧠 **Declarative Async State**: using `@lit/tasks` to handle Pending, Error, and Complete UI states for async requests.
- 🛠️ **Page Standardization with `ViewMixin`**: reusable view mixins handling navigation context and access control alongside runtime micro-frontend (MFE) loading.
- ⚡ **State Management with Lit + Signals**: fine-grained reactivity using `@lit-labs/preact-signals` for lightweight state stores.
- 📦 **Micro-Frontends & Tooling**: modern Vite & Biome tooling, fast Time to Interactive, ~5KB footprint, and component reusability across any stack.

---

## 🚀 Presentation Setup (Slidev)

This presentation is built with [Slidev](https://github.com/slidevjs/slidev).

To start the slide show locally:

1. Install dependencies:
   ```bash
   pnpm install
   ```

2. Run the dev server:
   ```bash
   pnpm dev
   ```

3. Visit <http://localhost:3030> in your browser.

Edit [`slides.md`](./slides.md) to make changes to the presentation.

Learn more about Slidev at the [Slidev Documentation](https://sli.dev/).
