---
theme: seriph
background: https://cover.sli.dev
title: Building Apps with Lit Element in 2026
info: |
  ## Building Applications with Web Components
  Presented by Quin Carter.
  Exploring modern application architecture with Lit.
class: text-center
drawings:
  persist: false
transition: slide-left
comark: true
duration: 45min
---

# Building Apps with Lit Element in 2026

Presented by Quin Carter

<div @click="$slidev.nav.next" class="mt-12 py-1" hover:bg="white op-10">
  Press Space for next page <carbon:arrow-right />
</div>

<div class="abs-br m-6 text-xl">
  <button @click="$slidev.nav.openInEditor()" title="Open in Editor" class="slidev-icon-btn">
    <carbon:edit />
  </button>
  <a href="https://github.com/slidevjs/slidev" target="_blank" class="slidev-icon-btn">
    <carbon:logo-github />
  </a>
</div>

<!--
The last comment block of each slide will be treated as slide notes. It will be visible and editable in Presenter Mode along with the slide. [Read more in the docs](https://sli.dev/guide/syntax.html#notes)
-->

---
transition: fade-out
---

# The Framework Landscape

Modern web development is dominated by powerful frameworks:

- **React**: Functional patterns, massive ecosystem.
- **Angular**: Opinionated, batteries-included for enterprise.
- **Vue**: Approachable, progressive, and highly flexible.
- **Svelte**: Compiler-first, minimal runtime overhead.

**What makes them good for building apps?**
Component models, declarative UIs, dependency management, and robust developer experience.

<style>
h1 {
  background-color: #2B90B6;
  background-image: linear-gradient(45deg, #4EC5D4 10%, #146b8c 20%);
  background-size: 100%;
  -webkit-background-clip: text;
  -moz-background-clip: text;
  -webkit-text-fill-color: transparent;
  -moz-text-fill-color: transparent;
}
</style>

<!--
Testing notes here
-->

---

# Question for the Audience

<div class="flex flex-col items-center justify-center h-full">
  <h2 class="text-3xl mb-8">What are you using today?</h2>
  <div class="grid grid-cols-2 gap-4">
    <div v-click class="border p-4 rounded text-center">React / Next.js</div>
    <div v-click class="border p-4 rounded text-center">Vue / Nuxt</div>
    <div v-click class="border p-4 rounded text-center">Angular</div>
    <div v-click class="border p-4 rounded text-center">Svelte / SvelteKit</div>
    <div v-click class="border p-4 rounded col-span-2 text-center bg-blue-500 bg-opacity-10 border-blue-500">
      Web Components / Lit
    </div>
  </div>
</div>

---

# Core Concepts for App Building

To build a real-world application in 2026, we need more than just components:

- 🏗️ **Application Structure**: Clear separation of concerns.
- 🛣️ **Routing**: Handling navigation and deep linking.
- 🔄 **Data Flow**: Prop drilling vs. Context.
- 🧠 **State Management**: Lightweight, reactive state machines.
- 🛠️ **Putting It All Together**: Reusable, scalable Views with a View Mixin
- 📦 **Packaging**: Micro-frontends and lightweight bundles.

---
layout: two-cols
---

# Application Structure

Our "App Shell" architecture:

- **src/app-shell.ts**: Main entry & layout
- **components/**: Atomic UI (Cards, Charts, Todos)
- **views/**: Page-level components & Mixins
- **shared/**:
    - **configuration/**: Routes & MFE config
    - **contexts/**: Auth, Nav, MFE loaders
    - **stores/**: Signal-based state
    - **utilities/**: Shared logic

::right::

```text
src
├── app-shell.ts
├── components
│   ├── card
│   ├── chart-js
│   ├── header
│   └── todos
├── shared
│   ├── configuration
│   ├── contexts
│   ├── stores
│   └── utilities
└── views
    ├── home-page
    └── todos-page
```

---

# Routing: Options in 2026

How we handle the "Single Page" experience in the Web Component ecosystem:

- **@lit-labs/router**:
    - Official Lit solution.
    - Native lifecycle integration.
    - Extremely lightweight and declarative.
- **Vaadin Router**:
    - Framework agnostic.
    - Robust nested routing and transition support.
    - Industry standard for complex Web Component apps.

---

# Implementation: @lit-labs/router

The lightweight, official solution for Lit.

````md magic-move
```ts {1-2}
// 1. Import the Router
import { Router } from '@lit-labs/router';

export class AppShell extends LitElement {
  // ...
}
```

```ts {5-10}
// 2. Initialize Routes in AppShell
import { Router } from '@lit-labs/router';

export class AppShell extends LitElement {
  private _router = new Router(this, [
    { path: '/', render: () => html`<home-page></home-page>` },
    { path: '/todos', render: () => html`<todos-page></todos-page>` },
    { path: '/todos/:id', render: (p) => html`<detail-page .id=${p.id}></detail-page>` }
  ]);
  
  // ...
}
```

```ts {7-15}
// 3. Define the Router Outlet
import { Router } from '@lit-labs/router';

export class AppShell extends LitElement {
  private _router = new Router(this, [ /* ... routes ... */ ]);

  render() {
    return html`
      <nav>
        <a href="/">Home</a>
        <a href="/todos">Todos</a>
      </nav>
      <main>${this._router.outlet()}</main>
    `;
  }
}
```
````

---

# Implementation: Vaadin Router

A powerful, framework-agnostic alternative.

````md magic-move
```ts {1-2}
// 1. Import the Router
import { Router } from '@vaadin/router';

