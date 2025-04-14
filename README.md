
<div align="center" style="font-family: Arial, sans-serif; font-size: 20px; line-height: 1.5;">

# 🌟 **Curso AWS Developer - EDN** 🌟  
# 🌟 Developer Associate 🌟

<a href="https://escoladanuvem.org"><a href="https://aws.amazon.com/pt/?nc2=h_lg">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Amazon_Web_Services_Logo.svg/2560px-Amazon_Web_Services_Logo.svg.png" width="150" alt="AWS Logo">
</a>

<img src="https://svgmix.com/uploads/1eb335-aws-sqs.svg" width="80" alt="S3 Logo"/>-
<img src="https://assets.streamlinehq.com/image/private/w_300,h_300,ar_1/f_auto/v1/icons/1/aws-sns-80t5m6hy89y74i2ta3yrz3.png/aws-sns-y7pv8d8lqjroqxkucbnv.png?_a=DAJFJtWIZAAC" width="80" alt="Lambda Logo"/>-
<img src="https://github.com/HalleyVeras/Escola_da_Nuvem/blob/main/Documentos/download%20(4)_processed.png?raw=true" width="180" alt="EDN Logo">

</div>

# 🧪 Laboratório 5 — Criação de Tópico SNS, Fila SQS Padrão e DLQ

Este laboratório explora a integração entre o Amazon SNS (Simple Notification Service) e o Amazon SQS (Simple Queue Service), demonstrando como mensagens podem ser publicadas por um serviço e consumidas por outros de forma assíncrona e resiliente. Também será configurada uma Dead-Letter Queue (DLQ) para tratamento de falhas no processamento de mensagens.

---

## 🎯 Objetivos

Você aprenderá a:

- Criar uma fila SQS para servir como **Dead-Letter Queue (DLQ)**.
- Criar uma fila SQS padrão principal.
- Configurar a fila principal para redirecionar mensagens para a DLQ após falhas de processamento.
- Criar um tópico SNS.
- Inscrever a fila principal no tópico SNS.
- Publicar mensagens no tópico e verificar o recebimento na fila.
- Simular falha e verificar o redirecionamento automático para a DLQ.

---

## 🧠 Cenário

Você está desenvolvendo uma aplicação baseada em microsserviços. Um serviço precisa notificar outros serviços sobre eventos importantes. Para desacoplar os componentes, usaremos:

- SNS como barramento de eventos.
- SQS como buffer confiável para assinantes.
- DLQ para isolar e analisar mensagens com falha no processamento.

---

## ✅ Pré-requisitos

- Conta ativa na AWS.
- Permissões para gerenciar recursos **SNS** e **SQS**.
- (Opcional) Endereço de e-mail para teste com tópicos SNS.

---

## 🛠️ Etapas do Laboratório

### 🔹 Parte 1 — Criação das Filas SQS

1. Acesse o **Console do SQS**.
2. Crie a fila **DLQ**:
   - Tipo: Padrão.
   - Nome: `minha-dlq-lab-seunome-aaaammdd`.
   - Mantenha as configurações padrão.
   - Copie o ARN da fila após a criação.
3. Crie a **fila principal**:
   - Tipo: Padrão.
   - Nome: `minha-fila-principal-lab-seunome-aaaammdd`.
   - Ative a opção de Dead-letter queue.
   - Cole o ARN da DLQ.
   - Defina o número máximo de recebimentos como `3`.
   - Copie o ARN da fila principal após a criação.

---

### 🔹 Parte 2 — Criação do Tópico SNS

1. Acesse o **Console do SNS**.
2. Crie um novo tópico:
   - Tipo: Padrão.
   - Nome: `meu-topico-lab-seunome-aaaammdd`.
   - Copie o ARN do tópico após a criação.

---

### 🔹 Parte 3 — Conectando SNS e SQS

1. Crie uma **assinatura** no tópico:
   - Protocolo: Amazon SQS.
   - Endpoint: ARN da fila principal.
   - (Opcional) Marque "Entrega bruta" se quiser ver a mensagem sem metadados.
2. Configure a **política da fila principal**:
   - Edite a política da fila para permitir que o tópico SNS envie mensagens.
   - Use o modelo fornecido no laboratório e substitua os placeholders pelos seus ARNs e ID da conta.

---

### 🔹 Parte 4 — Testes

#### ✅ Teste de publicação e recebimento:

- Publique uma mensagem no tópico SNS.
- Verifique se a mensagem foi recebida na fila principal.

```json
{
  "evento": "pedido_criado",
  "pedido_id": "12345",
  "cliente_id": "9876",
  "timestamp": "2024-03-15T10:00:00Z"
}
```

#### ❌ Teste de falha e envio para DLQ:

- Aguarde a mensagem ser reprocessada 3 vezes (sem excluí-la).
- Após o limite de tentativas, verifique a DLQ e confirme que a mensagem foi redirecionada.

---

## 📌 Dicas

- Não exclua a mensagem manualmente durante os testes de DLQ.
- Utilize o tempo limite de visibilidade para forçar o reprocessamento.
- Habilite "Entrega bruta" no SNS para mensagens limpas, se desejado.

---
## 🎥 Vídeo Demonstrativo

Você poderá assistir a um vídeo demonstrando todo o processo passo a passo. Fique ligado!

---
## 📚 Conceitos abordados

- **SNS**: Serviço de publicação/assinatura para notificação de eventos.
- **SQS**: Fila para desacoplamento e durabilidade.
- **DLQ**: Mecanismo para lidar com mensagens que falham no processamento.



