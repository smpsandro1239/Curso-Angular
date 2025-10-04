# Lição 10.3: Criar um "Store" Reativo com Signals

!Cérebro Central

Agora que entendemos as primitivas dos Signals, vamos usá-las para construir o nosso `CartStoreService`. Este serviço será a nossa **fonte única da verdade** para tudo o que diz respeito ao carrinho de compras.

**Analogia:** Pensa neste serviço como o sistema de inventário central de uma loja. As caixas de pagamento (os nossos componentes) não guardam o seu próprio stock. Elas consultam o sistema central para saber o preço e a disponibilidade (`computed`) e informam o sistema sempre que um item é vendido (`método de ação`).

## Passo 1: Criar o Serviço

Primeiro, vamos gerar o nosso serviço de store.

```sh
ng g s store/cart
```

Isto irá criar o ficheiro `src/app/store/cart.service.ts`.

## Passo 2: Definir o Estado e os Dados Derivados

Dentro do serviço, vamos usar `signal` para o nosso estado base (a lista de itens) e `computed` para os dados que são calculados a partir desse estado.

**`src/app/store/cart.service.ts`**
```typescript
import { Injectable, signal, computed } from '@angular/core';
import { Sneaker } from '../interfaces/sneaker.interface'; // Assumindo que temos esta interface

// Podemos definir a forma de um item no carrinho
export interface CartItem {
  sneaker: Sneaker;
  quantity: number;
}

@Injectable({
  providedIn: 'root'
})
export class CartService {
  // 1. ESTADO (STATE): A fonte da verdade. É um signal privado.
  private cartItems = signal<CartItem[]>([]);

  // 2. DADOS DERIVADOS (SELECTORS): Signals públicos e computados.
  //    Eles reagem a mudanças no estado.
  
  // Expomos o estado de forma segura, como um signal de apenas leitura (readonly).
  public readonly items = this.cartItems.asReadonly();

  // Calcula o número total de itens no carrinho.
  public readonly totalItems = computed(() => {
    return this.cartItems().reduce((total, item) => total + item.quantity, 0);
  });

  // Calcula o preço total (vamos assumir um preço fixo por agora).
  public readonly totalPrice = computed(() => {
    const pricePerSneaker = 120; // Exemplo
    return this.totalItems() * pricePerSneaker;
  });

  constructor() { }

  // 3. AÇÕES (ACTIONS): Métodos públicos para modificar o estado.

  /**
   * Adiciona um sneaker ao carrinho. Se já existir, aumenta a quantidade.
   */
  addSneaker(sneaker: Sneaker): void {
    this.cartItems.update(items => {
      const itemInCart = items.find(item => item.sneaker.id === sneaker.id);

      if (itemInCart) {
        // Se o item já existe, cria um novo array onde esse item tem a quantidade atualizada.
        return items.map(item => 
          item.sneaker.id === sneaker.id 
            ? { ...item, quantity: item.quantity + 1 } 
            : item
        );
      } else {
        // Se não existe, adiciona o novo item ao array.
        return [...items, { sneaker, quantity: 1 }];
      }
    });
  }

  /**
   * Remove um sneaker do carrinho.
   */
  removeSneaker(sneakerId: number): void {
    this.cartItems.update(items => items.filter(item => item.sneaker.id !== sneakerId));
  }
}
```

## A Arquitetura da Store

Repara no padrão que criámos:
1.  **Estado Privado (`private cartItems`)**: A fonte da verdade é um `signal` privado. Nenhum componente pode modificá-lo diretamente.
2.  **Seletores Públicos (`public readonly totalItems`)**: Expomos os dados através de `signals` públicos, de preferência `computed` e `readonly`. Os componentes podem "ler" estes valores, e eles irão atualizar-se magicamente.
3.  **Ações Públicas (`addSneaker`)**: A única forma de alterar o estado é através de métodos públicos bem definidos. Isto torna o fluxo de dados previsível e fácil de depurar.

Com este serviço, qualquer componente na nossa aplicação pode agora injetar o `CartService`, ler o `totalItems()` e chamar o `addSneaker()`, e a UI de todos os outros componentes que dependem do carrinho será atualizada automaticamente.

Na próxima lição, vamos ligar este serviço aos nossos componentes e ver a magia acontecer.