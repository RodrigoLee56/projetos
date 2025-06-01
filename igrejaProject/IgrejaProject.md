---

# 📊 Estudo de Caso: Aplicação Web de Controle Financeiro para Igreja

## 1. 💡 Contexto e Objetivo

A aplicação tem como objetivo auxiliar comunidades religiosas (como igrejas locais) no controle financeiro de entradas (ofertas, doações, eventos) e saídas (despesas com aluguel, energia, manutenção etc.). O sistema será inicialmente desenvolvido como uma aplicação web responsiva, com possibilidade futura de adaptação para dispositivos móveis.

### Público-Alvo:
- Líderes de pequenas congregações
- Tesoureiros de igrejas sem sistemas digitais de controle
- Voluntários que desejam melhorar a transparência financeira

---

## 2. 🛠️ Tecnologias Utilizadas

| Camada | Tecnologia |
|-------|------------|
| Linguagem | **Java 21 (OpenJDK)** |
| Framework Backend | **Spring Boot 3.x** |
| Template Engine | **Thymeleaf** |
| Banco de Dados | **PostgreSQL** |
| Frontend | **Bootstrap 5 + CSS puro** |
| Build Tool | **Maven** |
| Versionamento | **Git + GitHub** |

> ⚠️ Não será utilizada autenticação via Spring Security (não é necessária no momento).

---

## 3. 🗃️ Modelagem do Banco de Dados

### Tabelas Propostas:

#### `categoria`
Armazena categorias de lançamentos (ex: “Oferta”, “Energia Elétrica”)

```sql
CREATE TABLE categoria (
    id BIGSERIAL PRIMARY KEY,
    nome VARCHAR(100) NOT NULL,
    tipo VARCHAR(10) CHECK (tipo IN ('ENTRADA', 'SAIDA')) NOT NULL
);
```

#### `lancamento`
Armazena os registros financeiros

```sql
CREATE TABLE lancamento (
    id BIGSERIAL PRIMARY KEY,
    descricao VARCHAR(255),
    valor NUMERIC(10,2) NOT NULL,
    data DATE NOT NULL,
    tipo VARCHAR(10) CHECK (tipo IN ('ENTRADA', 'SAIDA')) NOT NULL,
    categoria_id BIGINT REFERENCES categoria(id)
);
```

---

## 4. 🏗️ Arquitetura do Projeto (Spring Boot + MVC)

```
src/
├── main/
│   ├── java/
│   │   └── com.exemplo.igrejafinance/
│   │       ├── controller/      → Controladores Thymeleaf
│   │       ├── model/           → Entidades JPA
│   │       ├── repository/      → Interfaces Spring Data JPA
│   │       ├── service/         → Lógica de negócio
│   │       └── dto/             → Records ou DTOs (Java 21)
│   │
│   └── resources/
│       ├── templates/           → Páginas HTML Thymeleaf
│       ├── static/css/          → Estilos Bootstrap 5 + CSS customizado
│       ├── application.properties → Configurações do projeto
│       └── data.sql             → Dados iniciais (seed)
```

---

## 5. ✨ Funcionalidades Principais

| Funcionalidade | Descrição |
|----------------|-----------|
| Cadastro de Categorias | Entradas e saídas organizadas por categoria |
| Lançamentos | Registro de valores com data, descrição e categoria |
| Visualização de Saldo | Exibe total de entradas, saídas e saldo atual |
| Histórico Financeiro | Listagem paginada dos lançamentos |
| Filtro por Período/Categoria | Facilidade na análise financeira |
| Relatório Mensal | Agrupado por mês, com totais por entrada/saída |

---

## 6. 🖥️ Interface Responsiva com Thymeleaf + Bootstrap 5

Páginas propostas:

- `index.html` – Visão geral com resumo financeiro
- `lancamentos.html` – Lista completa de lançamentos com filtros
- `form-lancamento.html` – Formulário para cadastro/editar lançamento
- `categorias.html` – Gerenciamento das categorias
- `relatorios.html` – Exibição simples de relatórios mensais

### Exemplo de trecho HTML com Thymeleaf:

```html
<tr th:each="l : ${lancamentos}">
    <td th:text="${l.data}"></td>
    <td th:text="${l.descricao}"></td>
    <td th:text="${l.tipo}"></td>
    <td th:text="${#numbers.formatDecimal(l.valor, 2, 'COMMA')}"></td>
</tr>
```

---

## 7. 🔧 Configuração Inicial

### application.properties

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/igreja_finance
spring.datasource.username=seu_usuario
spring.datasource.password=sua_senha
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
```

---

## 8. 🧪 Boas Práticas Recomendadas

- Uso de **records** (Java 21) para DTOs leves
- Separação clara entre camadas (Model, View, Controller)
- Uso de interfaces Spring Data JPA para repositórios
- Git Flow com branches `main`, `develop` e features
- Commits descritivos e uso de `.gitignore`
- Validação de campos no backend (mesmo sem Spring Security)
- Testes unitários com JUnit 5 (opcional)

---

## 9. 🚀 Melhorias Futuras Sugeridas

- Exportação de relatórios em PDF ou Excel
- Gráficos interativos com Chart.js
- API REST separada (para consumo mobile futuro)
- Integração com Google Calendar para lembretes de contas
- Backup automático do banco de dados
- Multi-igreja (suporte a múltiplas congregações)

---

## 10. 🧾 Resumo Final

Este estudo de caso apresentou uma estrutura robusta e prática para o desenvolvimento de um sistema de gestão financeira para igrejas utilizando tecnologias modernas e produtivas. A combinação de **Spring Boot**, **Thymeleaf**, **PostgreSQL** e **Bootstrap 5** permite criar uma solução rápida, fácil de manter e adaptável a necessidades futuras.