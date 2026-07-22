# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_cliente.json` | JSON | Personalizar recomendações |
| `produtos_financeiros.json` | JSON | Sugerir produtos adequados ao perfil do cliente |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Sim, alterei o perfil do cliente para um perfil PJ, adicionei informações como faturamento mensal, rating bancário e rar do cliente (Retorno ajustado ao Risco), métricas que os bancos utilizam para avaliar condições de contratação de créditos para os clientes.

Além disso, alterei as transações do cliente, para ficarem compátiveis com o nível de movimentação de uma empresa e volume das transações.

Também adicionei alguns produtos de crédito na base de produtos financeiros, com o objetivo de ofertar essas soluções para o cliente atingir seus objetivos, de maneiras diferentes, seja financiando com uma linha de crédito, ou investindo em produtos de investimento, cada um de acordo com o perfil do cliente.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

Os arquivos JSON e CSV são carregados junto com a documentação do agente, gerando assim a base de dados necessária para as interações.

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Os dados são consultados dinamicamente pelo modelo, visto que foram incluidos na memória do agente, antes de inicar a interação.

Sendo assim, as respostas que são geradas por ele, são embasadas nos dados fornecidos previamente.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
Dados do Cliente:
- Nome: BradIA ltda
- Perfil: Moderado
- Saldo disponível: R$ 50.000

Últimas transações:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
...
```
