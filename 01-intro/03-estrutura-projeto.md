# Lição 1.3: A Estrutura do Projeto Angular

![Estrutura](https://media.giphy.com/media/3o7TKSjRrfIPjeiVyE/giphy.gif)

Com o Angular CLI instalado, estamos prontos para criar a nossa primeira aplicação. O CLI vai fazer todo o trabalho pesado, criando uma estrutura de pastas e ficheiros otimizada e pronta a usar.

**Analogia:** Pensa num móvel do IKEA. O Angular CLI é a caixa que recebes. O comando `ng new` é o ato de abrir a caixa e encontrar todas as peças pré-cortadas, os parafusos e o manual de instruções. Já está tudo lá, só precisas de começar a montar.

## Passo 1: Criar o Projeto com `ng new`

1.  Abre o teu terminal numa pasta onde queres guardar os teus projetos (ex: `Documentos/Projetos`).
2.  Corre o comando `ng new`. Vamos chamar ao nosso primeiro projeto `minha-primeira-app`.

    ```sh
    ng new minha-primeira-app
    ```

3.  O CLI vai fazer-te algumas perguntas para configurar o projeto:
    -   **Would you like to add Angular routing?** (Gostarias de adicionar rotas Angular?)
        -   Escreve `y` (de *yes*) e prime Enter. O *routing* é o que nos permite ter múltiplas "páginas" na nossa aplicação. É quase sempre necessário.
    -   **Which stylesheet format would you like to use?** (Que formato de folha de estilos gostarias de usar?)
        -   Usa as setas para selecionar **SCSS** e prime Enter. O SCSS é um "superconjunto" do CSS que nos dá superpoderes, como variáveis e funções. É o padrão da indústria.

4.  O CLI vai agora criar a pasta `minha-primeira-app` e instalar todas as dependências necessárias. Isto pode demorar um ou dois minutos.

## Passo 2: Correr a Aplicação

Depois de tudo instalado, vamos pôr a aplicação a funcionar.

1.  Entra na pasta do projeto que acabaste de criar:
    ```sh
    cd minha-primeira-app
    ```
2.  Usa o comando `ng serve` para arrancar o servidor de desenvolvimento:
    ```sh
    ng serve
    ```
    Podes adicionar a flag `--open` (ou `-o`) para que ele abra o browser automaticamente: `ng serve --open`.

3.  O terminal vai compilar a aplicação e, no final, mostrar uma mensagem a dizer que o servidor está a correr em `http://localhost:4200/`.

Abre esse endereço no teu browser. Vais ver uma página de boas-vindas do Angular. Parabéns, a tua primeira aplicação Angular está no ar!

## A Estrutura de Pastas

Abre a pasta `minha-primeira-app` no VS Code. Pode parecer assustador, mas vamos focar-nos no essencial.

-   **`src/`**: A pasta "source" (código-fonte). É aqui que vamos viver.
    -   **`index.html`**: A única página HTML da nossa aplicação. Repara que tem uma tag estranha: `<app-root></app-root>`. É aqui que o Angular vai "injetar" o nosso componente principal.
    -   **`main.ts`**: O ponto de entrada da aplicação. É o primeiro ficheiro a ser executado e é ele que "arranca" (bootstraps) o módulo principal da nossa aplicação.
    -   **`styles.scss`**: O ficheiro de estilos globais da nossa aplicação.
    -   **`app/`**: Esta é a pasta mais importante. Contém os blocos de construção da nossa aplicação.
        -   **`app.component.ts`**: A lógica (TypeScript) do nosso componente principal.
        -   **`app.component.html`**: A estrutura (HTML) do nosso componente principal.
        -   **`app.component.scss`**: Os estilos (SCSS) específicos do nosso componente principal.
        -   **`app.module.ts`**: O módulo principal (ou raiz) da nossa aplicação. É ele que "declara" quais os componentes que fazem parte deste módulo.
        -   **`app-routing.module.ts`**: O ficheiro que vai gerir as nossas rotas/páginas.

-   **`angular.json`**: O "cérebro" do Angular CLI. É um ficheiro de configuração gigante que diz ao CLI como construir, testar e servir a nossa aplicação. Raramente precisamos de mexer aqui no início.

-   **`package.json`**: O ficheiro que gere as dependências e scripts do nosso projeto, tal como já vimos.

## O Teu Primeiro "Olá Mundo" Angular

Vamos fazer uma pequena alteração para provar que estamos no controlo.

1.  Abre o ficheiro `src/app/app.component.html`.
2.  Apaga todo o conteúdo que lá está.
3.  Escreve o seguinte:
    ```html
    <h1>Olá, Mundo Angular!</h1>
    <p>A minha primeira app está a funcionar!</p>
    ```
4.  Guarda o ficheiro. O `ng serve` deteta a alteração, recompila a aplicação e o teu browser atualiza-se automaticamente. Vais ver a tua nova mensagem!

---

**Setup Completo!** ✅

Concluíste o Módulo 1! Agora sabes:
-   O que é o Angular e o seu ecossistema.
-   Como usar o Angular CLI para criar e servir uma aplicação.
-   A estrutura básica de um projeto Angular.

No próximo módulo, vamos mergulhar no conceito mais fundamental do Angular: os **Componentes**.