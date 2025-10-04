# Lição 8.1: O `HttpClient`: Fazer Pedidos a APIs

!Pedido na App

Chegou a hora de ligar a nossa aplicação ao mundo. Para ir buscar dados a um servidor (uma API), o Angular fornece um módulo otimizado e poderoso chamado `HttpClientModule`.

**Analogia:** O `HttpClient` é a funcionalidade na app da Glovo que te permite carregar no botão "Fazer Pedido". Ele sabe como construir o pedido, enviá-lo para o restaurante (o servidor) e receber uma resposta.

## Passo 1: Configurar o `HttpClientModule`

Tal como o `FormsModule` e o `ReactiveFormsModule`, o `HttpClientModule` precisa de ser importado no nosso módulo principal para que os seus serviços fiquem disponíveis em toda a aplicação.

**`src/app/app.module.ts`**
```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http'; // 1. Importar

// ... outros imports

@NgModule({
  declarations: [
    // ...
  ],
  imports: [
    BrowserModule,
    HttpClientModule // 2. Adicionar aos imports
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## Passo 2: Injetar o `HttpClient` num Serviço

A melhor prática não é usar o `HttpClient` diretamente num componente. Em vez disso, encapsulamos toda a lógica de comunicação com a API dentro de um serviço.

Vamos criar um serviço para ir buscar os nossos *sneakers*.

1.  Gera um novo serviço: `ng g s services/sneaker`
2.  No `sneaker.service.ts`, vamos injetar o `HttpClient`.

**`src/app/services/sneaker.service.ts`**
```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http'; // 1. Importar

@Injectable({
  providedIn: 'root'
})
export class SneakerService {

  // O URL da nossa API (pode ser uma API pública ou um json-server local)
  private apiUrl = 'https://my-json-server.typicode.com/typicode/demo/posts'; // URL de exemplo

  // 2. Injetar o HttpClient no constructor
  constructor(private http: HttpClient) { }

  // 3. Criar um método para ir buscar os dados
  getSneakers() {
    return this.http.get(this.apiUrl);
  }
}
```

## Passo 3: O que é que `http.get()` Retorna?

Repara que o nosso método `getSneakers()` retorna o resultado de `this.http.get(...)`.

Este método **não retorna os dados diretamente**. Ele retorna um tipo especial de objeto chamado **`Observable`**.

Um `Observable` é um "fluxo de dados" que pode emitir valores ao longo do tempo. No caso de um pedido HTTP, ele vai emitir um único valor (a resposta do servidor) e depois completar.

### Tipar os Nossos Pedidos

Para aproveitarmos o poder do TypeScript, podemos dizer ao `HttpClient` que tipo de dados esperamos receber. Isto dá-nos autocompletação e segurança de tipos.

```typescript
// Definimos uma interface para a forma de um Sneaker
export interface Sneaker {
  id: number;
  title: string; // A API de exemplo usa 'title' em vez de 'nome'
}

// Dizemos ao .get() que esperamos receber um array de Sneakers
getSneakers() {
  return this.http.get<Sneaker[]>(this.apiUrl);
}
```

Agora que já sabemos como fazer o pedido, na próxima lição vamos aprender o que é exatamente um `Observable` e como podemos "subscrever" a ele para finalmente recebermos os nossos dados.