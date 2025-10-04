# Lição 5.1: O Problema da Lógica no Componente

!Emaranhado

Até agora, fizemos um ótimo trabalho a construir componentes. O nosso `TodoListComponent` funciona perfeitamente: ele tem a sua própria lista de tarefas, métodos para as adicionar, métodos para as alterar...

Mas agora surge um novo requisito: queremos mostrar no nosso `HeaderComponent` um contador com o número de tarefas pendentes.

Como é que o `HeaderComponent` acede à lista de tarefas que "vive" dentro do `TodoListComponent`?

As opções são complicadas:
-   Poderíamos tentar passar os dados do filho (`TodoListComponent`) para o pai (`AppComponent`) e depois do pai para o outro filho (`HeaderComponent`). Isto é complexo e cria um acoplamento forte entre os componentes.
-   Poderíamos copiar a lista de tarefas para o `HeaderComponent`. Péssima ideia! Teríamos duas fontes de verdade e os dados nunca estariam sincronizados.

Este é um problema clássico em aplicações que crescem. A lógica de negócio e os dados não devem pertencer a um componente específico.

## O Princípio da Responsabilidade Única

Um bom design de software segue o **Princípio da Responsabilidade Única (Single Responsibility Principle)**. Aplicado ao Angular:

> A responsabilidade de um **Componente** é controlar a **vista** (a UI). Ele deve ser responsável por apresentar dados e delegar as ações do utilizador, mas não deve ser responsável por gerir o estado global da aplicação ou por lógica de negócio complexa.

O nosso `TodoListComponent` está a violar este princípio. Ele está a ser, ao mesmo tempo:
1.  O controlador da vista da lista de tarefas.
2.  A "base de dados" que guarda as tarefas.

## A Solução: Serviços (Services)

A solução do Angular para este problema é extrair a lógica e os dados para uma classe separada, reutilizável, chamada **Serviço**.

**Analogia:** O teu `TodoListComponent` é o ecrã do teu telemóvel que mostra a tua localização no mapa. O `HeaderComponent` é o widget na barra de notificações que também mostra a tua localização. Nenhum deles calcula a localização. Ambos pedem a localização ao **"Serviço de GPS"** do sistema operativo.

O Serviço torna-se a **fonte única da verdade**.

-   **`TaskService` (O nosso novo serviço):**
    -   Responsável por guardar o array de tarefas.
    -   Responsável por ter os métodos `getTarefas()`, `adicionarTarefa()`, `toggleConcluida()`.

-   **`TodoListComponent`:**
    -   Pede as tarefas ao `TaskService`.
    -   Quando o utilizador clica, chama o método correspondente no `TaskService`.

-   **`HeaderComponent`:**
    -   Pede as tarefas ao `TaskService` para poder calcular quantas estão pendentes.

Desta forma, os nossos componentes ficam "burros" e focados apenas na apresentação, enquanto a lógica de negócio fica centralizada, organizada e reutilizável.

Na próxima lição, vamos usar o Angular CLI para criar o nosso primeiro serviço e começar a refatorar a nossa To-Do List.