<template>
  <div class="menu-panel">
    <div class="panel-header">
      <h2>Accessibility Controls</h2>
      <button class="close-btn" @click="$emit('close')">&times;</button>
    </div>

    <section>
      <h5>Visual Adjustments</h5>
      <div class="grid">
        <button :class="{'active': settings.contrast}" @click="settings.contrast = !settings.contrast; applyContrast()">
          <i class="icon contrast-icon"></i>
          Contrast+
        </button>
        <button :class="{'active': settings.highlightLinks}" @click="settings.highlightLinks = !settings.highlightLinks; applyHighlightLinks()">
          <i class="icon link-icon"></i>
          Highlight Links
        </button>
        <button :class="{'active': settings.biggerText}" @click="settings.biggerText = !settings.biggerText; applyFontSize()">
          <i class="icon text-icon"></i>
          Bigger Text
        </button>
        <button :class="{'active': settings.textSpacing}" @click="settings.textSpacing = !settings.textSpacing; applySpacing()">
          <i class="icon spacing-icon"></i>
          Text Spacing
        </button>
      </div>
    </section>

    <section>
      <h5>Behavior Controls</h5>
      <div class="grid">
        <button :class="{'active': settings.pauseAnimations}" @click="settings.pauseAnimations = !settings.pauseAnimations; applyPauseAnimations()">
          <i class="icon pause-icon"></i>
          Pause Animations
        </button>
        <button :class="{'active': settings.hideImages}" @click="settings.hideImages = !settings.hideImages; applyHideImages()">
          <i class="icon hide-icon"></i>
          Hide Images
        </button>
        <button :class="{'active': settings.dyslexiaFont}" @click="settings.dyslexiaFont = !settings.dyslexiaFont; applyDyslexiaFont()">
          <i class="icon dyslexia-icon"></i>
          Dyslexia Font
        </button>
      </div>
    </section>

    <section>
      <h5>Interface Tools</h5>
      <div class="grid">
        <button :class="{'active': settings.cursor}" @click="settings.cursor = !settings.cursor; applyCursor()">
          <i class="icon cursor-icon"></i>
          Enlarge Cursor
        </button>
        <button :class="{'active': settings.tooltips}" @click="settings.tooltips = !settings.tooltips; applyTooltips()">
          <i class="icon tooltip-icon"></i>
          Force Tooltips
        </button>
        <button :class="{'active': settings.pageStructure}" @click="settings.pageStructure = !settings.pageStructure; applyPageStructure()">
          <i class="icon structure-icon"></i>
          Page Structure
        </button>
      </div>
    </section>
  </div>
</template>


<script setup>
import { reactive, onMounted, watch } from 'vue';

const settings = reactive({
  contrast: false,
  highlightLinks: false,
  biggerText: false,
  textSpacing: false,
  pauseAnimations: false,
  hideImages: false,
  dyslexiaFont: false,
  cursor: false,
  tooltips: false,
  pageStructure: false
});

onMounted(() => {
  const saved = localStorage.getItem('a11y-settings');
  if (saved) Object.assign(settings, JSON.parse(saved));

  applyContrast();
  applyHighlightLinks();
  applyFontSize();
  applySpacing();
  applyPauseAnimations();
  applyHideImages();
  applyDyslexiaFont();
  applyCursor();
  applyTooltips();
  applyPageStructure();
});

watch(settings, (newVal) => {
  localStorage.setItem('a11y-settings', JSON.stringify(newVal));
}, { deep: true });

