# Lição 8.4: Gestão de Erros em Pedidos HTTP

!Erro

O que acontece se a API estiver em baixo? Ou se pedirmos um *sneaker* que não existe (um erro 404)? Uma aplicação robusta não pode simplesmente "partir". Ela tem de lidar com os erros de forma graciosa e, se possível, informar o utilizador.

**Analogia:** A tua encomenda da Glovo falhou (o restaurante fechou). A app não pode simplesmente crashar. Ela tem de te mostrar uma mensagem: "Lamentamos, o seu pedido não pôde ser processado".

O RxJS e o `HttpClient` dão-nos mecanismos poderosos para gerir erros.

## 1. O `error` Callback no `.subscribe()`

Como vimos na lição anterior, o objeto que passamos ao `.subscribe()` tem uma propriedade `error`. Este é o primeiro sítio onde podemos apanhar um erro.

**`sneaker-gallery.component.ts`**
```typescript
export class SneakerGalleryComponent implements OnInit {
  // ...
  errorMessage = '';

  ngOnInit(): void {
    this.sneakerService.getSneakers().subscribe({
      next: (data) => {
        this.sneakers = data;
        this.isLoading = false;
      },
      error: (err) => {
        console.error('FALHA no componente:', err);
        this.errorMessage = 'Não foi possível carregar os sneakers. Tenta mais tarde.';
        this.isLoading = false;
      }
    });
  }
  // ...
}
```

Isto funciona, mas a lógica de gestão de erros fica no componente. Muitas vezes, queremos que o serviço lide com o erro de uma forma mais centralizada.

## 2. O Operador `catchError` do RxJS

O RxJS tem um operador chamado `catchError` que nos permite intercetar um erro num *pipe* de um `Observable`.

**Analogia:** O `catchError` é o gerente do restaurante. Se a cozinha diz que não há um ingrediente, o gerente pode intercetar o problema e decidir o que fazer: ligar ao cliente a sugerir uma alternativa ou simplesmente cancelar o pedido.

Podemos usá-lo no nosso serviço para, por exemplo, fazer log do erro e/ou retornar um valor "seguro" (como um array vazio) para que a aplicação não parta.

**`src/app/services/sneaker.service.ts` (Atualizado)**
```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, of } from 'rxjs';
import { catchError } from 'rxjs/operators'; // 1. Importar o operador

// ... interface Sneaker ...

@Injectable({
  providedIn: 'root'
})
export class SneakerService {
  private apiUrl = 'https://jsonplaceholder.typicode.com/posts';

  constructor(private http: HttpClient) { }

  getSneakers(): Observable<Sneaker[]> {
    return this.http.get<Sneaker[]>(this.apiUrl).pipe(
      catchError(this.handleError<Sneaker[]>('getSneakers', [])) // 2. Usar o operador
    );
  }

  // 3. Criar um método privado para lidar com o erro
  private handleError<T>(operation = 'operation', result?: T) {
    return (error: any): Observable<T> => {
      console.error(`${operation} falhou: ${error.message}`); // Faz log do erro

      // Deixa a app continuar a correr retornando um resultado vazio.
      return of(result as T);
    };
  }
}
```

Com esta abordagem, o nosso componente nem sequer precisa de ter um `error` callback no `.subscribe()`, porque o serviço já tratou do erro e devolveu um array vazio, prevenindo que a aplicação quebre.

---

**Módulo 8 Concluído!** 🏆

Agora sabes como ligar a tua aplicação a uma fonte de dados externa. Dominas o `HttpClient` para fazer pedidos, entendes o que são `Observables` e como os consumir com `.subscribe()` ou com o `async` pipe, e sabes como gerir erros de forma robusta.

Estás pronto para o projeto do módulo: a **Galeria de Sneakers**.