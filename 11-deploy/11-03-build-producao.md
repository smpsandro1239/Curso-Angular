# Lição 11.3: Build de Produção: Otimizar para a Performance

!Fábrica

Até agora, temos trabalhado no nosso "estúdio de desenvolvimento" com o comando `ng serve`. Este comando é fantástico para desenvolvimento porque é rápido, atualiza o browser automaticamente (live-reload) e dá-nos mapas do nosso código para facilitar a depuração (debugging).

No entanto, o resultado do `ng serve` não é otimizado para ser servido a utilizadores reais. É lento, pesado e contém muita informação extra que só é útil para nós, developers.

Para preparar a nossa aplicação para o mundo, precisamos de criar um **build de produção**.

**Analogia:** Pensa na construção de um carro. O `ng serve` é o protótipo na fábrica: tem fios expostos, peças por pintar e ferramentas à volta para que os engenheiros possam fazer ajustes rápidos. O **build de produção** é o carro final que sai da linha de montagem: polido, pintado, com tudo no sítio, otimizado para performance e segurança, e pronto para ir para o stand.

## O Comando `ng build`

O Angular CLI torna este processo incrivelmente simples com o comando `ng build`. Quando corremos este comando, ele executa uma série de otimizações poderosas:

1.  **Compilação AOT (Ahead-of-Time):** O Angular compila os teus templates HTML e componentes para código JavaScript otimizado *antes* de o browser o receber. Isto torna a renderização inicial muito mais rápida.
2.  **Minificação (Minification):** Remove todos os espaços em branco, comentários e encurta os nomes das variáveis no teu código JavaScript e CSS. O resultado é funcionalmente idêntico, mas os ficheiros são muito mais pequenos.
3.  **Uglification/Tersering:** Transforma o teu código de uma forma que o torna mais difícil de ler por humanos (mas perfeitamente legível pela máquina), reduzindo ainda mais o tamanho.
4.  **Tree Shaking:** "Abana a árvore" do teu código e deixa cair todo o código que nunca é usado (funções, classes, etc.). Se importaste uma biblioteca inteira mas só usaste uma função, o resto é removido.
5.  **Bundling:** Agrupa o teu código em ficheiros "bundle" otimizados.

## Como Fazer o Build

No terminal, na raiz do teu projeto, corre o seguinte comando:

```sh
ng build
```

*(Por defeito, `ng build` já assume uma configuração de produção. Antigamente era necessário usar a flag `--prod`, mas isso já não é preciso nas versões modernas do Angular CLI.)*

O processo pode demorar um ou dois minutos.

## O Resultado: A Pasta `dist`

Quando o build termina, vais ver uma nova pasta na raiz do teu projeto chamada `dist/nome-do-projeto/`.

Esta pasta contém **tudo** o que é necessário para a tua aplicação correr. São apenas ficheiros estáticos (HTML, CSS, JavaScript, imagens). Não há código TypeScript, não há `node_modules`. É um site autónomo e otimizado.

Podes pegar nesta pasta e colocá-la em qualquer servidor web, e a tua aplicação irá funcionar.

## Testar o Build Localmente

Como podes ver o resultado do teu build antes de o publicares? Não podes simplesmente abrir o `index.html` no browser, porque a aplicação precisa de ser servida por um servidor web.

Uma forma fácil de o fazer é com um pacote NPM chamado `http-server`.

1.  Instala-o globalmente (a flag `-g` torna-o disponível em qualquer pasta):
    ```sh
    npm install -g http-server
    ```
2.  Navega para dentro da pasta do teu build:
    ```sh
    cd dist/nome-do-projeto
    ```
3.  Inicia o servidor:
    ```sh
    http-server -o
    ```

Isto vai iniciar um servidor web simples e abrir a tua aplicação de produção no browser. Vais notar que ela carrega muito mais depressa!

Agora que temos a nossa "caixa" com a aplicação pronta a ser enviada, a última lição vai mostrar como a colocar num serviço de alojamento global para que qualquer pessoa no mundo a possa visitar.