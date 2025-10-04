# Lição 10.2: Introdução aos Signals: `signal()`, `computed()` e `effect()`

!Folha de Cálculo

Os Signals são a base do novo modelo de reatividade do Angular. Eles permitem-nos criar valores reativos de uma forma muito mais simples e explícita do que com RxJS/Observables.

**Analogia:** Pensa numa folha de cálculo.
-   Uma célula onde escreves um número (ex: `A1 = 10`) é um **`signal()`**.
-   Uma célula com uma fórmula (ex: `B1 = A1 * 2`) é um **`computed()`**. Se mudares `A1`, `B1` atualiza-se automaticamente.
-   Uma notificação que te avisa sempre que o valor em `B1` muda é um **`effect()`**.

Existem três primitivas principais que precisamos de dominar.

## 1. `signal(initialValue)`: A Fonte da Verdade

Um `signal` é um "invólucro" à volta de um valor que pode notificar os seus "consumidores" sempre que o valor muda.

Para criar um, importamos `signal` de `@angular/core`.

```typescript
import { signal } from '@angular/core';

export class MeuComponente {
  // Cria um signal com o valor inicial 0.
  // 'contador' não é o número 0, é um objeto Signal que contém o número 0.
  contador = signal(0);

  constructor() {
    // Para LER o valor, chamamos o signal como uma função.
    console.log(this.contador()); // 0

    // Para ATUALIZAR o valor, usamos o método .set()
    this.contador.set(5);
    console.log(this.contador()); // 5

    // Para atualizar com base no valor anterior, usamos .update()
    this.contador.update(valorAtual => valorAtual + 1);
    console.log(this.contador()); // 6
  }
}
```

## 2. `computed(computationFn)`: Valores Derivados

Um `computed` signal é um signal cujo valor é calculado a partir de outros signals. Ele é reativo: se um dos signals de que ele depende mudar, o seu valor é recalculado automaticamente.

Eles são *lazy* (preguiçosos) e *memoized* (guardam o resultado em cache), o que os torna muito eficientes.

```typescript
import { signal, computed } from '@angular/core';

export class MeuComponente {
  quantidade = signal(2);
  precoUnitario = signal(10);

  // 'total' é um computed signal. Ele "ouve" as mudanças em 'quantidade' e 'precoUnitario'.
  total = computed(() => this.quantidade() * this.precoUnitario());

  constructor() {
    console.log(this.total()); // 20

    // Se mudarmos um dos signals base...
    this.quantidade.set(3);

    // ...o 'total' é recalculado automaticamente!
    console.log(this.total()); // 30
  }
}
```

## 3. `effect(effectFn)`: Efeitos Secundários (Side Effects)

Um `effect` é uma operação que corre sempre que um ou mais valores de signal mudam. Ele é perfeito para executar "efeitos secundários", como fazer logging, guardar dados no `localStorage`, ou manipular o DOM.

Um `effect` corre sempre pelo menos uma vez e depois regista todos os signals que foram lidos dentro dele. Sempre que um desses signals muda, o `effect` corre novamente.

```typescript
import { signal, effect } from '@angular/core';

export class MeuComponente {
  nomeUtilizador = signal('Sandro');

  constructor() {
    // Cria um efeito que corre sempre que 'nomeUtilizador' muda.
    effect(() => {
      console.log(`O nome do utilizador mudou para: ${this.nomeUtilizador()}`);
    });
    // Output inicial: "O nome do utilizador mudou para: Sandro"

    // Mais tarde, noutro sítio...
    setTimeout(() => {
      this.nomeUtilizador.set('Ana');
      // Output automático: "O nome do utilizador mudou para: Ana"
    }, 2000);
  }
}
```

Com estas três ferramentas (`signal`, `computed`, `effect`), temos tudo o que precisamos para construir sistemas reativos complexos de uma forma muito mais clara e direta.

Na próxima lição, vamos usar isto para construir o nosso `CartStoreService`, o "cérebro" central da nossa aplicação.