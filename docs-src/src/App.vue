<script setup>
import { computed, ref } from 'vue';

const starters = [
  {
    id: 'vue',
    label: 'Vue app',
    badge: 'Best fit for this repo',
    eyebrow: 'Vue starter',
    title: 'Keep the Vue setup small and the styles reusable.',
    summary: 'Use this when you want a single page demo with a stable token base and a small amount of component logic.',
    bullets: [
      'Import the library stylesheet in the entry file once.',
      'Render the docs demo as one Vue surface.',
      'Swap the starter content without leaving the page.'
    ],
    snippet: `import { createApp } from 'vue';
import '../../pristine-styles.css';
import './styles.css';
import App from './App.vue';

createApp(App).mount('#app');`
  },
  {
    id: 'react',
    label: 'React app',
    badge: 'Component shell',
    eyebrow: 'React starter',
    title: 'Import the stylesheet once and let React stay focused.',
    summary: 'Use the same tokens when you are wiring a React shell, dashboard, or design system preview.',
    bullets: [
      'Load the stylesheet in the React entry file.',
      'Keep component CSS narrow and local.',
      'Use the same tokens for surfaces, spacing, and theme states.'
    ],
    snippet: `import React from 'react';
import ReactDOM from 'react-dom/client';
import '../../pristine-styles.css';
import './styles.css';
import { App } from './App';

ReactDOM.createRoot(document.getElementById('root')).render(<App />);`
  },
  {
    id: 'html',
    label: 'Plain HTML',
    badge: 'Fastest route',
    eyebrow: 'Plain HTML starter',
    title: 'Link the stylesheet and keep the page in your control.',
    summary: 'Use this when you want the shortest possible path for static pages, docs, or prototypes.',
    bullets: [
      'Link the compiled stylesheet directly.',
      'Build your own structure with plain markup.',
      'Flip to dark mode with the semantic theme tokens.'
    ],
    snippet: `<link rel="stylesheet" href="./pristine-styles.css">
<main class="panel">
  <h1>Pristine Styles</h1>
  <p>Use the tokens for layout, color, and theme values.</p>
</main>`
  }
];

const activeStarter = ref('vue');

const activeStarterData = computed(() => {
  return starters.find((starter) => starter.id === activeStarter.value) ?? starters[0];
});

const tokens = [
  {
    title: 'Link color',
    name: '--color-link',
    swatch: 'var(--color-link)',
    description: 'Anchors, buttons, and accent states.'
  },
  {
    title: 'Secondary surface',
    name: '--surface-color-secondary',
    swatch: 'var(--surface-color-secondary)',
    description: 'Cards, code blocks, and inset panels.'
  },
  {
    title: 'Border color',
    name: '--color-border',
    swatch: 'var(--color-border)',
    description: 'Layout separators and quiet outlines.'
  },
  {
    title: 'Focus ring',
    name: '--color-focus-ring',
    swatch: 'var(--color-focus-ring)',
    description: 'Keyboard states and accessible emphasis.'
  }
];

const deploySteps = [
  {
    title: 'Build the library CSS',
    description: 'Run pnpm build to refresh pristine-styles.css from scss/index.scss.'
  },
  {
    title: 'Build the Vue docs app',
    description: 'Run pnpm build:docs to compile the Vue page into docs/.'
  },
  {
    title: 'Publish docs/',
    description: 'Point GitHub Pages at the docs folder or ship that folder in your release flow.'
  }
];
</script>

