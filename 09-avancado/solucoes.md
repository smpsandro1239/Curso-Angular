# Módulo 9: Soluções dos Exercícios – Refatorar o SneakerCard

Aqui está a solução completa para o projeto. O resultado é um conjunto de componentes que seguem as melhores práticas de comunicação e composição do Angular.

---

### Interface `sneaker.interface.ts` (Base)

É uma boa prática definir a "forma" dos nossos dados numa interface.

```typescript
// src/app/interfaces/sneaker.interface.ts
export interface Sneaker {
  id: number;
  nome: string;
  marca: string;
  imagemUrl: string;
}
```

---

### `ui/card.component.ts` e `ui/card.component.html` (Solução Ouro)

Este é o nosso componente de layout genérico, que usa projeção de conteúdo.

**`card.component.ts`**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-card',
  templateUrl: './card.component.html',
  styleUrls: ['./card.component.scss'] // Adicionar algum estilo base
})
export class CardComponent { }
```

**`card.component.html`**
```html
<div class="card-container">
  <div class="card-header" *ngIf="!!'card-header'">
    <ng-content select="[card-header]"></ng-content>
  </div>
  <div class="card-body">
    <ng-content select="[card-body]"></ng-content>
  </div>
  <div class="card-footer">
    <ng-content select="[card-footer]"></ng-content>
  </div>
</div>
```

---

### `sneaker-card.component.ts` e `sneaker-card.component.html` (Solução Ouro)

O `SneakerCard` agora é um componente de apresentação que recebe dados (`@Input`), emite eventos (`@Output`) e usa o componente de layout `app-card`.

**`sneaker-card.component.ts`**
```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core';
import { Sneaker } from '../interfaces/sneaker.interface';

@Component({
  selector: 'app-sneaker-card',
  templateUrl: './sneaker-card.component.html'
})
export class SneakerCardComponent {
  @Input() sneaker!: Sneaker;
  @Output() addToCart = new EventEmitter<Sneaker>();

  onAddToCartClick(): void {
    this.addToCart.emit(this.sneaker);
  }
}
```

**`sneaker-card.component.html`**
```html
<app-card>
  <h3 card-header>{{ sneaker.nome }}</h3>

  <div card-body class="sneaker-image-container">
    <img [src]="sneaker.imagemUrl" [alt]="sneaker.nome">
  </div>

  <div card-footer>
    <button (click)="onAddToCartClick()">Adicionar ao Carrinho</button>
  </div>
</app-card>
```

---

### `sneaker-gallery.component.ts` e `sneaker-gallery.component.html` (Componente Pai)

A galeria agora orquestra tudo, passando os dados para os filhos e ouvindo os seus eventos.

**`sneaker-gallery.component.html`**
```html
<h2>Galeria de Sneakers</h2>

<div class="carrinho-info">
  Itens no carrinho: {{ carrinho.length }}
</div>

<div class="gallery-grid">
  <app-sneaker-card
    *ngFor="let s of sneakers"
    [sneaker]="s"
    (addToCart)="handleAddToCart($event)">
  </app-sneaker-card>
</div>
```