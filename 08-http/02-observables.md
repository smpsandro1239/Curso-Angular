# Lição 8.2: Observables: O "Netflix" dos Dados

![Netflix](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExbWw5a2N5c3JqY254b250b294c251b294c251b294c251b294/l41lHw24p23v5TzSE/giphy.gif)

Na lição anterior, vimos que o método `this.http.get()` não nos devolve os dados diretamente, mas sim um `Observable`. O que é isto?

## O que é um Observable?

Um **`Observable`** é um conceito do **RxJS** (Reactive Extensions for JavaScript), uma biblioteca poderosa para programação reativa. Podes pensar num `Observable` como uma **fonte de dados que pode emitir múltiplos valores ao longo do tempo**.

**Analogia:** Pensa no **Netflix**.
-   Uma `Promise` é como um filme que descarregas. Uma vez que o download está completo, tens o filme todo. É um evento único.
-   Um `Observable` é como uma **série de TV**. Tu "subscreves" à série. Recebes um episódio hoje, outro amanhã, e assim por diante. É um fluxo contínuo de dados ao longo do tempo.

No contexto de pedidos HTTP, o `Observable` que o `HttpClient` retorna é um caso especial: ele emite **um único valor** (a resposta da API) e depois **completa**. Mas a sua natureza de "fluxo" é o que o torna tão poderoso para lidar com eventos, inputs do utilizador e outras fontes de dados assíncronas.

## Observable vs. Promise

| Característica        | Promise                               | Observable                                  |
| :-------------------- | :------------------------------------ | :------------------------------------------ |
| **Valores**           | Emite um único valor.                 | Pode emitir múltiplos valores ao longo do tempo. |
| **Cancelável**        | Não pode ser cancelada.               | Pode ser cancelado.                         |
| **Execução**          | Executa imediatamente.                | É *lazy* (só executa quando alguém subscreve). |
| **Gestão de Erros**   | `.catch()`                            | `.subscribe(..., errorCallback, ...)`       |
| **Integração Angular**| Menos integrada (usada com `async/await` em componentes). | Totalmente integrada (serviços, `async` pipe, etc.). |

## Porquê Observables no Angular?

O Angular usa `Observables` extensivamente por várias razões:

1.  **Consistência:** Permite lidar com todos os tipos de eventos e dados assíncronos (HTTP, eventos do DOM, timers) de uma forma unificada.
2.  **Poder:** O RxJS oferece centenas de operadores para transformar, filtrar e combinar fluxos de dados de formas incrivelmente poderosas.
3.  **Performance:** A natureza *lazy* dos `Observables` significa que o trabalho só é feito quando é realmente necessário, e podem ser cancelados, libertando recursos.

## Criar um Observable Simples

Podemos criar os nossos próprios `Observables` para entender melhor como funcionam.

```typescript
import { Observable } from 'rxjs';

const meuObservable = new Observable(observer => {
  // O código aqui só corre quando alguém subscreve
  console.log('Observable: Olá!');
  observer.next(1); // Emite o valor 1
  observer.next(2); // Emite o valor 2
  observer.next(3); // Emite o valor 3
  setTimeout(() => {
    observer.next(4); // Emite o valor 4 depois de 1 segundo
    observer.complete(); // Diz que o Observable terminou
  }, 1000);
});

// Se ninguém subscrever, nada acontece!
console.log('Antes de subscrever...');

// Para receber os valores, temos de subscrever
meuObservable.subscribe(valor => console.log(`Valor recebido: ${valor}`));

console.log('Depois de subscrever...');
```

Se correres este código, verás que "Observable: Olá!" só aparece depois de subscrevermos, e os valores são emitidos ao longo do tempo.

Na próxima lição, vamos ver como "subscrever" a um `Observable` do `HttpClient` para finalmente recebermos os dados da nossa API, e como usar o `async` pipe para o fazer de forma ainda mais elegante no template.