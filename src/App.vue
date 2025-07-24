<script setup lang="ts">
import { ref, computed, onMounted } from 'vue'

const input = ref('')
const isDark = ref(false)
const copied = ref(false)

const output = computed(() => {
  if (!input.value.trim()) return ''
  
  return input.value.replace(/\./g, '[.]')
})

const copyToClipboard = async () => {
  if (!output.value) return
  
  try {
    await navigator.clipboard.writeText(output.value)
    copied.value = true
    setTimeout(() => copied.value = false, 2000)
  } catch (err) {
    console.error('Failed to copy:', err)
  }
}

const toggleTheme = () => {
  isDark.value = !isDark.value
  document.documentElement.setAttribute('data-theme', isDark.value ? 'dark' : 'light')
}

onMounted(() => {
  const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches
  isDark.value = prefersDark
  document.documentElement.setAttribute('data-theme', isDark.value ? 'dark' : 'light')
})
</script>

<template>
  <div class="app" itemscope itemtype="https://schema.org/WebApplication">
    <header class="header">
      <h1 itemprop="name">Email & URL Obfuscator</h1>
      <meta itemprop="description" content="Tool to obfuscate emails and URLs by replacing dots with brackets for security analysis" />
      <meta itemprop="applicationCategory" content="SecurityApplication" />
      <button @click="toggleTheme" class="theme-toggle" :aria-label="isDark ? 'Switch to light mode' : 'Switch to dark mode'" :title="isDark ? 'Switch to light mode' : 'Switch to dark mode'">
        {{ isDark ? '☀️' : '🌙' }}
        <span class="sr-only">{{ isDark ? 'Currently in dark mode' : 'Currently in light mode' }}</span>
      </button>
    </header>
    
    <main class="main" role="main">
      <section class="input-section">
        <label for="input" class="label">Input</label>
        <textarea 
          id="input"
          v-model="input" 
          placeholder="Enter emails or URLs (e.g., user@example.com, www.example.com)"
          class="textarea"
        ></textarea>
      </section>
      
      <section class="output-section">
        <div class="output-header">
          <label for="output" class="label">Output</label>
          <button 
            @click="copyToClipboard" 
            :disabled="!output"
            class="copy-btn"
            :class="{ copied }"
          >
            {{ copied ? 'Copied!' : 'Copy' }}
          </button>
        </div>
        <textarea 
          id="output"
          :value="output" 
          readonly 
          placeholder="Obfuscated output will appear here"
          class="textarea output"
        ></textarea>
      </section>
    </main>
    
    <footer class="footer" role="contentinfo">
      <p>Other cybersecurity tools:</p>
      <div class="links">
        <a href="https://kali-in.834008.xyz/" target="_blank" rel="noopener">Kali Tools</a>
        <a href="https://aws-cloudtrail-search.834008.xyz/" target="_blank" rel="noopener">CloudTrail Search</a>
        <a href="https://link-cleaner.834008.xyz/" target="_blank" rel="noopener">Link Cleaner</a>
        <a href="https://super-app.834008.xyz/" target="_blank" rel="noopener">Super App</a>
      </div>
    </footer>
  </div>
</template>

<style scoped>
.app {
  max-width: 800px;
  margin: 0 auto;
  padding: 1rem;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 2rem;
}

.header h1 {
  margin: 0;
  font-size: clamp(1.5rem, 4vw, 2.5rem);
}

.theme-toggle {
  font-size: 1.5rem;
  background: none;
  border: 2px solid var(--border-color);
  border-radius: 50%;
  width: 3rem;
  height: 3rem;
  cursor: pointer;
  transition: all 0.2s;
}

.theme-toggle:hover {
  transform: scale(1.1);
}

.main {
  display: grid;
  gap: 1.5rem;
}

.input-section,
.output-section {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.output-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.label {
  font-weight: 600;
  font-size: 1.1rem;
}

.textarea {
  min-height: 120px;
  padding: 1rem;
  border: 2px solid var(--border-color);
  border-radius: 8px;
  font-family: inherit;
  font-size: 1rem;
  background: var(--input-bg);
  color: var(--text-color);
  resize: vertical;
  transition: border-color 0.2s;
}

.textarea:focus {
  outline: none;
  border-color: var(--accent-color);
}

.textarea.output {
  background: var(--output-bg);
  cursor: default;
}

.copy-btn {
  padding: 0.5rem 1rem;
  background: var(--accent-color);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 500;
  transition: all 0.2s;
}

.copy-btn:hover:not(:disabled) {
  background: var(--accent-hover);
  transform: translateY(-1px);
}

.copy-btn:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.copy-btn.copied {
  background: var(--success-color);
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border: 0;
}

.footer {
  margin-top: 3rem;
  padding-top: 2rem;
  border-top: 1px solid var(--border-color);
  text-align: center;
}

.footer p {
  margin: 0 0 1rem 0;
  opacity: 0.8;
}

.links {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  justify-content: center;
}

.links a {
  color: var(--accent-color);
  text-decoration: none;
  padding: 0.5rem 1rem;
  border-radius: 6px;
  transition: all 0.2s;
}

.links a:hover {
  background: var(--accent-color);
  color: white;
  transform: translateY(-1px);
}

@media (min-width: 768px) {
  .main {
    grid-template-columns: 1fr 1fr;
  }
  
  .app {
    padding: 2rem;
  }
}
</style>
