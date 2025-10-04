# Lição 1.2: As Apps Essenciais – Angular CLI

![Canivete Suíço](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExbWw5a2N5c3JqY254b250b294c251b294c251b294c251b294/l2SpSQLgV3z3b3gIM/giphy.gif)

Para trabalharmos com um *framework* tão completo como o Angular, precisamos de uma ferramenta à altura que nos ajude a gerir a complexidade. A equipa da Google criou a ferramenta perfeita para isso: o **Angular CLI** (Command-Line Interface).

**Analogia:** O Angular CLI é o teu **canivete suíço de developer Angular**. Tem uma ferramenta para tudo:
-   Queres criar um projeto novo? `ng new`
-   Queres criar um novo componente? `ng generate component`
-   Queres correr a tua aplicação para a testares? `ng serve`
-   Queres preparar a aplicação para produção? `ng build`

É uma ferramenta de linha de comandos que automatiza tarefas repetitivas e garante que os teus projetos seguem sempre as melhores práticas.

## Passo 1: Instalar o Angular CLI

O Angular CLI é um pacote NPM, tal como o `express` ou o `react`. No entanto, como o vamos usar para criar e gerir múltiplos projetos, instalâmo-lo de forma **global** no nosso sistema, e não apenas dentro de um projeto.

1.  Abre o teu terminal (PowerShell, CMD, Terminal do macOS/Linux).
2.  Corre o seguinte comando. A flag `-g` significa "global".

    ```sh
    npm install -g @angular/cli
    ```

    *(Nota: Em macOS ou Linux, poderás precisar de correr este comando com `sudo` à frente, dependendo das tuas permissões: `sudo npm install -g @angular/cli`)*

## Passo 2: Verificar a Instalação

Depois da instalação terminar, podes verificar se tudo correu bem. O comando principal do Angular CLI é `ng`.

Corre no terminal:

```sh
ng --version
```

Se tudo estiver correto, vais ver um grande logo do Angular em ASCII art e uma lista de informações sobre a tua versão do Angular CLI, Node.js e do teu sistema operativo.

## Os Comandos Mais Importantes (para já)

Não precisas de decorar todos os comandos, mas familiariza-te com estes:

-   `ng new <nome-do-projeto>`: Cria uma nova área de trabalho Angular com uma aplicação inicial.
-   `ng serve`: Arranca o servidor de desenvolvimento, compila a aplicação e fica a "ouvir" por alterações nos teus ficheiros, atualizando o browser automaticamente.
-   `ng generate <tipo> <nome>` (ou `ng g <tipo> <nome>`): Gera novos "blocos de construção" para a tua aplicação. Os mais comuns são:
    -   `ng g component meu-componente`
    -   `ng g service meu-servico`
    -   `ng g module meu-modulo`

---

**Ferramenta na mão!** 🛠️

Com o Angular CLI instalado, tens o poder de criar, gerir e construir aplicações Angular de forma profissional e eficiente.

Na próxima lição, vamos usar o comando `ng new` para criar o nosso primeiro projeto e vamos explorar a estrutura de pastas e ficheiros que o CLI gera para nós.