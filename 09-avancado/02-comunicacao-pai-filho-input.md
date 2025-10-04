# Lição 9.2: Comunicação Pai-Filho: `@Input()`

!Instruções

Os nossos componentes ainda são muito independentes. O `SneakerCardComponent` que criámos tem os seus próprios dados "hardcoded". Isto torna-o inútil para mostrar uma galeria de *sneakers* diferentes.

Precisamos de uma forma de o componente-pai (a galeria) dizer a cada `SneakerCard` (o filho) qual o *sneaker* que ele deve mostrar. Para isto, usamos o decorator `@Input()`.

**Analogia:** Pensa no `SneakerCard` como uma peça de Lego "cartão de produto". O `@Input()` é a ranhura onde o manual de instruções (o componente-pai) encaixa a informação específica: "tu és o cartão para as Air Force 1", "tu és o cartão para as Dunk Low".

Uma propriedade marcada com `@Input()` é uma "porta de entrada" para dados vindos de fora.

## Passo 1: Definir o `@Input()` no Componente Filho

Vamos refatorar o nosso `SneakerCardComponent` para que ele receba os dados de um *sneaker* em vez de os ter definidos internamente.

**`src/app/sneaker-card/sneaker-card.component.ts` (Refatorado)**
```typescript
import { Component, Input } from '@angular/core'; // 1. Importar o Input

// Supondo que temos uma interface para o Sneaker
export interface Sneaker {
  nome: string;
  marca: string;
  urlImagem: string;
  emStock: boolean;
}

@Component({
  selector: 'app-sneaker-card',
  templateUrl: './sneaker-card.component.html',
  styleUrls: ['./sneaker-card.component.scss']
})
export class SneakerCardComponent {
  // 2. Marcar a propriedade 'sneaker' como uma porta de entrada de dados.
  // O '!' diz ao TypeScript: "confia em mim, esta propriedade vai ser inicializada pelo pai".
  @Input() sneaker!: Sneaker; 
}
```

Agora, o nosso componente já não tem dados próprios. Ele espera recebê-los. O seu template também precisa de ser atualizado para usar o objeto `sneaker`.

**`src/app/sneaker-card/sneaker-card.component.html` (Refatorado)**
```html
<!-- Usamos o 'sneaker' que veio do @Input -->
<div class="card" *ngIf="sneaker">
  <h2>{{ sneaker.nome }} ({{ sneaker.marca }})</h2>
  <img [src]="sneaker.urlImagem" alt="Foto de {{ sneaker.nome }}">
  <button [disabled]="!sneaker.emStock">Comprar</button>
</div>
```

## Passo 2: Passar os Dados no Componente Pai

Agora, no componente-pai (ex: `SneakerGalleryComponent`), usamos *Property Binding* (`[]`) para passar os dados para o `@Input` do filho.

**`src/app/sneaker-gallery/sneaker-gallery.component.html`**
```html
<div class="sneaker-grid">
  <!-- No nosso loop, para cada 'sneaker' do array... -->
  <app-sneaker-card 
    *ngFor="let sneaker of sneakers"
    [sneaker]="sneaker"> 
    <!-- ...passamos o objeto 'sneaker' inteiro para a propriedade 'sneaker' do filho. -->
  </app-sneaker-card>
</div>
```

O nome dentro dos parênteses retos `[sneaker]` tem de corresponder exatamente ao nome da propriedade marcada com `@Input()` no componente filho.

## `@Input()` e o `ngOnChanges`

Sempre que o componente-pai passa um novo valor para uma propriedade `@Input` (por exemplo, se o array de *sneakers* for reordenado), o *lifecycle hook* `ngOnChanges` do componente filho é chamado. Isto permite-nos reagir a mudanças nos dados que vêm de fora.

Com o `@Input()`, criámos um componente `SneakerCard` verdadeiramente reutilizável. Podemos usá-lo em qualquer sítio da nossa aplicação, bastando passar-lhe o objeto `sneaker` que queremos que ele mostre.

Na próxima lição, vamos aprender a fazer a comunicação inversa: como é que o filho pode "falar" com o pai, usando `@Output()`.