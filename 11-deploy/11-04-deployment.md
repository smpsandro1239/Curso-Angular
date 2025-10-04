# Lição 11.4: Deployment: A Tua App no Mundo!

!Foguetão

Temos a nossa pasta `dist` com a aplicação otimizada e pronta. E agora? Como é que a pomos num URL público como `www.aminhaloja.com`? A este processo chamamos **deployment** (publicação).

Felizmente, hoje em dia existem serviços fantásticos que tornam este processo incrivelmente simples, muitas vezes gratuito para projetos pessoais. Vamos usar um dos melhores: a **Vercel**.

**Analogia:** O teu carro de corrida está pronto e polido na garagem (`dist`). A Vercel é a transportadora de classe mundial que pega no teu carro, coloca-o num avião e entrega-o em minutos no circuito do Mónaco (a internet), pronto para a corrida. E o melhor de tudo? Ela fica a vigiar a tua fábrica e, sempre que produzes uma versão nova e melhorada do carro, ela vai buscá-la e troca-a no circuito, automaticamente.

## Porquê a Vercel?

-   **Integração com Git:** Liga-se diretamente ao teu repositório no GitHub.
-   **CI/CD Automático:** Sempre que fazes `git push` para o teu ramo principal, a Vercel faz um novo build e deploy automaticamente.
-   **Gratuito:** O plano "Hobby" é perfeito e gratuito para projetos pessoais e portfólio.
-   **Performance:** A tua aplicação é distribuída por uma rede global (CDN), o que a torna super rápida em qualquer parte do mundo.

## Passo 1: Pré-requisitos

1.  O teu projeto Angular tem de estar num repositório no **GitHub**.
2.  Precisas de uma conta na Vercel. Vai a vercel.com e clica em "Sign Up", escolhendo a opção **"Continue with GitHub"**.

## Passo 2: Fazer o Deploy

O processo é tão simples que parece magia.

1.  **Importar o Projeto:** No teu dashboard da Vercel, clica em **"Add New... > Project"**.
2.  **Encontrar o Repositório:** A Vercel vai mostrar os teus repositórios do GitHub. Encontra o da tua "Sneaker Store" e clica em **"Import"**.
3.  **Configurar e Fazer Deploy:**
    -   **Framework Preset:** A Vercel é inteligente e vai detetar que é um projeto **Angular**.
    -   **Build and Output Settings:** Ela já sabe que o comando de build é `ng build` e que a pasta de output é `dist/nome-do-projeto`. Normalmente, **não precisas de alterar nada!**
    -   Clica em **"Deploy"**.

É só isto.

A Vercel vai agora clonar o teu repositório, instalar as dependências, correr o `ng build` e publicar o resultado. Em cerca de um ou dois minutos, vais ver uma chuva de confetes e um link para a tua aplicação, ao vivo na internet!

## O Fluxo de Trabalho Mágico (CI/CD)

A partir de agora, o teu fluxo de trabalho é este:
1.  Fazes alterações ao teu código no teu computador.
2.  Fazes `git add .`, `git commit -m "..."` e `git push`.
3.  A Vercel deteta o `push`, faz um novo build e atualiza o site automaticamente.

Nunca mais precisas de fazer upload manual de ficheiros.

---

**Módulo 11 Concluído!** 🏆

Chegaste ao fim da jornada técnica principal! Agora sabes construir uma aplicação Angular completa, desde a ideia inicial até à publicação global. Dominas:

-   **Proteção de Rotas** com Route Guards.
-   **Otimização de Performance** com Lazy Loading.
-   **Build de Produção** para criar uma versão otimizada da tua app.
-   **Deployment Contínuo** com a Vercel.

Estás pronto para o projeto final do módulo e para construir aplicações Angular de nível profissional.