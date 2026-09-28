<script setup>
import { ref, onBeforeUnmount } from "vue";

const progresso = ref(0);
const displayPct = ref(0);
const bubbles = ref([]);
const concluido = ref(false);
let bubbleId = 0;
let rafId = null;
let simRaf = null;
let lastProgresso = 0;

function setProgresso(v) {
  const target = Math.max(0, Math.min(100, v));
  const prev = progresso.value;
  progresso.value = target;
  concluido.value = target >= 100;
  if (Math.abs(target - prev) > 0.5) emitBubbles(target, prev);
  animateCount(prev, target);
  if (target >= 100 && prev < 100) celebrate();
}

function animateCount(from, to) {
  if (rafId) cancelAnimationFrame(rafId);
  const start = performance.now();
  const dur = 400;
  const step = (now) => {
    const t = Math.min(1, (now - start) / dur);
    displayPct.value = Math.round(from + (to - from) * t);
    if (t < 1) rafId = requestAnimationFrame(step);
  };
  rafId = requestAnimationFrame(step);
}

function emitBubbles(target, prev) {
  const count = Math.min(5, Math.ceil(Math.abs(target - prev) / 5));
  for (let i = 0; i < count; i++) {
    const id = bubbleId++;
    bubbles.value.push({
      id,
      left: target,
      offset: (Math.random() - 0.5) * 30,
      size: 6 + Math.random() * 10,
      delay: Math.random() * 0.2,
      color: ["#7c3aed", "#ec4899", "#fbbf24"][Math.floor(Math.random() * 3)],
    });
  }
}

function removeBubble(id) {
  bubbles.value = bubbles.value.filter((b) => b.id !== id);
}

function celebrate() {
  for (let i = 0; i < 20; i++) {
    const id = bubbleId++;
    bubbles.value.push({
      id,
      left: 50 + (Math.random() - 0.5) * 40,
      offset: (Math.random() - 0.5) * 200,
      size: 8 + Math.random() * 14,
      delay: Math.random() * 0.3,
      color: ["#7c3aed", "#ec4899", "#fbbf24", "#34d399"][Math.floor(Math.random() * 4)],
    });
  }
}

function add10() { setProgresso(progresso.value + 10); }
function sub10() { setProgresso(progresso.value - 10); }
function reset() {
  if (simRaf) cancelAnimationFrame(simRaf);
  setProgresso(0);
}

function simulateDownload() {
  if (simRaf) cancelAnimationFrame(simRaf);
  let v = progresso.value;
  const step = () => {
    if (v >= 100) return;
    if (Math.random() > 0.2) v += Math.random() * 4;
    setProgresso(v);
    if (v < 100) {
      const delay = 100 + Math.random() * 400;
      simRaf = setTimeout(step, delay);
    }
  };
  step();
}

onBeforeUnmount(() => {
  if (rafId) cancelAnimationFrame(rafId);
  if (simRaf) clearTimeout(simRaf);
});
</script>

<template>
  <main class="app">
    <h1>Barra de Progresso com Microinterações</h1>
    <div class="bar-wrap" :class="{ 'bar-wrap--done': concluido }">
      <div class="bar-track">
        <div class="bar-fill" :style="{ width: progresso + '%' }">
          <span class="bar-head"></span>
        </div>
        <span
          v-for="b in bubbles"
          :key="b.id"
          class="bubble"
          :style="{
            left: b.left + '%',
            width: b.size + 'px',
            height: b.size + 'px',
            animationDelay: b.delay + 's',
            '--bx': b.offset + 'px',
            background: b.color,
          }"
          @animationend="removeBubble(b.id)"
        ></span>
      </div>
      <span class="pct">{{ displayPct }}%</span>
    </div>
    <div v-if="concluido" class="seal">✓ Concluído</div>
    <div class="controls">
      <button @click="sub10">−10</button>
      <button @click="add10">+10</button>
      <button @click="simulateDownload">Simular download</button>
      <button @click="reset">Reset</button>
    </div>
  </main>
</template>
