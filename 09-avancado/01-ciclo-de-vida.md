# Lição 9.1: O Ciclo de Vida de um Componente

!Ciclo de Vida

Um componente Angular não aparece simplesmente no ecrã. Ele passa por um **ciclo de vida**: ele é criado, renderizado, verificado por alterações, e finalmente, destruído. O Angular permite-nos "ligar-nos" a estes momentos chave do ciclo de vida através de métodos especiais chamados **Lifecycle Hooks** (Ganchos de Ciclo de Vida).

**Analogia:** Pensa no ciclo de vida de um festivaleiro.
1.  **`constructor()`:** O momento em que decides ir ao festival. Ainda estás em casa a planear.
2.  **`ngOnInit()`:** Chegaste ao recinto! Montas a tenda, vês onde são as casas de banho. É a tua configuração inicial no local.
3.  **`ngOnChanges()`:** O cartaz muda. Uma banda que ias ver foi trocada por outra. Tens de reagir a esta mudança externa.
4.  **`ngOnDestroy()`:** O festival acabou. Desmontas a tenda, arrumas o lixo e vais-te embora, libertando o espaço.

## Os Hooks Mais Importantes

Existem 8 hooks, mas estes 4 são os que vais usar 99% do tempo.

### 1. `constructor()`

Embora não seja tecnicamente um hook do Angular, é o primeiro método a ser chamado. A sua única responsabilidade deve ser a **injeção de dependências**. Não deves colocar lógica complexa aqui, porque o componente ainda não está totalmente "ligado" à UI.

### 2. `ngOnInit()`

Este hook é chamado **uma vez**, depois de o componente ser inicializado e depois de as suas propriedades de `@Input` (que veremos a seguir) terem sido recebidas.

**Quando usar?** É o local perfeito para:
-   Ir buscar dados iniciais a um serviço.
-   Fazer subscrições a `Observables`.
-   Qualquer lógica de inicialização complexa.

Para o usar, a nossa classe de componente tem de `implements OnInit`.

```typescript
import { Component, OnInit } from '@angular/core';

@Component(...)
export class MeuComponente implements OnInit {

  constructor() {
    console.log('Constructor: A planear ir ao festival.');
  }

  ngOnInit(): void {
    console.log('ngOnInit: Cheguei ao recinto, a montar a tenda!');
  }
}
```

### 3. `ngOnChanges()`

Este hook é chamado antes do `ngOnInit` e sempre que uma das propriedades de `@Input` do componente muda. Ele dá-nos informação sobre o valor anterior e o valor atual da propriedade, permitindo-nos reagir a mudanças que vêm do componente-pai.

**Quando usar?** Quando precisas de executar uma lógica específica sempre que um dado vindo de fora muda.

### 4. `ngOnDestroy()`

Este hook é chamado **uma vez**, imediatamente antes de o componente ser destruído e removido do DOM (por exemplo, quando navegamos para outra página).

**Quando usar?** É o local **CRUCIAL** para fazer a "limpeza":
-   Fazer *unsubscribe* de `Observables` para evitar *memory leaks*.
-   Remover *event listeners* manuais.
-   Cancelar `timers`.

```typescript
import { Component, OnDestroy } from '@angular/core';

@Component(...)
export class MeuComponente implements OnDestroy {
  // ...
  ngOnDestroy(): void {
    console.log('ngOnDestroy: O festival acabou, a arrumar as coisas.');
    // this.minhaSubscricao.unsubscribe();
  }
}
```

Entender estes hooks é fundamental para criar componentes que se comportam de forma previsível e eficiente. Nas próximas lições, vamos ver como os `@Input`s disparam o `ngOnChanges` e como os `@Output`s nos ajudam a comunicar com o mundo exterior.