# Atividade Prática: API REST de Biblioteca
 
**Tecnologias:** Quarkus, JPA (Hibernate ORM), PostgreSQL, Docker
**Arquitetura:** Entity, DTO, Repository, Service e Controller
**Modalidade:** individual ou em dupla
 
---
 
## 1. Contexto
 
Uma biblioteca comunitária precisa sair das planilhas e ter um sistema para controlar seu acervo. Você foi contratado(a) para criar a **API REST** que vai gerenciar os **livros** da biblioteca e controlar os **empréstimos**.
 
Use como base o projeto desenvolvido em aula (Produto). A estrutura é a mesma, mas agora o domínio é outro.
 
## 2. Objetivo
 
Aplicar, em um caso novo, os conceitos vistos em aula:
 
- mapeamento objeto-relacional com JPA;
- separação em camadas (Entity, DTO, Repository, Service, Controller);
- validação de dados de entrada;
- tratamento de regras de negócio;
- conexão com PostgreSQL rodando em Docker.
## 3. Requisitos do sistema
 
### 3.1 Entidade `Livro`
 
| Campo | Tipo | Regras |
|---|---|---|
| `id` | Long | Gerado automaticamente pelo banco |
| `titulo` | String | Obrigatório, até 150 caracteres |
| `autor` | String | Obrigatório, até 100 caracteres |
| `isbn` | String | Obrigatório, único, exatamente 13 caracteres |
| `anoPublicacao` | Integer | Obrigatório, entre 1500 e o ano atual |
| `disponivel` | Boolean | Começa como `true` ao cadastrar |
 
### 3.2 DTOs
 
- **`LivroRequestDTO`**: dados de entrada (`titulo`, `autor`, `isbn`, `anoPublicacao`) com as validações (`@NotBlank`, `@NotNull`, `@Size`, `@Min`, `@Max`).
- **`LivroResponseDTO`**: dados de saída (`id`, `titulo`, `autor`, `isbn`, `anoPublicacao`, `disponivel`).
O cliente **não** pode enviar o `id` nem o campo `disponivel` ao cadastrar um livro.
 
### 3.3 Endpoints obrigatórios
 
| Método | Rota | Descrição | Status de sucesso |
|---|---|---|---|
| POST | `/livros` | Cadastrar livro | 201 |
| GET | `/livros` | Listar todos os livros | 200 |
| GET | `/livros/{id}` | Buscar livro por id | 200 |
| PUT | `/livros/{id}` | Atualizar dados do livro | 200 |
| DELETE | `/livros/{id}` | Remover livro | 204 |
| GET | `/livros?autor=...` | Filtrar livros pelo nome do autor (parcial, sem diferenciar maiúsculas) | 200 |
 
### 3.4 Regras de negócio (no Service)
 
1. Não permitir cadastrar dois livros com o mesmo **ISBN**. Nesse caso, retornar erro **409 (Conflict)** com mensagem clara.
2. Ao buscar, atualizar ou remover um id inexistente, retornar **404** com a mensagem "Livro não encontrado".
3. Não permitir remover um livro que esteja **emprestado** (`disponivel = false`). Retornar **409**.
### 3.5 Funcionalidade de empréstimo
 
Criar dois endpoints para controlar a disponibilidade:
 
| Método | Rota | Regra |
|---|---|---|
| PATCH | `/livros/{id}/emprestar` | Só funciona se o livro estiver disponível. Caso contrário, retornar **409** ("Livro já está emprestado"). |
| PATCH | `/livros/{id}/devolver` | Só funciona se o livro estiver emprestado. Caso contrário, retornar **409** ("Livro não está emprestado"). |
 
Ambos devolvem o `LivroResponseDTO` atualizado.
 
## 4. Configuração do ambiente
 
1. Crie um `docker-compose.yml` com o PostgreSQL, usando **outro nome de banco** (por exemplo, `bibliotecadb`) e outro nome de container.
2. Ajuste o `application.properties` com a nova URL de conexão.
3. Mantenha `quarkus.hibernate-orm.database.generation=update` apenas para desenvolvimento.
4. Confirme no log do Quarkus que a tabela `livro` foi criada.
## 5. Estrutura esperada
 
```
src/main/java/org/minsait/
├── entity/      Livro.java
├── dto/         LivroRequestDTO.java
│                LivroResponseDTO.java
├── repository/  LivroRepository.java
├── service/     LivroService.java
└── controller/  LivroController.java
```
 
## 6. Dicas
 
- Use o `ProdutoService` e o `ProdutoRepository` como modelo, mas **não copie e cole sem entender**: cada linha deve fazer sentido para o novo domínio.
- O Repository só conhece **entidades** e JPQL. Ele não deve lançar exceções HTTP nem usar DTOs.
- Regras de negócio e `@Transactional` ficam no **Service**.
- O Controller só trata HTTP: rotas, `@Valid` e status de resposta.
- Para verificar ISBN duplicado, crie no Repository um método de consulta por ISBN que retorne `Optional<Livro>`.
- Para devolver status 409, pesquise sobre `WebApplicationException` ou `ClientErrorException` com `Response.Status.CONFLICT`.
- Para `PATCH`, use a anotação `@PATCH` do pacote `jakarta.ws.rs`.
- Teste tudo pelo Swagger UI (`/q/swagger-ui`) ou com `curl`.
## 7. Desafios extras (opcional)
 
1. Adicionar o endpoint `GET /livros/disponiveis`, listando apenas os livros disponíveis.
2. Adicionar paginação em `GET /livros` com os parâmetros `pagina` e `tamanho`.
3. Criar uma entidade `Categoria` (por exemplo, Romance, Técnico, Infantil) e relacioná-la ao livro com `@ManyToOne`.
4. Escrever um teste com RestAssured para o `POST /livros` (caso de sucesso e caso de dado inválido).
## 8. O que entregar
 
1. O projeto completo (repositório Git ou pasta compactada **sem** a pasta `target`).
2. O arquivo `docker-compose.yml`.
3. Um arquivo `README.md` curto contendo:
   - como subir o banco e a aplicação;
   - lista dos endpoints;
   - exemplos de `curl` para cadastrar, emprestar e devolver um livro.