export class AppShell extends LitElement {
  // ...
}
```

```ts {1,5-15}
// 2. Initialize in firstUpdated
import { Router } from '@vaadin/router';

export class AppShell extends LitElement {
  protected firstUpdated() {
    const outlet = this.shadowRoot?.querySelector('#outlet');
    const router = new Router(outlet);
    
    router.setRoutes([
      { path: '/', component: 'home-page' },
      { path: '/todos', component: 'todos-page' },
      { path: '(.*)', component: 'not-found-page' }
    ]);
  }
  
  // ...
}
```

```ts {9-11}
// 3. Render the Outlet Element
import { Router } from '@vaadin/router';

export class AppShell extends LitElement {
  protected firstUpdated() {
    // ... initialization logic ...
  }

  render() {
    return html`<main id="outlet"></main>`;
  }
}
```
````

---

# Data Flow: Context & Tasks

Managing complexity without the overhead of massive state libraries:

- **@lit/context**:
    - Dependency Injection for the web.
    - Used for `navigation.context.ts` and `accesses.context.ts`.
    - Decouples component logic from data providers.
- **@lit/tasks**:
    - Declarative asynchronous state management.
    - Native handling of *Pending*, *Error*, and *Complete* states for API calls.
  
---

# Deep Dive: @lit/context

Dependency injection for the component tree.

````md magic-move
```ts
// 1. Define the Context Token (navigation.context.ts)
import { createContext } from '@lit/context';
import { type NavItem } from '../interfaces/navigation.interface';

export const NavigationContext = createContext<NavItem[]>('navigation');
```

```ts
// 2. Provide the Context (app-shell.ts)
import { provide } from '@lit/context';
import { NavigationContext } from './contexts/navigation.context';

@customElement('app-shell')
export class AppShell extends LitElement {
  @provide({ context: NavigationContext })
  navItems: NavItem[] = initialNavItems;
}
```

```ts
// 3. Consume the Context (any child component)
import { consume } from '@lit/context';

export class MyView extends LitElement {
  @consume({ context: NavigationContext, subscribe: true })
  navItems: NavItem[] = [];

  render() {
    return html`<ul>${this.navItems.map(i => html`<li>${i.label}</li>`)}</ul>`;
  }
}
```
````

---

# Deep Dive: @lit/tasks

Managing asynchronous state declaratively.

````md magic-move
```ts
// 1. Defining a Task
import { Task } from '@lit/task';

