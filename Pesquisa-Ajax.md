# Pesquisa Comparativa: Assincronicidade no JavaScript
É fundamental entender que não estamos comparando quatro itens iguais, mas sim um ecossistema que evoluiu:

* **APIs de Requisição de Rede:** `XMLHttpRequest` (antiga) e `fetch` (moderna).
* **Mecanismos para Assincronicidade:** `Promises` (o objeto base) e `async/await` (a sintaxe moderna para usar Promises).

---

## Tabela Comparativa Resumida

| Característica | `XMLHttpRequest` (XHR) | `Promises` (Promessas) | `Fetch` API | `async / await` |
| :--- | :--- | :--- | :--- | :--- |
| **O que é?** | Uma API de rede (antiga) baseada em objeto. | Um objeto que representa um valor futuro. | Uma API de rede (moderna) baseada em função. | Uma sintaxe para lidar com código assíncrono. |
| **Paradigma** | Baseado em eventos e *callbacks*. | Encadeamento (`.then()`). | Baseada em `Promises` (usa o paradigma de `Promises`). | Linear, "síncrono-like". |
| **Como é usado?** | `new XMLHttpRequest()`, `.open()`, `.send()`, `.onload = ...` | `operacao().then(...).catch(...)` | `fetch(url).then(...).catch(...)` | `async function() { try { await ... } catch ... }` |
| **Tratamento de Erro** | Manipuladores de evento (`onerror`) e verificação de status (`xhr.status`). | Método `.catch()` no final da cadeia. | Método `.catch()` (mas **requer** verificação manual de `response.ok` para erros HTTP). | Bloco `try...catch` padrão do JavaScript. |
| **Legibilidade** | Baixa. Pode levar ao "Callback Hell" (Inferno de Callbacks). | Média. Resolve o "Callback Hell" através do encadeamento. | Média (com `.then()`). Muito mais limpa que XHR. | Muito Alta. É a forma mais fácil de ler e escrever código assíncrono. |
| **Relação entre eles** | Não usa `Promises` nativamente. | É a base que o `fetch` usa e que o `async/await` consome. | **Retorna** uma `Promise`. | **Consome** (aguarda) uma `Promise`. |

---

## Conceitos Detalhados e Usabilidade

### 1. XMLHttpRequest (XHR)

* **Conceito:** É uma API de navegador, disponível como um objeto (`new XMLHttpRequest()`), que permite fazer requisições HTTP para um servidor. Foi a tecnologia fundadora do **AJAX** (Asynchronous JavaScript and XML), permitindo que páginas web atualizassem partes de seu conteúdo sem a necessidade de um recarregamento completo. Seu funcionamento é assíncrono, mas gerenciado através de *callbacks* e escuta de eventos (como `onload`, `onerror`, `onreadystatechange`).

* **Usabilidade:** Hoje, o XHR é considerado **legado**. Sua sintaxe é verborrágica e complexa, e o gerenciamento de múltiplas requisições sequenciais leva ao infame "**Callback Hell**" (código aninhado profundamente, difícil de ler). Seu uso em projetos modernos não é recomendado, a menos que haja a necessidade extrema de suportar navegadores muito antigos (como o Internet Explorer 11, que não suporta `fetch`).

### 2. Promises (Promessas)

* **Conceito:** Uma `Promise` é um objeto do JavaScript (introduzido no ES6/2015) que representa a eventual conclusão (ou falha) de uma operação assíncrona. Ela funciona como um "recibo" para um valor que ainda não está disponível. Uma `Promise` possui três estados:
    1.  **`pending` (pendente):** Estado inicial, a operação ainda não terminou.
    2.  **`fulfilled` (realizada):** A operação foi concluída com sucesso (a promessa foi cumprida).
    3.  **`rejected` (rejeitada):** A operação falhou (a promessa foi quebrada).

* **Usabilidade:** `Promises` são a **base** do JavaScript assíncrono moderno. Elas foram criadas para resolver o "Callback Hell", permitindo que operações assíncronas sejam encadeadas de forma limpa usando os métodos `.then()` (para sucesso) e `.catch()` (para falha). Quase todas as novas APIs assíncronas do navegador (como `fetch`) e do Node.js são baseadas em `Promises`.

### 3. Fetch API

* **Conceito:** A API `fetch` é a substituta moderna do `XMLHttpRequest`. É uma API global do navegador (`window.fetch()`) projetada para ser mais simples, mais poderosa e, o mais importante, **nativamente baseada em `Promises`**. Ao chamar `fetch(url)`, ela retorna imediatamente uma `Promise` que será resolvida com um objeto `Response` assim que o servidor responder.

* **Usabilidade:** `fetch` é o **padrão atual** para fazer requisições de rede. Sua sintaxe é muito mais limpa. No entanto, ela tem uma particularidade importante: uma `Promise` de `fetch` **não é rejeitada** em caso de erros HTTP (como 404 - Não Encontrado ou 500 - Erro de Servidor). Ela só é rejeitada em caso de falha de rede (ex: sem conexão). Por isso, é responsabilidade do desenvolvedor verificar o status da resposta (ex: `if (!response.ok)`).

### 4. `async / await`

* **Conceito:** `async/await` (introduzido no ES2017) **não é uma nova API**, mas sim "açúcar sintático" (*syntactic sugar*) construído em cima das `Promises`. São duas palavras-chave usadas juntas:
    * **`async`**: Colocada antes de uma função, ela automaticamente faz com que a função retorne uma `Promise`.
    * **`await`**: Usada *dentro* de uma função `async`, ela "pausa" a execução da função até que a `Promise` à sua frente seja resolvida (ou rejeitada).

* **Usabilidade:** Esta é a **forma preferida** de consumir `Promises` no desenvolvimento moderno. `async/await` permite escrever código assíncrono que se parece e se comporta como código síncrono (linear, de cima para baixo). Isso melhora drasticamente a legibilidade e a manutenção. Além disso, permite o uso de blocos `try...catch` padrão do JavaScript para tratar erros de `Promises` rejeitadas, o que é muito mais intuitivo do que o encadeamento de `.catch()`.