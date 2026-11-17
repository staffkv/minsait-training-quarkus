# Exercício 01 — Criando sua primeira classe Java: Cliente

## Objetivo

Praticar os conceitos básicos de Java aprendidos em aula, criando uma classe que representa um cliente dentro de um projeto Quarkus.

Ao finalizar, o projeto deverá ser publicado em um repositório no GitHub.

## 1. Criando o projeto

1. Acesse o [Quarkus Code Generator](https://code.quarkus.io/).
2. Crie um novo projeto Quarkus.
3. Utilize as configurações apresentadas durante a aula.
4. Baixe e abra o projeto na sua IDE.

## 2. Desenvolvendo a atividade

Dentro do projeto, crie uma classe Java chamada `Cliente`.

A classe deverá representar um cliente e possuir os seguintes atributos:

| Atributo | Tipo |
|---|---|
| `id` | `Long` |
| `nome` | `String` |
| `idade` | `Integer` |
| `email` | `String` |
| `telefone` | `String` |

### Exemplo inicial

```java
public class Cliente {

    private Long id;
    private String nome;
    private Integer idade;
    private String email;
    private String telefone;

}
```

### Sua tarefa

- Criar a classe `Cliente`.
- Definir os cinco atributos solicitados.
- Utilizar os tipos de dados adequados.
- Declarar todos os atributos como `private`.

## 3. Publicando no GitHub

1. Acesse o GitHub.
2. Crie um repositório chamado `exercicio-01-cliente`.
3. Configure o Git no projeto.
4. Realize o primeiro commit.
5. Envie o projeto para o GitHub (`push`).

[Criar repositório no GitHub](https://github.com/new)

## 4. Entrega

Ao finalizar, compartilhe o link do seu repositório GitHub com o instrutor.

### Checklist de conclusão

- [ ] Projeto Quarkus criado.
- [ ] Classe `Cliente` criada.
- [ ] Cinco atributos definidos corretamente.
- [ ] Atributos declarados como `private`.
- [ ] Projeto publicado no GitHub.
- [ ] Link do repositório compartilhado.

## 5. Desafio extra (opcional)

Adicione mais um atributo à classe `Cliente`, como:

- `endereco`
- `dataNascimento`
- `profissao`

**Bom exercício!**
