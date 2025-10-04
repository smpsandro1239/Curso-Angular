# Lição 5.2: Criar o Primeiro Serviço

![Serviço de GPS](https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExbWw5a2N5c3JqY254b250b294c251b294c251b294c251b294/3oKIPEqDecIy5P0o4U/giphy.gif)

Já identificámos o problema: a nossa lógica de tarefas está "presa" dentro do `TodoListComponent`. Vamos agora criar o nosso "Serviço de GPS", uma classe centralizada para gerir as tarefas.

**Analogia:** Vamos construir a central do nosso "Serviço de GPS". Esta central vai ter a base de dados de mapas (o nosso array de tarefas) e as funções para interagir com ela (os nossos métodos).

## Passo 1: Gerar o Serviço com o Angular CLI

Tal como para os componentes, o Angular CLI tem um comando para gerar serviços. Isto garante que seguimos as melhores práticas e que o ficheiro é criado no sítio certo.

1.  Abre o terminal na raiz do teu projeto (`minha-primeira-app`).
2.  Corre o seguinte comando. Vamos criar o nosso serviço dentro de uma pasta `services` para manter tudo organizado.

    ```sh
    ng generate service services/task
    ```
    (Abreviatura: `ng g s services/task`)

O CLI vai criar dois ficheiros: `src/app/services/task.service.ts` e `src/app/services/task.service.spec.ts` (um ficheiro de teste que ignoraremos por agora).

## Passo 2: Anatomia de um Serviço

Abre o ficheiro `src/app/services/task.service.ts`. O código é muito simples:

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class TaskService {

  constructor() { }
}
```

As partes importantes são:

-   **`@Injectable()`**: Este *decorator* marca a classe como um serviço que pode ser "injetado" noutros sítios (componentes, outros serviços, etc.).
-   **`providedIn: 'root'`**: Esta é a configuração mais importante. Significa que o Angular vai criar **uma única instância** (um *singleton*) deste serviço para toda a a nossa aplicação. Todos os componentes que pedirem o `TaskService` vão receber exatamente o mesmo objeto. É isto que nos permite partilhar dados!

## Passo 3: Mover a Lógica para o Serviço

Agora, vamos fazer a refatoração. Vamos tirar a lógica e os dados do `TodoListComponent` e passá-los para o `TaskService`.

**`src/app/services/task.service.ts` (Atualizado)**
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class TaskService {
  // O array de tarefas agora vive aqui, no serviço.
  // É privado para que ninguém de fora o possa modificar diretamente.
  private tarefas = [
    { id: 1, descricao: 'Aprender sobre serviços', concluida: true, dataCriacao: new Date() },
    { id: 2, descricao: 'Praticar Injeção de Dependências', concluida: false, dataCriacao: new Date() }
  ];

  constructor() { }

  // Método público para devolver as tarefas
  getTarefas() {
    return this.tarefas;
  }

  // Método público para adicionar uma tarefa
  adicionarTarefa(descricao: string) {
    const novaTarefa = {
      id: this.tarefas.length + 1,
      descricao: descricao,
      concluida: false,
      dataCriacao: new Date()
    };
    this.tarefas.push(novaTarefa);
  }

  // Método público para alterar o estado de uma tarefa
  toggleConcluida(tarefaId: number) {
    const tarefa = this.tarefas.find(t => t.id === tarefaId);
    if (tarefa) {
      tarefa.concluida = !tarefa.concluida;
    }
  }
}
```

O nosso serviço é agora a **fonte única da verdade** para os dados das tarefas.

O nosso `TodoListComponent` ficou "órfão" da sua lógica. Ele agora precisa de uma forma de aceder a este serviço.

Na próxima lição, vamos ver o mecanismo mágico que o Angular usa para "entregar" uma instância do `TaskService` ao nosso componente: a **Injeção de Dependências**.