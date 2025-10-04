# Lição 3.3: Two-Way Binding: A Sincronização Perfeita com `ngModel`

!Setas nos dois sentidos

Na lição anterior, vimos como ler o valor de um campo de input:
1.  Usámos *Property Binding* `[value]` para definir o valor inicial (opcional).
2.  Usámos *Event Binding* `(input)` para chamar um método que atualiza a propriedade na nossa classe.

Este padrão é tão comum que o Angular nos dá um atalho para fazer as duas coisas ao mesmo tempo: o **Two-Way Data Binding**.

**Analogia:** Se o *One-Way Binding* é um altifalante (lógica -> UI) e o *Event Binding* é um microfone (UI -> lógica), o *Two-Way Binding* é um **walkie-talkie**. Podes falar e ouvir ao mesmo tempo, mantendo a comunicação perfeitamente sincronizada.

## A Sintaxe "Banana in a Box" `[( )]`

O *Two-Way Binding* combina a sintaxe do *Property Binding* (`[]`) e do *Event Binding* (`()`), criando uma nova sintaxe: `[( )]`.

A mnemónica popular para se lembrar disto é "banana in a box" (uma banana dentro de uma caixa).

A diretiva que nos permite fazer isto em formulários é a `ngModel`.

` [(ngModel)]="propriedade" `

Isto é um atalho para:

` [ngModel]="propriedade" (ngModelChange)="propriedade = $event" `

## Passo 1: Importar o `FormsModule`

A diretiva `ngModel` não faz parte do núcleo do Angular; ela vive num módulo separado chamado `FormsModule`. Para a podermos usar, temos de importar este módulo no nosso `app.module.ts`.

**Este é um passo crucial e muitas vezes esquecido!**

**`src/app/app.module.ts`**
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms'; // 1. Importar o FormsModule

// ... outros imports

@NgModule({
  declarations: [
    // ...
  ],
  imports: [
    BrowserModule,
    FormsModule // 2. Adicionar aos imports
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Passo 2: Usar o `ngModel`

Agora que o nosso módulo "conhece" o `ngModel`, podemos usá-lo para simplificar o nosso exemplo de input da lição anterior.

**`meu-componente.component.ts`**
```typescript
export class MeuComponenteComponent {
  // A propriedade que queremos sincronizar
  nomeUtilizador = 'Sandro';
}
```

**`meu-componente.component.html`**
```html
<!-- Ligamos o valor do input diretamente à propriedade 'nomeUtilizador' -->
<input type="text" [(ngModel)]="nomeUtilizador" placeholder="Escreve o teu nome...">

<!-- A interpolação vai-se atualizar automaticamente! -->
<h3>Olá, {{ nomeUtilizador }}!</h3>
```

Com apenas uma linha, `[(ngModel)]="nomeUtilizador"`, conseguimos uma sincronização perfeita. Se a propriedade `nomeUtilizador` mudar na classe, o input atualiza-se. Se o utilizador escrever no input, a propriedade `nomeUtilizador` na classe atualiza-se.

---

**Módulo 3 Concluído!** 🏆

Agora dominas as quatro formas de *Data Binding* em Angular, a base de toda a interatividade. No próximo módulo, vamos explorar as **Diretivas** e os **Pipes**, ferramentas que nos dão ainda mais controlo sobre a aparência e o comportamento do nosso HTML.