# Lição 4.2: Diretivas de Atributo: `[ngClass]` e `[ngStyle]`

!Guarda-Roupa

Enquanto as diretivas estruturais (`*ngIf`, `*ngFor`) alteram a estrutura do DOM (adicionando ou removendo elementos), as **Diretivas de Atributo** alteram a aparência ou o comportamento de um elemento já existente.

**Analogia:** Se as diretivas estruturais são a equipa de cenografia que monta ou desmonta um cenário, as diretivas de atributo são a equipa de **guarda-roupa e maquilhagem**. Elas não removem o ator de cena, mas mudam a sua roupa (`[ngClass]`) ou a sua maquilhagem (`[ngStyle]`) com base no guião.

Repara que estas diretivas usam a sintaxe de *Property Binding* (`[]`), pois estão a ligar uma propriedade da nossa classe a um atributo do elemento.

## 1. `[ngClass]`: Mudar a Roupa

A diretiva `[ngClass]` permite-nos adicionar ou remover classes CSS de um elemento dinamicamente. É uma das diretivas mais úteis para criar UIs reativas.

A forma mais comum de a usar é passando um objeto onde:
-   As **chaves** são os nomes das classes CSS.
-   Os **valores** são expressões booleanas. Se a expressão for `true`, a classe é aplicada. Se for `false`, é removida.

### Exemplo: Alerta de Mensagem

Vamos criar uma caixa de mensagem que muda de cor dependendo se é uma mensagem de sucesso ou de erro.

**`meu-componente.component.scss`**
```scss
.alerta {
  padding: 15px;
  border-radius: 5px;
  border: 1px solid;
}
.alerta-sucesso {
  background-color: #d4edda;
  border-color: #c3e6cb;
  color: #155724;
}
.alerta-erro {
  background-color: #f8d7da;
  border-color: #f5c6cb;
  color: #721c24;
}
```

**`meu-componente.component.ts`**
```typescript
export class MeuComponenteComponent {
  tipoMensagem = 'sucesso'; // Pode ser 'sucesso' ou 'erro'
}
```

**`meu-componente.component.html`**
```html
<!-- A classe 'alerta' está sempre presente.
     A classe 'alerta-sucesso' só é aplicada se tipoMensagem for 'sucesso'.
     A classe 'alerta-erro' só é aplicada se tipoMensagem for 'erro'. -->
<div class="alerta" [ngClass]="{
  'alerta-sucesso': tipoMensagem === 'sucesso',
  'alerta-erro': tipoMensagem === 'erro'
}">
  Esta é uma mensagem de exemplo.
</div>
```

## 2. `[ngStyle]`: Mudar a Maquilhagem

A diretiva `[ngStyle]` permite-nos aplicar estilos CSS *inline* a um elemento de forma dinâmica.

### Exemplo: Barra de Progresso

**`meu-componente.component.ts`**
```typescript
export class MeuComponenteComponent {
  progresso = 75; // um valor de 0 a 100
}
```

**`meu-componente.component.html`**
```html
<div class="barra-progresso-container">
  <!-- A largura da barra interior é controlada pela propriedade 'progresso' -->
  <div class="barra-progresso" [ngStyle]="{ 'width': progresso + '%' }">
    {{ progresso }}%
  </div>
</div>
```

### `[ngClass]` vs. `[ngStyle]`

Regra geral, deves **preferir `[ngClass]` em vez de `[ngStyle]`**. Manter os teus estilos nos ficheiros `.scss` (associados a classes) é uma prática muito melhor do que aplicar estilos *inline*. Usa o `[ngStyle]` apenas para casos muito específicos onde o estilo é verdadeiramente dinâmico e calculado, como no exemplo da barra de progresso.

Na próxima lição, vamos aprender a formatar os dados que mostramos na UI de forma limpa e reutilizável com os **Pipes**.