# Módulo 10: Gestão de Estado (State Management) com Signals

![Cérebro](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExbWw5a2N5c3JqY254b250b294c251b294c251b294c251b294/l41lHw24p23v5TzSE/giphy.gif)

À medida que as nossas aplicações crescem, partilhar dados entre componentes distantes torna-se um desafio. Passar dados através de múltiplos níveis de `@Input` e `@Output` (conhecido como *prop drilling*) é complicado e propenso a erros. Precisamos de um "cérebro" central para a nossa aplicação.

Este módulo apresenta os **Angular Signals**, a nova e revolucionária forma de gerir estado no Angular. Os Signals oferecem uma reatividade mais fina e um código mais simples e legível em comparação com as abordagens tradicionais baseadas em RxJS.

**Analogia:** Pensa numa folha de cálculo como o Excel.
-   **Estado tradicional (RxJS):** Se mudas o valor de uma célula, tens de dizer manualmente a todas as outras células que dependem dela para se recalcularem.
-   **Estado com Signals:** Se mudas o valor da célula A1, qualquer fórmula que use A1 (como `=A1 * 2`) atualiza-se **automaticamente e instantaneamente**. Não precisas de gerir subscrições nem de te preocupar com a deteção de alterações.

---

## 🎯 Objetivos de Aprendizagem

No final deste módulo, serás capaz de:

- ✅ Explicar o problema da gestão de estado em aplicações complexas.
- ✅ Entender o que são os Angular Signals e como funcionam.
- ✅ Usar as três primitivas dos Signals: `signal()`, `computed()` e `effect()`.
- ✅ Criar um serviço de "store" reativo usando apenas Signals.
- ✅ Refatorar uma aplicação para usar um padrão de gestão de estado centralizado.

## 🛠️ Pré-requisitos

- Domínio de Serviços e Injeção de Dependências (Módulo 5).
- Domínio de comunicação entre componentes (Módulo 9).

---

## 📚 Lições

1.  **O Problema do Estado Partilhado (Revisão)**
2.  **Introdução aos Signals: `signal()`, `computed()` e `effect()`**
3.  **Criar um "Store" Reativo com Signals**
4.  **Signals em Componentes: O Futuro da Deteção de Alterações**

## 🏆 Projeto do Módulo

**O Carrinho de Compras Global:** Vamos refatorar a nossa `SneakerGallery` para usar um `CartStoreService` baseado em Signals.

-   O estado do carrinho (os itens e a quantidade total) viverá dentro do `CartStoreService` como `signals`.
-   O `SneakerCardComponent` vai chamar um método no serviço para adicionar um item.
-   Um novo `HeaderComponent` vai ler o número de itens diretamente do `CartStoreService` usando um `computed` signal, atualizando-se automaticamente sem qualquer `@Input` ou `@Output`.

---

Prepara-te para simplificar drasticamente a forma como geres o estado nas tuas aplicações Angular!