function applyContrast() {
  document.documentElement.style.filter = settings.contrast ? 'contrast(120%)' : '';
}
function applyHighlightLinks() {
  document.querySelectorAll('a').forEach(a => a.style.borderBottom = settings.highlightLinks ? '2px solid #f00' : '');
}
function applyFontSize() {
  document.documentElement.style.setProperty('--font-size', settings.biggerText ? '1.2rem' : '1rem');
}
function applySpacing() {
  document.body.style.letterSpacing = settings.textSpacing ? '0.1em' : '';
  document.body.style.lineHeight = settings.textSpacing ? '1.8' : '';
}
function applyPauseAnimations() {
  const existing = document.getElementById('pause-anims');
  if (existing) existing.remove();
  if (settings.pauseAnimations) {
    const style = document.createElement('style');
    style.id = 'pause-anims';
    style.innerHTML = `*, ::before, ::after {
      animation-delay: -1ms !important;
      animation-duration: 1ms !important;
    }`;
    document.head.appendChild(style);
  }
}
function applyHideImages() {
  document.querySelectorAll('img').forEach(img => img.style.visibility = settings.hideImages ? 'hidden' : 'visible');
}
function applyDyslexiaFont() {
  const existing = document.getElementById('dyslexia-font');
  if (existing) existing.remove();
  if (settings.dyslexiaFont) {
    const link = document.createElement('link');
    link.href = 'https://cdn.jsdelivr.net/npm/opendyslexic@latest/opendyslexic.css';
    link.rel = 'stylesheet';
    link.id = 'dyslexia-font';
    document.head.appendChild(link);
    document.body.style.fontFamily = 'OpenDyslexic';
  } else {
    document.body.style.fontFamily = '';
  }
}
function applyCursor() {
  const styleId = 'enlarge-cursor-style';
  document.getElementById(styleId)?.remove();
  if (settings.cursor) {
    const style = document.createElement('style');
    style.id = styleId;
    style.innerHTML = `* { cursor: url('https://cur.cursors-4u.net/symbols/sym-1/sym78.cur'), auto !important; }`;
    document.head.appendChild(style);
  }
}
function applyTooltips() {
  const styleId = 'force-tooltips-style';
  document.getElementById(styleId)?.remove();
  if (settings.tooltips) {
    const style = document.createElement('style');
    style.id = styleId;
    style.innerHTML = `
      [title], [aria-label] {
        position: relative;
      }
      [title]:hover::after, [aria-label]:hover::after {
        content: attr(title attr(aria-label));
        position: absolute;
        background: #000;
        color: #fff;
        padding: 4px 8px;
        font-size: 12px;
        border-radius: 4px;
        white-space: nowrap;
        z-index: 10000;
        top: 100%;
        left: 0;
      }
    `;
    document.head.appendChild(style);
    document.querySelectorAll('[title], [aria-label]').forEach(el => {
      const tooltip = el.getAttribute('title') || el.getAttribute('aria-label');
      if (tooltip) el.setAttribute('data-tooltip', tooltip);
    });
  }
}
function applyPageStructure() {
  const linkId = 'a11y-structure-style';
  document.getElementById(linkId)?.remove();
  if (settings.pageStructure) {
    const link = document.createElement('link');
    link.id = linkId;
    link.rel = 'stylesheet';
    link.href = 'https://ffoodd.github.io/a11y.css/css/a11y-en.css';
    document.head.appendChild(link);
  }
}
</script>

<style scoped>
.menu-panel {
  position: fixed;
  top: 80px;
  right: 20px;
  background: #EFF1F5;
  border: 1px solid #ccc;
  padding: 1em;
  width: 300px;
  max-height: 75vh;
  overflow-y: auto;
  box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  z-index: 999;
  color: black;
  border-radius: 10px;
  font-family: sans-serif;
  padding-top: 0;
}

.menu-panel h2 {
  margin-top: 0;
}

.menu-panel h3 {
  margin-top: 1em;
  margin-bottom: 0.5em;
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0.5rem;
  margin-bottom: 1rem;
}

.grid button {
  background: #ffffff;
  border: 1px solid #ffffff;
  padding: 0.75rem;
  text-align: center;
  font-size: 0.9rem;
  border-radius: 8px;
  cursor: pointer;
  transition: background 0.2s ease;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
}

.grid button.active {
  background-color: #007bff;
  color: white;
  border-color: #007bff;
}

.grid button:hover {
  background-color: #e0e0e0;
}

.icon {
  font-size: 1.2rem;
}

/* Placeholder icon classes — you can swap these with actual icons later */
.contrast-icon::before { content: ""; font-family: "Font Awesome 6 Free"; font-weight: 900; content: "\f0eb"; }   /* fa-adjust */
.link-icon::before { content: "\f0c1"; font-family: "Font Awesome 6 Free"; font-weight: 900; }  /* fa-link */
.text-icon::before { content: "\f031"; font-family: "Font Awesome 6 Free"; font-weight: 900; }  /* fa-font */
.spacing-icon::before { content: "\f547"; font-family: "Font Awesome 6 Free"; font-weight: 900; } /* fa-ruler-horizontal */
.pause-icon::before { content: "\f04c"; font-family: "Font Awesome 6 Free"; font-weight: 900; }  /* fa-pause */
.hide-icon::before { content: "\f070"; font-family: "Font Awesome 6 Free"; font-weight: 900; }  /* fa-eye-slash */
.dyslexia-icon::before { content: "\f5da"; font-family: "Font Awesome 6 Free"; font-weight: 900; } /* fa-book-open */
.cursor-icon::before { content: "\f245"; font-family: "Font Awesome 6 Free"; font-weight: 900; } /* fa-mouse-pointer */
.tooltip-icon::before { content: "\f059"; font-family: "Font Awesome 6 Free"; font-weight: 900; } /* fa-info-circle */
.structure-icon::before { content: "\f02d"; font-family: "Font Awesome 6 Free"; font-weight: 900; } /* fa-book */


.panel-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: sticky;
  top: 0;
  background: #5d56eb;
  color: white;
  z-index: 1000;
  padding: 0.5rem;
  border-bottom: 1px solid #ccc;
  margin: -16px -16px 8px -16px;
}

.panel-header h2 {
  margin: 0;
  font-size: 1.1rem;
}

.close-btn {
  font-size: 1.5rem;
  background: none;
  border: none;
  cursor: pointer;
  color: #ffffff;
  font-weight: bold;
  line-height: 1;
  padding: 0 0.5rem;
}


[class^="icon-"], 
[class$="-icon"],
.contrast-icon, .link-icon, .text-icon, .spacing-icon, .pause-icon,
.hide-icon, .dyslexia-icon, .cursor-icon, .tooltip-icon, .structure-icon {
  font-style: normal;
}
</style>

