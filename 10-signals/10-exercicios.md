# Módulo 10: Exercícios Práticos – O Carrinho de Compras Global

Vamos aplicar os teus novos superpoderes com Signals para criar um sistema de carrinho de compras global, desacoplado e reativo.

---

## 🥉 Exercício Nível Bronze: A Store Básica

**O Problema:** Precisamos de uma fonte central para o estado do nosso carrinho. Vamos criar o serviço de store com o estado principal e uma ação para adicionar itens.

**A tua Missão:**

1.  Gera um novo serviço: `ng g s store/cart`.
2.  No `cart.service.ts`:
    *   Importa `signal` e as interfaces `Sneaker` e `CartItem` (podes ter de criar esta última).
    *   Cria um `signal` **privado** chamado `cartItems` para guardar um array de `CartItem[]`, inicializado como um array vazio.
    *   Cria um método **público** `addSneaker(sneaker: Sneaker)`.
    *   Dentro de `addSneaker`, usa `this.cartItems.update()` para adicionar o novo sneaker ao carrinho. A lógica deve verificar se o item já existe: se sim, incrementa a quantidade; se não, adiciona um novo `CartItem` ao array.

---

## 🥈 Exercício Nível Prata: Dados Derivados e Leitura do Estado

**O Problema:** Os nossos componentes precisam de ler o estado do carrinho (como o número total de itens) sem poder modificá-lo diretamente.

**A tua Missão:**

1.  No `cart.service.ts`:
    *   Importa `computed`.
    *   Cria um `signal` **público** e `readonly` chamado `totalItems`. Este deve ser um `computed` signal que calcula o número total de sapatilhas no carrinho (somando as `quantity` de todos os `CartItem`s).
2.  Cria um novo componente: `ng g c header`.
3.  No `header.component.ts`:
    *   Injeta o `CartService` (usando `inject()`).
    *   Expõe o serviço como uma propriedade pública para que o template o possa usar.
4.  No `header.component.html`:
    *   Mostra o número total de itens lendo diretamente do `signal` do serviço: `Carrinho: {{ cartService.totalItems() }}`.
5.  Adiciona o `<app-header>` ao teu `app.component.html`.

---

## 🥇 Projeto Nível Ouro: Desacoplamento Total

**O Problema:** O nosso `SneakerCardComponent` ainda usa `@Output` para comunicar com o pai. Vamos refatorá-lo para que ele comunique diretamente com a store, completando o desacoplamento.

**A tua Missão:**

1.  No `sneaker-card.component.ts`:
    *   Remove a propriedade `@Output() addToCart` e o `EventEmitter`.
    *   Injeta o `CartService`.
    *   Altera o método `onAddToCartClick()` para que ele chame diretamente o método `addSneaker()` do `cartService`.
2.  No `sneaker-gallery.component.html` (o pai):
    *   Remove o *event binding* `(addToCart)` da tag `<app-sneaker-card>`.
3.  No `sneaker-gallery.component.ts`:
    *   Remove o método `handleAddToCart()` e a propriedade `carrinho`. Este componente agora só se preocupa em mostrar a galeria, não em gerir o estado do carrinho.

Testa a aplicação. Ao clicares num `SneakerCard`, o contador no `HeaderComponent` deve atualizar-se instantaneamente, provando que os teus componentes estão a comunicar através da store central, sem se conhecerem. Parabéns!