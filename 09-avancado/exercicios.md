# Módulo 9: Exercícios Práticos – Refatorar o SneakerCard

Vamos aplicar os teus conhecimentos de comunicação entre componentes para transformar o nosso `SneakerCard` num componente verdadeiramente reutilizável e interativo.

---

## 🥉 Exercício Nível Bronze: Componente "Dumb" com `@Input`

**O Problema:** O nosso `SneakerCard` (ou a `div` que o representa na galeria) está a ser criado dentro de um `*ngFor`, mas ele não recebe dados. Vamos torná-lo num componente "burro" (dumb) que apenas mostra os dados que lhe são passados.

**A tua Missão:**

1.  Gera um novo componente: `ng g c sneaker-card`.
2.  No `sneaker-card.component.ts`:
    *   Importa o decorator `Input` do `@angular/core`.
    *   Cria uma propriedade chamada `sneaker` e marca-a com `@Input()`. Esta propriedade deve ser do tipo `Sneaker` (podes ter de criar ou importar a interface `Sneaker`).
3.  No `sneaker-card.component.html`:
    *   Cria o HTML para um cartão de produto. Ele deve mostrar o nome, a marca e a imagem do *sneaker*, usando o objeto `sneaker` que vem do `@Input`.
4.  No `sneaker-gallery.component.html`:
    *   Dentro do teu `*ngFor`, substitui o HTML do cartão que tinhas pela tag `<app-sneaker-card>`.
    *   Usa *property binding* para passar o objeto `sneaker` de cada iteração do loop para o `@Input` do `SneakerCardComponent`: `[sneaker]="sneaker"`.

---

## 🥈 Exercício Nível Prata: Interatividade com `@Output`

**O Problema:** O nosso `SneakerCard` é apenas visual. Queremos que ele reaja a ações do utilizador e notifique o componente-pai.

**A tua Missão:**

1.  No `sneaker-card.component.html`, adiciona um botão "Adicionar ao Carrinho".
2.  No `sneaker-card.component.ts`:
    *   Importa `Output` e `EventEmitter` do `@angular/core`.
    *   Cria uma nova propriedade chamada `addToCart` e marca-a com `@Output()`. Inicializa-a como um `new EventEmitter<Sneaker>()`.
    *   Cria um método que será chamado quando o botão for clicado. Dentro deste método, usa `this.addToCart.emit(this.sneaker)` para emitir o evento com os dados do *sneaker* atual.
3.  No `sneaker-card.component.html`, liga o evento `(click)` do botão ao método que criaste.
4.  No `sneaker-gallery.component.ts` (o pai):
    *   Cria um array para o `carrinho` e um método `handleAddToCart(sneaker: Sneaker)` que adiciona o *sneaker* recebido a esse array.
5.  No `sneaker-gallery.component.html`:
    *   Na tag `<app-sneaker-card>`, usa *event binding* para "ouvir" o evento `(addToCart)` e chamar o método `handleAddToCart($event)`.
    *   Mostra em algum lado o número de itens no carrinho: `Carrinho: {{ carrinho.length }}`.

---

## 🥇 Projeto Nível Ouro: Componente de Layout com `<ng-content>`

**O Problema:** O nosso `SneakerCard` tem a sua própria estrutura de "cartão". E se quiséssemos ter outros tipos de cartões na nossa app com o mesmo estilo base? Vamos criar um componente de layout genérico.

**A tua Missão:**

1.  Cria um novo componente genérico: `ng g c ui/card`.
2.  No `card.component.html`, cria uma estrutura de cartão base (uma `div` com uma classe `card`, por exemplo). Dentro dele, adiciona "ranhuras" para o conteúdo:
    *   Um `<ng-content select="[card-header]"></ng-content>` para o título.
    *   Um `<ng-content select="[card-body]"></ng-content>` para o corpo/imagem.
    *   Um `<ng-content select="[card-footer]"></ng-content>` para os botões/ações.
3.  Refatora o `sneaker-card.component.html`:
    *   Envolve todo o conteúdo com a tag `<app-card>`.
    *   Adiciona os atributos correspondentes (`card-header`, `card-body`, `card-footer`) aos elementos que queres projetar para cada ranhura. Por exemplo: `<h2 card-header>{{ sneaker.nome }}</h2>`.

Parabéns! Agora tens um `SneakerCard` que é um componente de apresentação "burro", interativo e que usa um componente de layout genérico, demonstrando um domínio completo dos padrões de composição do Angular.