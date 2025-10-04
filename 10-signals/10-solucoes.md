# Módulo 10: Soluções dos Exercícios – O Carrinho de Compras Global

Aqui está a solução completa para o projeto. O resultado é uma aplicação com um estado global reativo, onde os componentes comunicam através de um serviço central (store), sem se conhecerem.

---

### `src/app/store/cart.service.ts` (Solução Completa)

Este é o "cérebro" da nossa aplicação. Ele contém o estado, os dados derivados e as ações para modificar o estado.

```typescript
import { Injectable, signal, computed } from '@angular/core';
import { Sneaker } from '../interfaces/sneaker.interface';

export interface CartItem {
  sneaker: Sneaker;
  quantity: number;
}

@Injectable({
  providedIn: 'root'
})
export class CartService {
  // Estado privado
  private cartItems = signal<CartItem[]>([]);

  // Seletores (dados derivados) públicos
  public readonly items = this.cartItems.asReadonly();
  public readonly totalItems = computed(() => 
    this.cartItems().reduce((total, item) => total + item.quantity, 0)
  );

  // Ações públicas
  addSneaker(sneaker: Sneaker): void {
    this.cartItems.update(items => {
      const itemInCart = items.find(item => item.sneaker.id === sneaker.id);
      if (itemInCart) {
        return items.map(item =>
          item.sneaker.id === sneaker.id
            ? { ...item, quantity: item.quantity + 1 }
            : item
        );
      }
      return [...items, { sneaker, quantity: 1 }];
    });
  }
}
```

---

### `src/app/sneaker-card/sneaker-card.component.ts` (Solução Ouro)

O `SneakerCard` agora fala diretamente com a store, sem precisar de `@Output`.

```typescript
import { Component, Input, inject } from '@angular/core';
import { Sneaker } from '../interfaces/sneaker.interface';
import { CartService } from '../store/cart.service';

@Component({
  selector: 'app-sneaker-card',
  templateUrl: './sneaker-card.component.html'
})
export class SneakerCardComponent {
  @Input() sneaker!: Sneaker;

  private cartService = inject(CartService);

  onAddToCartClick(): void {
    this.cartService.addSneaker(this.sneaker);
  }
}
```

---

### `src/app/header/header.component.ts` e `header.component.html` (Solução Prata)

O `HeaderComponent` lê o estado diretamente da store.

**`header.component.ts`**
```typescript
import { Component, inject } from '@angular/core';
import { CartService } from '../store/cart.service';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html'
})
export class HeaderComponent {
  public readonly cartService = inject(CartService);
}
```

**`header.component.html`**
```html
<header class="main-header">
  <h1>Sneaker Store</h1>
  <div class="cart-status">
    🛒 Carrinho: {{ cartService.totalItems() }}
  </div>
</header>
```