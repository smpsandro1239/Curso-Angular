# Lição 4.3: Pipes: Transformar Dados na UI

!Filtro de Água

Muitas vezes, os dados que temos na nossa classe (`.ts`) não estão no formato exato que queremos mostrar na UI (`.html`). Por exemplo, podemos ter uma data completa com horas e segundos, mas só queremos mostrar o dia/mês/ano. Ou podemos ter um texto em minúsculas que queremos mostrar em maiúsculas.

Para resolver isto, o Angular dá-nos os **Pipes**. Um *pipe* pega num dado, transforma-o e devolve o resultado formatado para ser mostrado na UI. O dado original na classe do componente não é alterado.

**Analogia:** Um *pipe* é como um **filtro de imagem do Instagram**. A tua foto original (`dado`) continua guardada no telemóvel. O filtro (`pipe`) é aplicado apenas no momento em que vais publicar a *Story*, alterando a sua aparência (`dado transformado`).

## A Sintaxe do Pipe `|`

Usamos os *pipes* diretamente no nosso template HTML, dentro de uma expressão de interpolação `{{ }}`, usando o caractere `|` (que se assemelha a um cano, ou *pipe* em inglês).

`{{ dado | nomeDoPipe }}`

O Angular já vem com vários *pipes* incorporados muito úteis.

## Pipes Comuns

Vamos ver alguns exemplos.

**`meu-componente.component.ts`**
```typescript
export class MeuComponenteComponent {
  tituloDoCurso = 'angular do zero ao pro';
  dataDeLancamento = new Date(); // A data e hora atuais
  preco = 49.99;
  numeroDeAlunos = 12345;
}
```

**`meu-componente.component.html`**
```html
<!-- Pipes de Texto -->
<p>Título em maiúsculas: {{ tituloDoCurso | uppercase }}</p>
<p>Título em minúsculas: {{ tituloDoCurso | lowercase }}</p>
<p>Título capitalizado: {{ tituloDoCurso | titlecase }}</p>

<!-- Pipe de Data -->
<p>Data completa: {{ dataDeLancamento | date }}</p>

<!-- Pipe de Números -->
<p>Preço: {{ preco | number }}</p>
<p>Número de Alunos: {{ numeroDeAlunos | number }}</p>

<!-- Pipe de Moeda -->
<p>Preço formatado: {{ preco | currency }}</p>
```

## Passar Parâmetros a um Pipe

Alguns *pipes* aceitam parâmetros para personalizar a formatação. Usamos dois pontos `:` para separar o nome do *pipe* dos seus parâmetros.

### Exemplo com `date` e `currency`

```html
<!-- Formatar a data para o padrão português -->
<p>Data (PT): {{ dataDeLancamento | date:'dd/MM/yyyy' }}</p>

<!-- Formatar a moeda para Euros, mostrando o símbolo -->
<p>Preço (EUR): {{ preco | currency:'EUR':'symbol' }}</p>

<!-- Formatar um número para ter no mínimo 2 dígitos inteiros e no máximo 2 casas decimais -->
<p>Preço (Decimal): {{ preco | number:'2.0-2' }}</p>
```

## Encadeamento de Pipes (Chaining)

Podes aplicar múltiplos *pipes* a um mesmo dado. Eles serão aplicados por ordem, da esquerda para a direita.

```html
<!-- Primeiro formata a data, depois converte o resultado para maiúsculas -->
<p>Data em Maiúsculas: {{ dataDeLancamento | date:'fullDate' | uppercase }}</p>
```

---

**Módulo 4 Concluído!** 🏆

Agora tens o controlo total sobre a estrutura e a aparência da tua UI. Sabes mostrar ou esconder elementos com `*ngIf`, criar listas com `*ngFor`, aplicar estilos dinâmicos com `[ngClass]` e formatar os teus dados com `Pipes`.

Estás pronto para aplicar tudo isto no projeto do módulo: a **Lista de Tarefas (To-Do List)**.