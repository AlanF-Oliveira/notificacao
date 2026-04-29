# Notificacao

Microsserviço responsável pelo envio de notificações por e-mail aos usuários sobre suas tarefas agendadas e mensagens avulsas. Utiliza **Thymeleaf** para renderização dos templates HTML dos e-mails disparados.

> Este microsserviço faz parte de um ecossistema maior. O ponto de entrada recomendado para o frontend é o **[BFF Agendador de Tarefas](https://github.com/AlanF-Oliveira/bff-agendador-tarefas)**.

---

## Tecnologias

| Tecnologia | Versão | Uso |
|---|---|---|
| Java | 17 | Linguagem principal |
| Spring Boot | 4.0.3 | Framework base |
| Spring Mail | — | Envio de e-mails via SMTP |
| Thymeleaf | — | Renderização dos templates HTML |
| Lombok | — | Redução de boilerplate |
| Gradle | — | Build |
| Docker | — | Containerização |

---

## Estrutura do Projeto

```
notificacao/
├── .github/
│   └── workflows/
│       └── gradle.yml              # Pipeline CI/CD
├── src/
│   └── main/
│       ├── java/com/alan/notificacao/
│       │   ├── controller/
│       │   │   └── EmailController.java
│       │   └── business/
│       │       ├── EmailService.java
│       │       └── dto/
│       │           ├── TarefasDTO.java
│       │           └── ComunicacaoDTO.java
│       └── resources/
│           ├── templates/
│           │   ├── notificacao.html  # Template de notificação de tarefa
│           │   └── mensagem.html     # Template de mensagem avulsa
│           └── application.yaml
├── Dockerfile
└── build.gradle
```

---

## Endpoints

Base path: `/email`

| Método | Endpoint | Descrição |
|---|---|---|
| `POST` | `/email` | Envia notificação de tarefa agendada |
| `POST` | `/email/mensagem` | Envia mensagem avulsa ao destinatário |

### POST `/email` — Notificação de tarefa

Envia um e-mail HTML ao usuário informando sobre uma tarefa agendada para o dia.

**Body esperado (`TarefasDTO`):**

```json
{
  "emailUsuario": "usuario@email.com",
  "nomeTarefa": "Reunião de alinhamento",
  "dataEvento": "14:00",
  "descricao": "Reunião com o time de produto"
}
```

### POST `/email/mensagem` — Mensagem avulsa

Envia um e-mail HTML com uma mensagem personalizada ao destinatário.

**Body esperado (`ComunicacaoDTO`):**

```json
{
  "emailDestinatario": "destinatario@email.com",
  "nomeDestinatario": "João Silva",
  "dataHoraEnvio": "28/04/2025 14:30",
  "mensagem": "Olá, tudo bem?",
  "modoDeEnvio": "EMAIL",
  "telefoneDestinatario": "(85) 99999-9999"
}
```

---

## Configuração de E-mail

O serviço se conecta ao Gmail via SMTP com STARTTLS. As configurações ficam em `application.yaml`:

```yaml
spring:
  mail:
    host: smtp.gmail.com
    port: 587
    username: seu-email@gmail.com
    password: sua-senha-de-app
    protocol: smtp
    properties:
      mail:
        smtp:
          auth: true
          starttls:
            enable: true

envio:
  email:
    remetente: seu-email@gmail.com
    nomeRemetente: 'Seu Nome'

server:
  port: 8082
```

> **Importante:** utilize uma **senha de aplicativo** do Google, não sua senha pessoal. Crie uma em [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords).

---

## Executando com Docker (recomendado)

> Para subir todo o ecossistema de uma vez (BFF + todos os microsserviços + bancos), use o `docker-compose` do repositório **[bff-agendador-tarefas](https://github.com/AlanF-Oliveira/bff-agendador-tarefas)**.

Para rodar apenas este serviço isoladamente:

```bash
git clone https://github.com/AlanF-Oliveira/notificacao.git
cd notificacao
docker build -t notificacao .
docker run -p 8082:8082 notificacao
```

| Serviço | Porta |
|---|---|
| `notificacao` | `8082` |

---

## Executando sem Docker

### Pré-requisitos

- Java 17+
- Credenciais de e-mail configuradas no `application.yaml`

### Executando

```bash
git clone https://github.com/AlanF-Oliveira/notificacao.git
cd notificacao
./gradlew bootRun
```

---

## CI/CD

O projeto utiliza **GitHub Actions** para integração contínua. O pipeline é acionado automaticamente em:

- Pull Requests abertos, sincronizados ou reabertos para a branch `master`

**Etapas do pipeline:**

1. Checkout do código
2. Configuração do JDK 17 (Temurin)
3. Cache das dependências Gradle
4. Permissão de execução para o `gradlew`
5. Build com Gradle (`./gradlew build`)
6. Execução dos testes (`./gradlew test`)

O arquivo de configuração está em `.github/workflows/gradle.yml`.

---

## Microsserviços Relacionados

| Serviço | Repositório | Papel |
|---|---|---|
| **BFF** | [bff-agendador-tarefas](https://github.com/AlanF-Oliveira/bff-agendador-tarefas) | Ponto de entrada — orquestra todas as chamadas |
| **usuario** | [usuario](https://github.com/AlanF-Oliveira/usuario) | Gerencia usuários e autenticação JWT |
| **agendador-tarefas** | [agendador-tarefas](https://github.com/AlanF-Oliveira/agendador-tarefas) | Gerencia as tarefas agendadas |# Notificacao

Microsserviço responsável pelo envio de notificações por e-mail aos usuários sobre suas tarefas agendadas e mensagens avulsas. Utiliza **Thymeleaf** para renderização dos templates HTML dos e-mails disparados.

> Este microsserviço faz parte de um ecossistema maior. O ponto de entrada recomendado para o frontend é o **[BFF Agendador de Tarefas](https://github.com/AlanF-Oliveira/bff-agendador-tarefas)**.

---

## Tecnologias

| Tecnologia | Versão | Uso |
|---|---|---|
| Java | 17 | Linguagem principal |
| Spring Boot | 4.0.3 | Framework base |
| Spring Mail | — | Envio de e-mails via SMTP |
| Thymeleaf | — | Renderização dos templates HTML |
| Lombok | — | Redução de boilerplate |
| Gradle | — | Build |
| Docker | — | Containerização |

---

## Estrutura do Projeto

```
notificacao/
├── .github/
│   └── workflows/
│       └── gradle.yml              # Pipeline CI/CD
├── src/
│   └── main/
│       ├── java/com/alan/notificacao/
│       │   ├── controller/
│       │   │   └── EmailController.java
│       │   └── business/
│       │       ├── EmailService.java
│       │       └── dto/
│       │           ├── TarefasDTO.java
│       │           └── ComunicacaoDTO.java
│       └── resources/
│           ├── templates/
│           │   ├── notificacao.html  # Template de notificação de tarefa
│           │   └── mensagem.html     # Template de mensagem avulsa
│           └── application.yaml
├── Dockerfile
└── build.gradle
```

---

## Endpoints

Base path: `/email`

| Método | Endpoint | Descrição |
|---|---|---|
| `POST` | `/email` | Envia notificação de tarefa agendada |
| `POST` | `/email/mensagem` | Envia mensagem avulsa ao destinatário |

### POST `/email` — Notificação de tarefa

Envia um e-mail HTML ao usuário informando sobre uma tarefa agendada para o dia.

**Body esperado (`TarefasDTO`):**

```json
{
  "emailUsuario": "usuario@email.com",
  "nomeTarefa": "Reunião de alinhamento",
  "dataEvento": "14:00",
  "descricao": "Reunião com o time de produto"
}
```

### POST `/email/mensagem` — Mensagem avulsa

Envia um e-mail HTML com uma mensagem personalizada ao destinatário.

**Body esperado (`ComunicacaoDTO`):**

```json
{
  "emailDestinatario": "destinatario@email.com",
  "nomeDestinatario": "João Silva",
  "dataHoraEnvio": "28/04/2025 14:30",
  "mensagem": "Olá, tudo bem?",
  "modoDeEnvio": "EMAIL",
  "telefoneDestinatario": "(85) 99999-9999"
}
```

---

## Configuração de E-mail

O serviço se conecta ao Gmail via SMTP com STARTTLS. As configurações ficam em `application.yaml`:

```yaml
spring:
  mail:
    host: smtp.gmail.com
    port: 587
    username: seu-email@gmail.com
    password: sua-senha-de-app
    protocol: smtp
    properties:
      mail:
        smtp:
          auth: true
          starttls:
            enable: true

envio:
  email:
    remetente: seu-email@gmail.com
    nomeRemetente: 'Seu Nome'

server:
  port: 8082
```

> **Importante:** utilize uma **senha de aplicativo** do Google, não sua senha pessoal. Crie uma em [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords).

---

## Executando com Docker (recomendado)

> Para subir todo o ecossistema de uma vez (BFF + todos os microsserviços + bancos), use o `docker-compose` do repositório **[bff-agendador-tarefas](https://github.com/AlanF-Oliveira/bff-agendador-tarefas)**.

Para rodar apenas este serviço isoladamente:

```bash
git clone https://github.com/AlanF-Oliveira/notificacao.git
cd notificacao
docker build -t notificacao .
docker run -p 8082:8082 notificacao
```

| Serviço | Porta |
|---|---|
| `notificacao` | `8082` |

---

## Executando sem Docker

### Pré-requisitos

- Java 17+
- Credenciais de e-mail configuradas no `application.yaml`

### Executando

```bash
git clone https://github.com/AlanF-Oliveira/notificacao.git
cd notificacao
./gradlew bootRun
```

---

## CI/CD

O projeto utiliza **GitHub Actions** para integração contínua. O pipeline é acionado automaticamente em:

- Pull Requests abertos, sincronizados ou reabertos para a branch `master`

**Etapas do pipeline:**

1. Checkout do código
2. Configuração do JDK 17 (Temurin)
3. Cache das dependências Gradle
4. Permissão de execução para o `gradlew`
5. Build com Gradle (`./gradlew build`)
6. Execução dos testes (`./gradlew test`)

O arquivo de configuração está em `.github/workflows/gradle.yml`.

---

## Microsserviços Relacionados

| Serviço | Repositório | Papel |
|---|---|---|
| **BFF** | [bff-agendador-tarefas](https://github.com/AlanF-Oliveira/bff-agendador-tarefas) | Ponto de entrada — orquestra todas as chamadas |
| **usuario** | [usuario](https://github.com/AlanF-Oliveira/usuario) | Gerencia usuários e autenticação JWT |
| **agendador-tarefas** | [agendador-tarefas](https://github.com/AlanF-Oliveira/agendador-tarefas) | Gerencia as tarefas agendadas |