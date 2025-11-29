<template>
  <div class="app-root">
    <div class="card">
      <h1 class="title">Brahma-Firelight + Vue + TypeScript Starter</h1>

      <div class="controls">
        <button class="btn-primary" :disabled="loading" @click="callEcho">
          {{ loading ? "Sending..." : "POST → /echo" }}
        </button>
      </div>

      <pre class="output">{{
        response || "Click the button to send POST /echo"
      }}</pre>

      <h2 class="iframe-title">Docs</h2>

      <div class="iframe-wrapper">
        <iframe
          title="rsjs docs"
          src="https://shyam20001.github.io/rsjs"
          loading="lazy"
          sandbox="allow-scripts allow-same-origin"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";

const response = ref<string>("");
const loading = ref(false);

async function callEcho() {
  loading.value = true;
  response.value = "";
  try {
    const res = await fetch("/api/echo", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({ msg: "Hello from Vue!" }),
    });

    // prefer text (mirrors your React code). You can also check content-type and parse JSON.
    const text = await res.text();
    response.value = text;
  } catch (err: unknown) {
    response.value =
      "Error: " + (err instanceof Error ? err.message : String(err));
  } finally {
    loading.value = false;
  }
}
</script>

<style scoped>
:root {
  --bg: #0e0e0e;
  --card: #1a1a1a;
  --accent: #4ade80;
  --muted: #9aa4b2;
}

.app-root {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: var(--bg);
  padding: 2rem;
  color: #fff;
  box-sizing: border-box;
  font-family: Inter, system-ui, -apple-system, "Segoe UI", Roboto,
    "Helvetica Neue", Arial;
}

.card {
  width: 100%;
  max-width: 900px;
  background: var(--card);
  padding: 2rem;
  border-radius: 14px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.6);
  border: 1px solid #333;
  display: flex;
  flex-direction: column;
  gap: 1rem;
  box-sizing: border-box;
}

.title {
  text-align: center;
  margin: 0;
  color: var(--accent);
  font-size: 1.3rem;
  font-weight: 700;
}

.controls {
  display: flex;
  justify-content: center;
  margin-top: 0.25rem;
}

.btn-primary {
  padding: 12px 28px;
  font-size: 16px;
  cursor: pointer;
  border-radius: 8px;
  border: none;
  background: #79af13ff;
  color: #000000ff;
  font-weight: 600;
  transition: transform 0.12s ease, box-shadow 0.12s ease;
}
.btn-primary:active {
  transform: translateY(1px) scale(0.997);
}
.btn-primary:disabled {
  opacity: 0.6;
  cursor: not-allowed;
  transform: none;
  box-shadow: none;
}

.output {
  background: #111;
  color: #0f0;
  padding: 1rem;
  border-radius: 8px;
  min-height: 80px;
  font-size: 14px;
  overflow-x: auto;
  text-align: left;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, "Roboto Mono",
    monospace;
  white-space: pre-wrap;
  word-break: break-word;
  border: 1px solid rgba(255, 255, 255, 0.03);
}

.iframe-title {
  text-align: center;
  margin-top: 1rem;
  margin-bottom: 0;
  color: var(--muted);
  font-size: 1rem;
}

.iframe-wrapper {
  width: 100%;
  height: 400px;
  border-radius: 10px;
  margin-top: 0.75rem;
  border: 1px solid #333;
  overflow: hidden;
  background: #0b0b0b;
  position: relative;
}
.iframe-wrapper iframe {
  width: 100%;
  height: 100%;
  border: 0;
  display: block;
}

/* responsive adjustments */
@media (max-width: 640px) {
  .card {
    padding: 1.25rem;
    border-radius: 12px;
  }
  .title {
    font-size: 1.1rem;
  }
  .btn-primary {
    padding: 10px 18px;
    font-size: 14px;
  }
  .iframe-wrapper {
    height: 300px;
  }
}
</style>
