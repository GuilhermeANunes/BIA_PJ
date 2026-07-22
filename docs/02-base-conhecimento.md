# Base de Conhecimento

## Dados Utilizados

Alterei as informações originais, modificando o perfil do cliente para PJ e consolidei as várias fontes de dados em uma única Base de dados relacionais utilizando um arquivo sqlite3, facilitando consultas, organizando as informações e aumentando a eficiência do processo de consulta e acesso as informações, visto que com uma única consulta o agente pode acessar todas as informações necessárias, reduzindo a utilização de tokens. 

Segue a organização da base de dados:

| Arquivo | Formato | Tabela | Utilização no Agente |
|---------|---------|--------|--------------|
| `data_base.db` | DB | histprico_atendimento | Contextualizar interações anteriores |
| `data_base.db` | DB | cliente | Dados cadastrais do cliente |
| `data_base.db` | DB | metas | Personalizar recomendações |
| `data_base.db` | DB | produtos_investimento | Sugerir produtos de investimento adequados ao perfil do cliente |
| `data_base.db` | DB | produtos_credito | Sugerir produtos de credito adequados ao perfil do cliente |
| `data_base.db` | DB | produtos_recomendacao_cliente | Produtos filtrados de acordo com o perfil do cliente |
| `data_base.db` | DB | transacoes | Analisar padrão de movimentacao do cliente |

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

Para acessar a base de dados, o agente faz uma consulta SQL para buscar as informações necessárias de acordo com a pergunta do usuario, conforme exemplo:

´´´SQL
SELECT 
    c.nome AS cliente,
    c.perfil_investidor,
    c.rating,
    t.data AS data_transacao,
    t.descricao,
    t.categoria,
    t.valor,
    t.tipo
FROM cliente c
JOIN transacoes t ON c.cliente_id = t.cliente_id
WHERE c.nome = 'BradIA LTDA'
ORDER BY t.data DESC
LIMIT 10;
´´´
Mas como uma segunda opção, os dados podem ser anexados manualmente no início da sessão.

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Caso as consultas sejam realizadas pelo agente diretamente em SQL, os dados serão usados dessas maneira:

cliente | perfil_investidor | rating | data_transacao | descricao | categoria | valor | tipo
BradIA LTDA | moderado | B | 2025-10-31 | Tarifa Cesta Bancária PJ + Taxas de Emissão PIX/Boleto | Despesas Bancárias | 165.0 | saida
BradIA LTDA | moderado | B | 2025-10-28 | Assessoria Jurídica Especializada em Contratos de TI | Serviços Terceirizados | 1200.0 | saida
BradIA LTDA | moderado | B | 2025-10-27 | Cartão Caju - Benefício Alimentação / Refeição Devs | Pessoas & Benefícios | 2400.0 | saida

Caso o agente utilize Python para realizar a consulta, os dados serão usados dessa maneira:

´´´Python
[
  {
    "cliente": "BradIA LTDA",
    "perfil_investidor": "moderado",
    "rating": "B",
    "data_transacao": "2025-10-31",
    "descricao": "Tarifa Cesta Bancária PJ + Taxas de Emissão PIX/Boleto",
    "categoria": "Despesas Bancárias",
    "valor": 165.0,
    "tipo": "saida"
  },
  {
    "cliente": "BradIA LTDA",
    "perfil_investidor": "moderado",
    "rating": "B",
    "data_transacao": "2025-10-28",
    "descricao": "Assessoria Jurídica Especializada em Contratos de TI",
    "categoria": "Serviços Terceirizados",
    "valor": 1200.0,
    "tipo": "saida"
  }
]
´´´

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
