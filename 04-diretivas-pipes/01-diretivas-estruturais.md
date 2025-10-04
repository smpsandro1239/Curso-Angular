# Lição 4.1: Diretivas Estruturais: `*ngIf` e `*ngFor`

!Estrutura

As **Diretivas Estruturais** são um dos superpoderes do Angular. Elas permitem-nos alterar a estrutura do DOM, adicionando, removendo ou manipulando elementos. Reconheces uma diretiva estrutural pelo asterisco `*` que vem antes do seu nome.

**Analogia:** Pensa que és o realizador de um filme. As diretivas estruturais são as tuas ordens para a equipa de cenografia. `*ngIf` é a ordem "só monta este cenário se a cena for à noite". `*ngFor` é a ordem "coloca 100 figurantes com esta farda em cena".

Vamos ver as duas mais importantes.

## 1. `*ngIf`: Renderização Condicional

A diretiva `*ngIf` adiciona ou remove um elemento do DOM com base numa expressão booleana. Se a expressão for `true`, o elemento é adicionado. Se for `false`, o elemento é removido.

### Exemplo: Mensagem de Boas-Vindas

Vamos mostrar uma mensagem diferente dependendo se o utilizador fez login ou não.

**`meu-componente.component.ts`**
```typescript
export class MeuComponenteComponent {
  utilizadorLogado = true;
  nomeUtilizador = 'Sandro';
}
```

**`meu-componente.component.html`**
```html
<!-- Este h2 só é adicionado ao DOM se 'utilizadorLogado' for true -->
<h2 *ngIf="utilizadorLogado">Bem-vindo de volta, {{ nomeUtilizador }}!</h2>

<!-- Este h2 só é adicionado ao DOM se 'utilizadorLogado' for false -->
<h2 *ngIf="!utilizadorLogado">Por favor, faz login para continuar.</h2>
```

### `*ngIf` com `else`

O exemplo acima funciona, mas estamos a repetir a condição. Podemos usar um bloco `else` para um código mais limpo. Para isso, usamos uma tag especial do Angular, a `<ng-template>`.

```html
<!-- Se 'utilizadorLogado' for true, mostra este div. Senão (else), mostra o template 'blocoLogin'. -->
<div *ngIf="utilizadorLogado; else blocoLogin">
  <h2>Bem-vindo de volta, {{ nomeUtilizador }}!</h2>
  <button>Logout</button>
</div>

<!-- Este é o nosso bloco 'else'. Ele não é renderizado por defeito. -->
<ng-template #blocoLogin>
  <h2>Por favor, faz login para continuar.</h2>
  <button>Login</button>
</ng-template>
```

## 2. `*ngFor`: Renderizar Listas

A diretiva `*ngFor` é o `loop for` do nosso HTML. Ela repete um elemento para cada item de um array.

### Exemplo: Lista de Festivais

**`meu-componente.component.ts`**
```typescript
export class MeuComponenteComponent {
  festivais = [
    { nome: 'NOS Alive', local: 'Algés' },
    { nome: 'Super Bock Super Rock', local: 'Meco' },
    { nome: 'Paredes de Coura', local: 'Paredes de Coura' }
  ];
}
```

**`meu-componente.component.html`**
```html
<h3>Próximos Festivais:</h3>
<ul>
  <!-- A tag <li> será repetida para cada 'festival' no array 'festivais'.
       A variável 'festival' fica disponível dentro da tag. -->
  <li *ngFor="let festival of festivais">
    <strong>{{ festival.nome }}</strong> - {{ festival.local }}
  </li>
</ul>
```

### Apanhar o Índice

Podes também apanhar o índice de cada item na iteração, o que é útil para listas numeradas.

```html
<li *ngFor="let festival of festivais; let i = index">
  {{ i + 1 }}. {{ festival.nome }}
</li>
```

Na próxima lição, vamos ver as **Diretivas de Atributo**, que não alteram a estrutura do DOM, mas sim a aparência ou o comportamento dos elementos.