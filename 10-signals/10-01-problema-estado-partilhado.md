# Lição 10.1: O Problema do Estado Partilhado

!Componentes Distantes

Na nossa aplicação `SneakerGallery`, a comunicação era simples: um componente-pai (`SneakerGallery`) falava com os seus filhos diretos (`SneakerCard`). O pai tinha o estado (a lista de *sneakers* e o carrinho) e passava-o para baixo via `@Input` e recebia eventos de volta via `@Output`.

Isto funciona bem para componentes que estão próximos na árvore de componentes.

## O Desafio da Escalabilidade

Agora, imagina que a nossa aplicação cresce. Queremos adicionar um novo componente, um `<app-header>`, que está no topo da página, fora da galeria. Este cabeçalho precisa de mostrar o número de itens no carrinho.

Como é que o `<app-header>` acede ao estado do `carrinho`, que vive dentro do `SneakerGalleryComponent`?



Temos um problema:
1.  O `HeaderComponent` e o `SneakerGalleryComponent` são "irmãos" ou "primos" na árvore de componentes. Eles não têm uma relação direta de pai-filho.
2.  O estado do carrinho está "preso" dentro da `SneakerGallery`.

## A Solução "Prop Drilling" (e os seus problemas)

A forma "tradicional" de resolver isto sem uma ferramenta de gestão de estado seria:

1.  **Elevar o Estado:** Mover o estado do `carrinho` do `SneakerGalleryComponent` para o seu ancestral comum mais próximo, que poderia ser o `AppComponent`.
2.  **Passar Dados para Baixo (`@Input`):** O `AppComponent` passaria o número de itens do carrinho para o `HeaderComponent` via `@Input`.
3.  **Passar Eventos para Cima (`@Output`):** Quando um `SneakerCard` é clicado, ele emite um evento para a `SneakerGallery`. A `SneakerGallery`, por sua vez, teria de emitir *outro* evento para o `AppComponent`, para que este pudesse atualizar o estado do carrinho.



A isto chama-se **"Prop Drilling"** (perfuração de propriedades). Os dados e eventos têm de "perfurar" múltiplas camadas de componentes.

**Problemas do Prop Drilling:**
-   **Acoplamento:** Componentes intermédios (como a `SneakerGallery`) são forçados a conhecer e a passar dados/eventos que não lhes dizem respeito.
-   **Manutenção Difícil:** Se precisarmos de mais uma informação no `Header`, podemos ter de alterar 3 ou 4 componentes pelo caminho.
-   **Verboso:** O código torna-se repetitivo e complexo.

## A Solução: Um "Cérebro" Central (Store)

E se, em vez de passarmos os dados de mão em mão, tivéssemos uma fonte de verdade central e única? Um "cérebro" ou "armazém" (uma **Store**) que vivesse fora dos componentes.

-   Qualquer componente que precise de dados, vai buscá-los diretamente à Store.
-   Qualquer componente que precise de alterar os dados, envia um comando para a Store.



Isto desacopla os componentes. O `HeaderComponent` e o `SneakerCardComponent` já não precisam de saber um do outro. Ambos falam com a Store.

Tradicionalmente, isto era feito com bibliotecas como o **NgRx**, que usam RxJS e são muito poderosas, mas também complexas.

**Os Angular Signals oferecem uma forma nativa, muito mais simples e intuitiva de criar estas Stores reativas.**

Na próxima lição, vamos aprender as três primitivas dos Signals que nos vão permitir construir o nosso próprio "cérebro" para o carrinho de compras.