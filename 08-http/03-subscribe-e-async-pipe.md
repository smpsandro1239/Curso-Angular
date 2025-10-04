# Lição 8.3: `.subscribe()` e o `async` Pipe: Consumir os Dados

!Consumir Dados

Já sabemos que o `HttpClient` nos devolve um `Observable`. Para "desempacotar" os dados desse `Observable`, temos de **subscrever** a ele.

**Analogia:** Se o `Observable` é a tua série de TV no Netflix, o método `.subscribe()` é o ato de clicares no botão "Play". Só quando clicas é que os episódios (os dados) começam a ser transmitidos.

## 1. Subscrever com `.subscribe()` (Na Classe)

A forma mais comum de consumir um `Observable` é usando o método `.subscribe()` na classe do componente.

Vamos refatorar o nosso `SneakerService` e criar um `SneakerGalleryComponent` para mostrar os dados.

### `src/app/services/sneaker.service.ts` (Atualizado)
```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs'; // Importar Observable

export interface Sneaker {
  id: number;
  title: string;
  // Adicionar outras propriedades que a API possa ter
  body: string; // Exemplo de outra propriedade da API de teste
}

@Injectable({
  providedIn: 'root'
})
export class SneakerService {
  private apiUrl = 'https://jsonplaceholder.typicode.com/posts'; // Usar uma API de teste mais simples

  constructor(private http: HttpClient) { }

  getSneakers(): Observable<Sneaker[]> { // Tipamos o retorno como Observable de array de Sneaker
    return this.http.get<Sneaker[]>(this.apiUrl);
  }
}
```

### `src/app/sneaker-gallery/sneaker-gallery.component.ts` (Novo Componente)

1.  Gera o componente: `ng g c sneaker-gallery`
2.  Atualiza o ficheiro `.ts`:

```typescript
import { Component, OnInit } from '@angular/core';
import { SneakerService, Sneaker } from '../services/sneaker.service';

@Component({
  selector: 'app-sneaker-gallery',
  templateUrl: './sneaker-gallery.component.html',
  styleUrls: ['./sneaker-gallery.component.scss']
})
export class SneakerGalleryComponent implements OnInit {
  sneakers: Sneaker[] = [];
  isLoading = true; // Para mostrar um indicador de carregamento

  constructor(private sneakerService: SneakerService) { }

  ngOnInit(): void {
    this.sneakerService.getSneakers().subscribe({
      next: (data) => { // 'next' é chamado quando o Observable emite um valor
        this.sneakers = data;
        this.isLoading = false;
      },
      error: (err) => { // 'error' é chamado se o Observable falhar
        console.error('Erro ao buscar sneakers:', err);
        this.isLoading = false;
      },
      complete: () => { // 'complete' é chamado quando o Observable termina
        console.log('Busca de sneakers completa.');
      }
    });
  }
}
```

## 2. O `async` Pipe (No Template)

Subscrever e gerir `Observables` na classe pode ser um pouco verboso. O Angular oferece uma forma mais limpa de o fazer diretamente no template: o **`async` pipe**.

O `async` pipe subscreve automaticamente a um `Observable` e devolve o seu último valor emitido. Quando o componente é destruído, ele também faz o *unsubscribe* automaticamente, prevenindo *memory leaks*.

### `src/app/sneaker-gallery/sneaker-gallery.component.html` (Atualizado)
```html
<h2>A Minha Galeria de Sneakers</h2>

<div *ngIf="isLoading">A carregar sneakers...</div>

<!-- O 'async' pipe subscreve ao Observable e devolve os dados.
     Só renderiza a lista quando os dados chegam. -->
<div *ngIf="sneakerService.getSneakers() | async as sneakers">
  <div *ngFor="let sneaker of sneakers" class="sneaker-card">
    <h3>{{ sneaker.title }}</h3>
    <p>{{ sneaker.body }}</p>
  </div>
</div>
```

Repara que, para usar o `async` pipe, o `getSneakers()` do serviço tem de retornar o `Observable` diretamente.

O `async` pipe é a forma preferida de lidar com `Observables` no template, pois simplifica o código e gere o *unsubscribe* automaticamente.

Na próxima lição, vamos focar-nos na gestão de erros em pedidos HTTP, um aspeto crucial para qualquer aplicação robusta.