# Barra de Progresso com Microinterações

> Progress bar que se anima de forma orgânica, com bolhas ou partículas acompanhando o avanço.

## Stack

- Vite + Vue 3 (`<script setup>`, JavaScript) — sem TypeScript, sem lint/test, zero libs extras
- CSS puro em `src/styles.css`
- Arquivos: `index.html`, `package.json` (deps: `vue`), `vite.config.js`, `.stackblitzrc`, `src/main.js`, `src/App.vue`, `src/styles.css`
- App.vue único — `progresso` (0–100) como ref central

## Implementação

### 1. Barra orgânica

- Trilha com cantos generosos; fill com `transition: width 0.6s cubic-bezier(.22, 1, .36, 1)` + shimmer (gradiente animado por keyframes)
- "Orgânico": enquanto o valor muda, o `border-radius` do fill ondula levemente (keyframes alternando valores) e a cabeça do fill tem um glow suave
- Percentual numérico com count-up (rAF interpolando até o valor alvo)

### 2. Bolhas acompanhando o avanço

- Enquanto o progresso muda, emitir bolhas na cabeça do fill (`left = pct%`):
  spans absolutos com keyframes (sobe, escala, fade), tamanho/delay/desvio-horizontal aleatórios, cores do gradiente da barra
- Remoção automática no `animationend`
- Frequência de emissão proporcional à velocidade do avanço

### 3. Demonstração

- Botões: +10, −10, "simular download" (rAF com avanço irregular e pausas aleatórias até 100%), reset
- Ao chegar em 100%: pulso final na barra + explosão de bolhas + selo "concluído"

## Checklist — 100% da descrição

- [ ] Preenchimento com animação orgânica
- [ ] Bolhas/partículas nascendo na cabeça da barra conforme avança
- [ ] Estados 0 → 100% com celebração final
