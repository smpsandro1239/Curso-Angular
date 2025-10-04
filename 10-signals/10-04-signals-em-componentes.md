# Lição 10.4: Signals em Componentes: O Futuro da Deteção de Alterações

!Magia

Temos o nosso "cérebro" (`CartService`) pronto. Agora, vamos ligar os nossos componentes a ele e testemunhar a simplicidade e o poder dos Signals em ação.

Vamos refatorar a nossa aplicação para que:
1.  O `SneakerCardComponent` adicione itens ao carrinho chamando o serviço.
2.  Um novo `HeaderComponent` mostre o total de itens do carrinho, lendo diretamente do serviço.

## Passo 1: Ligar o `SneakerCardComponent` à Store

O nosso `SneakerCardComponent` já não precisa de "gritar" para o pai com `@Output`. Ele pode falar diretamente com a Store.

**`src/app/sneaker-card/sneaker-card.component.ts` (Refatorado)**
```typescript
import { Component, Input, inject } from '@angular/core'; // 1. inject
import { Sneaker } from '../interfaces/sneaker.interface';
import { CartService } from '../store/cart.service'; // 2. Importar o serviço

@Component({
  selector: 'app-sneaker-card',
  // ...
})
export class SneakerCardComponent {
  @Input() sneaker!: Sneaker;

  // Já não precisamos de @Output() e EventEmitter!

  // 3. Injetar o serviço. `inject()` é a forma moderna de o fazer.
  private cartService = inject(CartService);

  onAddToCartClick(): void {
    // 4. Chamar diretamente o método da store.
    this.cartService.addSneaker(this.sneaker);
    console.log(`${this.sneaker.nome} adicionado ao carrinho através da store!`);
  }
}
```

O `SneakerGalleryComponent` (o pai) agora fica muito mais simples. Ele já não precisa de gerir o estado do carrinho nem de ouvir eventos.

**`src/app/sneaker-gallery/sneaker-gallery.component.html` (Simplificado)**
```html
<div class="gallery-grid">
  <app-sneaker-card
    *ngFor="let s of sneakers"
    [sneaker]="s">
    <!-- Já não há (addToCart) binding! -->
  </app-sneaker-card>
</div>
```

## Passo 2: Criar o `HeaderComponent` para Ler da Store

Agora, a parte mágica. Vamos criar um componente completamente separado que vai mostrar o total de itens no carrinho.

1.  Gera o componente: `ng g c header`
2.  No `HeaderComponent`, vamos injetar o `CartService` e ler o `signal` computado `totalItems`.

**`src/app/header/header.component.ts`**
```typescript
import { Component, inject } from '@angular/core';
import { CartService } from '../store/cart.service';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
})
export class HeaderComponent {
  // Injeta o serviço
  public readonly cartService = inject(CartService);
}
```

**`src/app/header/header.component.html`**
```html
<header>
  <h1>Sneaker Store</h1>
  <div class="cart-status">
    <!-- Lemos o signal diretamente no template! -->
    Carrinho: {{ cartService.totalItems() }} itens
  </div>
</header>
```

## O Resultado

Adiciona `<app-header></app-header>` ao teu `app.component.html`. Agora, quando clicares no botão "Adicionar ao Carrinho" em qualquer `SneakerCard`, o `HeaderComponent` vai-se atualizar **automaticamente**.

Não precisámos de `@Input`, `@Output`, `Observables` ou do `async` pipe. O `HeaderComponent` simplesmente lê o `signal` `totalItems`. Como este `signal` é `computed` e depende do `signal` `cartItems`, o Angular sabe que, quando `cartItems` muda, qualquer componente que leia `totalItems` precisa de ser atualizado.

Esta é a reatividade fina e granular dos Signals. É mais simples de escrever, mais fácil de ler e mais eficiente.

---

**Módulo 10 Concluído!** 🏆

Dominaste a forma moderna de gerir estado em Angular. Sabes o que são Signals, como criar uma store reativa e como consumir esse estado nos teus componentes de uma forma limpa e eficiente.

Estás pronto para o projeto do módulo: **O Carrinho de Compras Global**.