<template>
  <div class="docs-shell">
    <header class="docs-topbar">
      <a class="site-brand" href="#top" aria-label="Pristine Styles home">
        <span class="site-brand__mark">PS</span>
        <span class="site-brand__copy">
          <strong>Pristine Styles</strong>
          <span>Vue demo</span>
        </span>
      </a>
      <nav class="site-nav" aria-label="Page sections">
        <a href="#starter-lab">Starter lab</a>
        <a href="#tokens">Tokens</a>
        <a href="#deploy">Deploy</a>
      </nav>
    </header>

    <main id="top">
      <section class="page-hero">
        <div class="hero-copy">
          <p class="eyebrow">GitHub Pages demo</p>
          <h1 class="page-title">One Vue page for the demo and the starter snippets.</h1>
          <p class="page-lede">The separate React and Vue starter pages are folded into one app, so the docs stay easier to maintain. Pick a stack, inspect the snippet, and keep moving.</p>
          <div class="hero-actions">
            <a class="button button--primary" href="#starter-lab">Open the demo</a>
            <a class="button button--secondary" href="#deploy">Check deploy steps</a>
          </div>
          <p class="page-note">The published site is built from this Vue app and written into the docs folder so GitHub Pages can serve it directly.</p>
        </div>
      </section>

      <section id="starter-lab" class="section">
        <div class="section-header">
          <p class="eyebrow">Starter lab</p>
          <h2>Switch between plain HTML, React, and Vue without leaving the page.</h2>
          <p>Each state shows the same design system from a different entry point shape.</p>
        </div>

        <div class="panel starter-shell">
          <div class="starter-tabs" aria-label="Starter modes">
            <button
              v-for="starter in starters"
              :key="starter.id"
              type="button"
              class="starter-tab"
              :class="{ 'is-active': starter.id === activeStarter }"
              :aria-pressed="starter.id === activeStarter"
              @click="activeStarter = starter.id"
            >
              <span>{{ starter.label }}</span>
              <small>{{ starter.badge }}</small>
            </button>
          </div>

          <div class="starter-grid">
            <article :key="activeStarterData.id" class="panel starter-preview">
              <p class="eyebrow">{{ activeStarterData.eyebrow }}</p>
              <h2>{{ activeStarterData.title }}</h2>
              <p>{{ activeStarterData.summary }}</p>
              <ul class="starter-list">
                <li v-for="item in activeStarterData.bullets" :key="item">
                  {{ item }}
                </li>
              </ul>
              <div class="hero-actions">
                <a class="button button--primary" href="#deploy">Use these steps</a>
                <a class="button button--secondary" href="#tokens">See the tokens</a>
              </div>
            </article>

            <article :key="`${activeStarterData.id}-code`" class="code-panel starter-code">
              <p class="eyebrow">Entry file</p>
              <pre><code v-text="activeStarterData.snippet"></code></pre>
            </article>
          </div>
        </div>
      </section>

      <section id="tokens" class="section">
        <div class="section-header">
          <p class="eyebrow">Token snapshot</p>
          <h2>The values you will reach for most often.</h2>
          <p>These stay stable no matter which starter mode you choose.</p>
        </div>

        <div class="token-grid">
          <article v-for="token in tokens" :key="token.name" class="token-item">
            <span class="token-swatch" :style="{ '--swatch': token.swatch }"></span>
            <strong>{{ token.title }}</strong>
            <code>{{ token.name }}</code>
            <p>{{ token.description }}</p>
          </article>
        </div>
      </section>

      <section id="deploy" class="section split-section">
        <article class="panel">
          <p class="eyebrow">Deploy</p>
          <h2>Build the stylesheet, build the Vue app, publish docs/.</h2>
          <ol class="step-list">
            <li v-for="step in deploySteps" :key="step.title">
              <strong>{{ step.title }}</strong> {{ step.description }}
            </li>
          </ol>
        </article>

        <article class="panel theme-panel" data-theme="dark">
          <p class="eyebrow">Manual dark mode</p>
          <h2>Use <code>data-theme="dark"</code> or <code>.dark</code> on the root.</h2>
          <p>The semantic aliases handle the contrast flip, so the page stays readable without extra branching.</p>
          <div class="theme-preview">
            <span>background</span>
            <strong>var(--background-color)</strong>
            <span>surface</span>
            <strong>var(--surface-color-primary)</strong>
            <span>text</span>
            <strong>var(--text-primary)</strong>
            <span>focus</span>
            <strong>var(--color-focus-ring)</strong>
          </div>
        </article>
      </section>
    </main>

    <footer class="docs-footer">
      <p>Run pnpm build:docs before publishing. It rebuilds the stylesheet, bundles the Vue app, and refreshes the GitHub Pages output.</p>
    </footer>
  </div>
</template>