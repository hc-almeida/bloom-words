# Bloom Words - Arquitetura e Diretrizes de Desenvolvimento (Step 1)

## 1. Visão Geral do Projeto
"Bloom Words" é uma aplicação web de micro-aprendizado de vocabulário em inglês. O objetivo deste "Step 1" é entregar a experiência inicial ("The First Bloom"), focada em uma interface de visualização estática dos flashcards recém-carregados da API, com suporte a pronúncia nativa em áudio.

## 2. Stack Tecnológica
- **Framework:** Vue 3 (utilizando estritamente a Composition API com `<script setup>`).
- **Build Tool:** Vite.
- **Estilização:** Tailwind CSS. Evitar CSS customizado; focar em classes utilitárias.

## 3. Identidade Visual e Theming
O design baseia-se em um "Dark Mode Neon" que remete a crescimento, energia e fluência.
- **Background Principal:** `bg-slate-900` cobrindo 100% da viewport (`min-h-screen`).
- **Cards (Superfícies):** `bg-slate-800` com bordas arredondadas (`rounded-2xl`).
- **Cores de Destaque (Neon/Bloom):** Gradientes combinando Roxo e Rosa (`from-purple-500 to-pink-500`). Usar em textos de destaque e botões.
- **Tipografia:** Clean e moderna, texto descritivo em `slate-300`.

## 4. Arquitetura de Componentes

### 4.1. `App.vue` (Smart/Container Component)
- **Estado:** Controlar `isLoading` (boolean), `hasError` (boolean), e `words` (array).
- **Integração BFF:** Criar uma função assíncrona `fetchWords` que faça um GET na rota `https://fiap-bff-2502.onrender.com/ask`. Chamar essa função no `onMounted`.
- **Loading State:** Quando `isLoading` for `true`, esconder os cards e exibir um texto centralizado "Desabrochando palavras..." com a classe `animate-pulse` e as cores do tema.
- **Botão de Refresh:** Quando os cards estiverem carregados (`isLoading` false e `words.length > 0`), exibir abaixo do grid um botão "✨ Florescer Novas Palavras". Ao clicar, esse botão deve chamar `fetchWords` novamente para buscar 5 novas palavras.
- **Layout:** Estrutura em grid ou flex para centralizar o título principal, os cards e o botão de refresh de forma responsiva.

### 4.2. `FlashCard.vue` (Dumb/Presentational Component)
- **Responsabilidade:** Renderizar os dados de uma única palavra e lidar com a interação isolada (áudio).
- **Props:** Recebe um objeto `card` contendo `word`, `description`, e `useCase`.
- **Layout Interno (Estático):** - `word` no topo em destaque (gradiente roxo/rosa).
  - Um ícone de áudio (🔊) alinhado APENAS com o título (`word`).
  - `description` em cinza claro.
  - Linha divisória sutil.
  - `useCase` em itálico na parte inferior.
- **Integração de Áudio:** Implementar `window.speechSynthesis`. Ler exclusivamente no idioma `en-US`, apenas a string `word`, com velocidade (`rate`) de `0.85`.

## 5. Animações e Micro-interações
- **Efeito "The First Bloom":** Ao renderizar a lista de `FlashCard`, os cards devem surgir com uma transição (ex: de `scale-0 opacity-0` para `scale-100 opacity-100`).
- **Staggered Animation:** Utilizar o índice (index) do loop para aplicar um `delay` entre os cards, dando a sensação de que estão nascendo um após o outro.