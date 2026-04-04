# 🔔 Notificacao

Microsserviço responsável pelo envio de notificações por e-mail aos usuários sobre suas tarefas agendadas. Utiliza templates **HTML** para composição dos e-mails disparados.

> Este microsserviço faz parte de um ecossistema maior. O ponto de entrada recomendado para o frontend é o **[BFF Agendador de Tarefas](https://github.com/AlanF-Oliveira/bff-agendador-tarefas)**.

---

## 🚀 Tecnologias

| Tecnologia | Versão | Uso |
|---|---|---|
| Java | 17 | Linguagem principal |
| Spring Boot | — | Framework base |
| HTML | — | Templates de e-mail |
| Gradle | — | Build |
| Docker | — | Containerização |

---

## 📁 Estrutura do Projeto

```
notificacao/
├── .github/
│   └── workflows/          # Pipelines CI/CD
├── src/
│   └── main/
│       ├── java/           # Código fonte Java
│       └── resources/      # Templates HTML de e-mail
├── Dockerfile
└── build.gradle
```

---

## 🐳 Executando com Docker (recomendado)

> Para subir todo o ecossistema de uma vez (BFF + todos os microsserviços + bancos), use o `docker-compose` do repositório **[bff-agendador-tarefas](https://github.com/AlanF-Oliveira/bff-agendador-tarefas)**.

Para rodar apenas este serviço isoladamente:

```bash
git clone https://github.com/AlanF-Oliveira/notificacao.git
cd notificacao
docker build -t notificacao .
docker run -p 8082:8082 notificacao
```

### Serviço e porta

| Serviço | Porta |
|---|---|
| `notificacao` | `8082` |

---

## 🔧 Dockerfile

Build multi-stage com Gradle:

```dockerfile
# Stage 1 — build
FROM gradle:8.14-jdk17 AS build
WORKDIR /app
COPY . .
RUN gradle build --no-daemon

# Stage 2 — runtime
FROM eclipse-temurin:17-jdk
WORKDIR /app
COPY --from=build /app/build/libs/*.jar /app/notificacao.jar
EXPOSE 8082
CMD ["java", "-jar", "/app/notificacao.jar"]
```

---

## ▶️ Executando sem Docker

### Pré-requisitos

- Java 17+

### Executando

```bash
git clone https://github.com/AlanF-Oliveira/notificacao.git
cd notificacao
./gradlew bootRun
```

---

## 🧩 Microsserviços Relacionados

| Serviço | Repositório | Papel |
|---|---|---|
| **BFF** | [bff-agendador-tarefas](https://github.com/AlanF-Oliveira/bff-agendador-tarefas) | Ponto de entrada — orquestra todas as chamadas |
| **usuario** | [usuario](https://github.com/AlanF-Oliveira/usuario) | Gerencia usuários e autenticação JWT |
| **agendador-tarefas** | [agendador-tarefas](https://github.com/AlanF-Oliveira/agendador-tarefas) | Gerencia as tarefas agendadas |

---


## 👤 Autor

**Alan F. Oliveira** — [github.com/AlanF-Oliveira](https://github.com/AlanF-Oliveira) 