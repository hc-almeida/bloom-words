# 🌸 Bloom Words

Uma aplicação web de micro-aprendizado focada em expandir o vocabulário de inglês do usuário através de flashcards gerados por Inteligência Artificial e gamificação imersiva.

Projeto desenvolvido para a disciplina de Front-end Engineering (FIAP).

---

## 👥 Integrantes do Grupo

* LIVIA CARVALHO KELLER
* GABRIEL VITOR DE SOUZA
* HELLEN CAROLINE ALMEIDA AZEVEDO
* LUISA GABRIELE DA PURIFICAÇÃO
* THIAGO HIDEKI SASAOKA SANCHES

---

## 🚀 Tecnologias Utilizadas

Este projeto foi construído utilizando as ferramentas mais modernas do ecossistema front-end:

* **[Vue 3](https://vuejs.org/)**: Framework reativo usando estritamente a *Composition API* (`<script setup>`).
* **[Vite](https://vitejs.dev/)**: Build tool para desenvolvimento e bundling.
* **[Tailwind CSS](https://tailwindcss.com/)**: Estilização baseada em utilitários, utilizando conceitos de *Glassmorphism* e paletas em *Dark Neon*.
* **Web Speech API**: Utilização do `window.speechSynthesis` nativo do navegador para a pronúncia correta em `en-US` das palavras geradas.

## 🎯 Funcionalidades e Modos de Jogo

1.  **The First Bloom (Learning Mode):**
    * Consome a API BFF e exibe 5 flashcards estáticos contendo a palavra, descrição em português e um exemplo de uso em inglês.
    * Animações em cascata (staggered) simulando o desabrochar das palavras.
2.  **BOOM Mode (Challenge Mode):**
    * Um minigame de tensão no estilo "desarme a bomba".
    * O usuário possui um cronômetro de 15 segundos para ler a descrição em português e clicar na palavra correta em inglês para desarmar a fase.
    * Design totalmente responsivo com cursor customizado (mira tática) feito em SVG embutido.

## 🔗 Links do Projeto

* **Deploy da Aplicação:** [https://bloom-words.netlify.app/](https://bloom-words.netlify.app/)
* **API (BFF) Utilizada:** `https://fiap-bff-2502.onrender.com/ask` *(Nota: O back-end utilizado no momento é o de testes fornecido pelo professor).*

---

## 📊 Métricas de Qualidade (Web Vitals - Lighthouse)

Para garantir a qualidade e a melhor experiência para o usuário, o projeto foi submetido a uma auditoria utilizando o **Google Lighthouse** em ambiente de produção (aba anônima para evitar interferência de extensões).

**📄 [Clique aqui para visualizar o PDF completo do Relatório do Lighthouse](./lighthouse-report.pdf)**

### Resultados Obtidos:
* **Performance: 89/100**
  * *O que significa:* Avalia a velocidade de carregamento da página e a estabilidade visual. Mede o quão rápido o usuário consegue ver o conteúdo principal (LCP) e se a tela dá "pulos" inesperados (CLS).
* **Accessibility (Acessibilidade): 100/100**
  * *O que significa:* Avalia se o site pode ser navegado e compreendido por todos os usuários, incluindo pessoas com deficiências (uso de leitores de tela, contraste de cores adequado, tags semânticas e navegação por teclado).
* **Best Practices (Melhores Práticas): 100/100**
  * *O que significa:* Verifica a saúde geral do código. Garante que o site não usa APIs descontinuadas do navegador, se as conexões são seguras (HTTPS) e se o código segue os padrões modernos da Web.
* **SEO (Otimização para Motores de Busca): 91/100**
  * *O que significa:* Avalia o quão fácil é para o Google e outros buscadores entenderem e ranquearem a página (presença de meta tags, título descritivo, atributo de idioma correto, etc.).

---