class TodoView extends LitElement {
  private _apiTask = new Task(this, {
    task: async ([todoId]) => {
      const response = await fetch(`https://api.example.com/todos/${todoId}`);
      return response.json();
    },
    args: () => [this.selectedTodoId]
  });
}
```

```ts {3,4,5,6-11}
// 2. Rendering the Task States
render() {
  return this._apiTask.render({
    pending: () => html`<p>Loading...</p>`,
    error: (e) => html`<p>Error: ${e}</p>`,
    complete: (todo) => html`
      <div>
        <h3>${todo.title}</h3>
        <p>${todo.description}</p>
      </div>
    `
  });
}
```
````

<!--
- **@lit/task** is a reactive controller that manages asynchronous work within the component lifecycle.
- In **Step 1**, we instantiate the task. The `task` property is the actual async logic, and `args` is a function that returns an array of reactive dependencies.
- **Key point:** Whenever any property used in `args` (like `this.selectedTodoId`) changes, the task automatically resets and re-runs. No manual `watch` or `updated` logic required.
- In **Step 2**, we use the `render()` method of the task. This is a declarative API that forces you to handle all three UI states: Pending, Error, and Complete.
- This pattern eliminates the "boolean soup" (`isLoading`, `isError`, etc.) typically found in React or Vue components, keeping the main render method clean and type-safe.
-->

---

# The View Mixin: Standardizing Pages

Extending functionality across all views without duplication.

````md magic-move
```ts {*|4-10}
// 1. Context-based Data Flow
export const ViewMixin = <T extends Constructor<LitElement>>(superClass: T) => {
  class ViewMixinClass extends superClass {
    @consume({ context: NavigationContext, subscribe: true })
    navItems: NavItem[] = [];

    @consume({ context: AccessesContext, subscribe: true })
    accesses: string[] = [];

    @consume({ context: MfeLoaderContext, subscribe: true })
    mfeLoader: MfeLoader | undefined;
    
    // ...
  }
  return ViewMixinClass;
};
```

```ts
// 2. Lifecycle: Initialization
// ... in connectedCallback()
connectedCallback() {
  super.connectedCallback();
  this.featureIsEnabled = true;

  const filterKey = this.isMfe ? 'mfeComponent' : 'tagName';
  this.componentData = this.navItems.find(
    (item) => item[filterKey]?.tagName === this.tagName
  ) || {} as NavItem;

  this.mfeLoader?.init();
}
```

```ts
// 3. Lifecycle: Dynamic MFE Loading
protected firstUpdated(_changedProperties: PropertyValues) {
  const selectedMfe = this.mfeLoader?.config.find(
    (item) => item.tagName === this.tagName
  );

  if (selectedMfe) {
    const el = document.createElement(selectedMfe.tagName);
    this.shadowRoot?.querySelector('#mfe-container')
      ?.appendChild(el);
  }
}
```

```ts
// 4. Standardized Rendering Logic
renderMfe(customTemplate?: HTMLTemplateResult) {
  if (customTemplate) return html`${customTemplate}`;

  return html`${
    this.featureIsEnabled && this.componentData?.userHasPermission && this.isMfe
      ? html`<div id="mfe-container"></div>`
      : this.renderUnderConstruction()
  }`;
}
```

```ts {3,16|5-15}
// 5. Implementation in todos-page.ts
@customElement("todos-page")
export class TodosPage extends ViewMixin(LitElement) {
  isMfe = false;

  render() {
    return this.renderMfe(
      html`
        <div class="page-container">
          <h2>Your Tasks</h2>
          <todo-list></todo-list>
        </div>
      `,
    );
  }
}
```
````

---

# State Management: Lit + Signals

Why we moved away from heavy-weight state containers in 2026:

- **Fine-grained reactivity**: Only the components using the signal re-render.
- **Lightweight Stores**: `todo-list.store.ts` manages state with minimal boilerplate.
- **Native Integration**: Seamlessly integrated into Lit components via `@lit-labs/preact-signals`.

```ts
// Example: todo.store.ts
import { SignalWatcher, signal } from '@lit-labs/preact-signals';

export const todoSignal = signal<Todo[]>([]);
export const addTodo = (text: string) => {
  todoSignal.value = [...todoSignal.value, { text, done: false }];
};
```

---

# Lightweight & Versatile

The 2026 advantage of building with Lit:

- **Zero Framework Lock-in**: Web Components are the platform.
- **MFE Ready**: Seamlessly load Vite-based micro-frontends via `mfe-loader.utility.ts`.
- **Modern Tooling**: Powered by **Vite** and **Biome** for lightning-fast development.
- **Performance**: Lit's tiny footprint (~5KB) ensures the fastest Time to Interactive.
- **Versatility**: Use the same components in React, Vue, or vanilla HTML.

---
layout: two-cols
---

# App Shell Github Template

There is a Github Template that can help you get started and apply all these concepts very quickly!

- Navigation ready
- MFE Host App Ready
- Fill in the gaps
- Add on as needed
- ❤️ Built with Love by hand by @quincarter

::right::
<img src="./qr-code.png" alt="qr code" height="70%" width="70%" style="border-radius:20px; margin-left:2rem;"/>

---
layout: center
class: text-center
---

# Questions?

**Thank you!**

