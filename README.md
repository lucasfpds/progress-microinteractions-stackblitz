# Barra de Progresso com Microinterações

> Progress bar que se anima de forma orgânica, com bolhas ou partículas acompanhando o avanço.

## Tecnologias

- **Vue 3** com `<script setup>` (JavaScript)
- **Vite** como bundler
- **CSS puro** com keyframes (shimmer, organic, rise, pop)
- **requestAnimationFrame** para count-up do percentual
- Animação de bolhas via spans absolutos removidos no `animationend`

## Requisitos

- Node.js 18+

## Como executar

```bash
npm install
npm run dev
```

Abra http://localhost:5173 no navegador.

## Scripts

| Script | Descrição |
| --- | --- |
| `npm run dev` | Servidor de desenvolvimento (Vite) |
| `npm run build` | Build de produção |
| `npm run preview` | Serve o build de produção |

## Estrutura

```
src/
  main.js              entry: cria o app e importa o CSS
  App.vue              barra, bolhas, count-up, botões de demo
  styles.css           barra, fill, shimmer, bolhas, selo, controles
```

### Funcionalidades

- Trilha com cantos generosos; fill com `transition: width 0.6s cubic-bezier(.22, 1, .36, 1)`
- Shimmer: gradiente animado por keyframes
- Orgânico: `border-radius` do fill ondula levemente enquanto o valor muda
- Glow suave na cabeça do fill
- Percentual numérico com count-up (rAF interpolando até o valor alvo)
- Bolhas nascem na cabeça do fill (`left = pct%`): spans com keyframes (sobe, escala, fade)
- Tamanho, delay e desvio-horizontal aleatórios; cores do gradiente da barra
- Remoção automática no `animationend`
- Frequência de emissão proporcional à velocidade do avanço
- Botões: +10, −10, "simular download" (rAF com avanço irregular e pausas aleatórias), reset
- Ao chegar em 100%: pulso final na barra + explosão de bolhas + selo "concluído"


## Rodando no StackBlitz

Basta importar o repositório e rodar normalmente:

```
https://stackblitz.com/github/lucasfpds/progress-microinteractions-stackblitz
```

O projeto não depende de binários nativos — Vite é JavaScript puro, então
sobe sem bindings nativos no WebContainer do StackBlitz.
