# Lição 9.3: Comunicação Filho-Pai: `@Output()`

!Notificação

O nosso `SneakerCardComponent` já recebe dados do pai através de `@Input()`. Mas e se o utilizador clicar no botão "Comprar" dentro do cartão? O cartão (o filho) não deve ser responsável pela lógica do carrinho de compras. Essa responsabilidade é do componente-pai (a galeria ou a página da loja).

O filho precisa de uma forma de "gritar" para o pai: "Ei, alguém quer comprar este sneaker!". Para isso, usamos o decorator `@Output()`.

**Analogia:** Pensa num botão de chamada numa mesa de restaurante. O cliente (o filho) carrega no botão. Ele não vai ele próprio à cozinha buscar a bebida. O botão acende uma luz no balcão do empregado (o pai). O empregado vê a luz, vai à mesa e trata do pedido. O `@Output()` é esse botão de chamada.

## `@Output()` e `EventEmitter`

Para criar um evento personalizado, usamos duas ferramentas:
-   **`@Output()`**: Um decorator que marca uma propriedade como uma "porta de saída" de eventos.
-   **`EventEmitter`**: Uma classe do Angular que usamos para criar e emitir os nossos próprios eventos.

## Passo 1: Emitir o Evento no Componente Filho

Vamos fazer com que o nosso `SneakerCardComponent` emita um evento `adicionarAoCarrinho` quando o botão é clicado.

**`src/app/sneaker-card/sneaker-card.component.ts` (Refatorado)**
```typescript
import { Component, Input, Output, EventEmitter } from '@angular/core'; // 1. Importar Output e EventEmitter
import { Sneaker } from './sneaker.interface'; // Supondo que a interface está noutro ficheiro

@Component({
  selector: 'app-sneaker-card',
  templateUrl: './sneaker-card.component.html',
  styleUrls: ['./sneaker-card.component.scss']
})
export class SneakerCardComponent {
  @Input() sneaker!: Sneaker;

  // 2. Criar a propriedade de output. É um novo EventEmitter.
  // O <Sneaker> diz que este evento vai emitir um objeto do tipo Sneaker.
  @Output() adicionarAoCarrinho = new EventEmitter<Sneaker>();

  // 3. Criar um método para ser chamado no clique
  onComprarClick(): void {
    console.log('Botão "Comprar" clicado no filho!');
    // 4. Emitir o evento, passando o sneaker como "carga" (payload)
    this.adicionarAoCarrinho.emit(this.sneaker);
  }
}
```

Agora, atualizamos o template do filho para usar este novo método.

**`src/app/sneaker-card/sneaker-card.component.html` (Refatorado)**
```html
<div class="card" *ngIf="sneaker">
  <h2>{{ sneaker.nome }}</h2>
  <img [src]="sneaker.urlImagem" alt="Foto de {{ sneaker.nome }}">
  <!-- Ligamos o evento (click) ao nosso método -->
  <button (click)="onComprarClick()" [disabled]="!sneaker.emStock">Comprar</button>
</div>
```

## Passo 2: Ouvir o Evento no Componente Pai

O nosso filho agora "grita" um evento `adicionarAoCarrinho`. O pai precisa de "ouvir" esse grito. Fazemos isto usando a sintaxe de *Event Binding* `()` no template do pai.

**`src/app/sneaker-gallery/sneaker-gallery.component.html` (Atualizado)**
```html
<div class="sneaker-grid">
  <app-sneaker-card 
    *ngFor="let sneaker of sneakers"
    [sneaker]="sneaker"
    (adicionarAoCarrinho)="handleAdicionarAoCarrinho($event)">
    <!-- O nome do evento (adicionarAoCarrinho) tem de ser o mesmo da propriedade @Output.
         O $event vai conter os dados que foram emitidos (o objeto sneaker). -->
  </app-sneaker-card>
</div>

<div *ngIf="carrinho.length > 0">
  <h3>Carrinho: {{ carrinho.length }} itens</h3>
</div>
```

Finalmente, criamos o método no pai para lidar com o evento.

**`src/app/sneaker-gallery/sneaker-gallery.component.ts` (Atualizado)**
```typescript
export class SneakerGalleryComponent {
  // ...
  carrinho: Sneaker[] = [];

  handleAdicionarAoCarrinho(sneaker: Sneaker) {
    console.log('Pai recebeu o evento! Adicionando ao carrinho:', sneaker.nome);
    this.carrinho.push(sneaker);
  }
}
```

Com `@Input()` e `@Output()`, temos um fluxo de dados claro e unidirecional: os dados descem via `@Input`, e os eventos sobem via `@Output`. Este padrão é fundamental para construir aplicações Angular robustas e fáceis de manter.

Na próxima lição, vamos ver uma forma ainda mais flexível de compor componentes com `<ng-content>`.