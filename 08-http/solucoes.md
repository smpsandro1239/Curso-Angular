# Módulo 8: Soluções dos Exercícios – Projeto Galeria de Sneakers

Aqui está a solução completa para o projeto da Galeria de Sneakers. O resultado é um componente que vai buscar dados a uma API de forma reativa e robusta.

---

### `app.module.ts` (Pré-requisito)

O `HttpClientModule` é essencial para que o serviço `HttpClient` esteja disponível.

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { HttpClientModule } from '@angular/common/http'; // Importado aqui!

import { AppComponent } from './app.component';
// ... outros imports

@NgModule({
  declarations: [
    AppComponent,
    // ...
  ],
  imports: [
    BrowserModule,
    HttpClientModule // E adicionado aqui!
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

---

### `src/app/services/sneaker.service.ts` (Solução Completa)

O serviço encapsula toda a lógica de comunicação com a API, incluindo a gestão de erros.

```typescript
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable, of } from 'rxjs';
import { catchError } from 'rxjs/operators';

export interface Sneaker {
  id: number;
  title: string;
  body: string;
}

@Injectable({
  providedIn: 'root'
})
export class SneakerService {
  private apiUrl = 'https://jsonplaceholder.typicode.com/posts';

  constructor(private http: HttpClient) { }

  getSneakers(): Observable<Sneaker[]> {
    return this.http.get<Sneaker[]>(this.apiUrl).pipe(
      catchError(this.handleError<Sneaker[]>('getSneakers', []))
    );
  }

  private handleError<T>(operation = 'operation', result?: T) {
    return (error: any): Observable<T> => {
      console.error(`${operation} falhou:`, error);
      // Retorna um resultado seguro para que a aplicação não parta.
      return of(result as T);
    };
  }
}
```

---

### `src/app/sneaker-gallery/sneaker-gallery.component.ts` (Solução Ouro)

A versão final do componente é muito limpa, delegando a gestão da subscrição ao `async` pipe.

```typescript
import { Component } from '@angular/core';
import { Observable } from 'rxjs';
import { Sneaker, SneakerService } from '../services/sneaker.service';

@Component({
  selector: 'app-sneaker-gallery',
  templateUrl: './sneaker-gallery.component.html',
  styleUrls: ['./sneaker-gallery.component.scss']
})
export class SneakerGalleryComponent {
  // A propriedade guarda o Observable diretamente. O $ é uma convenção.
  sneakers$: Observable<Sneaker[]>;

  constructor(private sneakerService: SneakerService) {
    this.sneakers$ = this.sneakerService.getSneakers();
  }
}
```

---

### `src/app/sneaker-gallery/sneaker-gallery.component.html` (Solução Ouro)

O template usa o `async` pipe para subscrever ao `sneakers$` e lida com os estados de carregamento e de lista vazia.

```html
<div class="gallery-container">
  <h2>A Minha Galeria de Sneakers</h2>

  <!-- O *ngIf com o 'async' pipe gere tudo: espera, mostra os dados, ou mostra o template de erro -->
  <div *ngIf="sneakers$ | async as sneakers; else loadingOrError">
    <div *ngIf="sneakers.length > 0; else noSneakers" class="sneaker-grid">
      <div *ngFor="let sneaker of sneakers" class="sneaker-card">
        <h3>{{ sneaker.title | slice:0:20 }}...</h3> <!-- Pipe para encurtar o texto -->
      </div>
    </div>
  </div>

  <!-- Template para quando está a carregar -->
  <ng-template #loadingOrError>
    <p>A carregar sneakers...</p>
    <!-- Poderíamos ter uma lógica mais complexa para diferenciar loading de erro -->
  </ng-template>

  <!-- Template para quando a lista vem vazia (ex: erro na API) -->
  <ng-template #noSneakers>
    <p>Não foram encontrados sneakers ou ocorreu um erro.</p>
  </ng-template>
</div>
```