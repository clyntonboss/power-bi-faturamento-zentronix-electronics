# Medidas – Vendas

Este documento descreve as medidas criadas no modelo semântico do Power BI relacionadas à tabela **fPedidos**.

Todas as medidas seguem o padrão de desenvolvimento adotado no projeto:

- Uso de `VAR` para armazenar resultados intermediários
- Uso de `COALESCE()` para evitar retorno `BLANK()`
- Uso de `DIVIDE()` para evitar erro de divisão por zero
- Documentação clara da regra de negócio
- Estrutura padronizada para facilitar manutenção e escalabilidade

---

# Medidas da Fato Pedidos

## faturamento

**Descrição**

Faturamento total gerado pelos pedidos registrados no modelo.

**Tabela origem**

`fPedidos`

**Regra de negócio**

Soma os valores da coluna `valor_venda` da tabela `fPedidos`, representando a receita total das vendas.

**Retorno**

Valor numérico representando o faturamento total.

```DAX
faturamento =
VAR Resultado =
    SUM(
        fPedidos[valor_venda]
    )
RETURN
    COALESCE(Resultado, 0)
```

---

## quantidade_pedidos

**Descrição**

Quantidade total de pedidos registrados no modelo.

**Tabela origem**

`fPedidos`

**Regra de negócio**

Conta todas as linhas da tabela `fPedidos`, onde cada linha representa um pedido registrado no sistema.

**Retorno**

Número inteiro representando a quantidade total de pedidos.

```DAX
quantidade_pedidos =
VAR Resultado =
    COUNTROWS(
        fPedidos
    )
RETURN
    COALESCE(Resultado, 0)
```

---

## quantidade_produtos

**Descrição**

Quantidade total de produtos vendidos.

**Tabela origem**

`fPedidos`

**Regra de negócio**

Soma os valores da coluna `quantidade_produto` da tabela `fPedidos`, representando o total de itens vendidos.

**Retorno**

Número inteiro representando a quantidade total de produtos vendidos.

```DAX
quantidade_produtos =
VAR Resultado =
    SUM(
        fPedidos[quantidade_produto]
    )
RETURN
    COALESCE(Resultado, 0)
```

---

## ticket_medio

**Descrição**

Valor médio gasto por pedido.

**Regra de negócio**

Divide o faturamento total pela quantidade total de pedidos, representando o valor médio de cada pedido.

**Dependências**

- `[faturamento]`
- `[quantidade_pedidos]`

**Retorno**

Valor monetário representando o ticket médio.

```DAX
ticket_medio =
VAR Resultado =
    DIVIDE(
        [faturamento],
        [quantidade_pedidos]
    )
RETURN
    COALESCE(Resultado, 0)
```

---

# Padrão adotado no projeto

Todas as medidas seguem os seguintes princípios de desenvolvimento:

- Clareza na regra de negócio
- Padronização de código
- Tratamento de valores nulos
- Uso de funções seguras para cálculos (`COALESCE` e `DIVIDE`)
- Facilidade de manutenção
- Documentação integrada ao projeto

Esse padrão garante maior **qualidade técnica**, **legibilidade do modelo semântico** e **facilidade de colaboração em ambientes versionados**, como projetos mantidos no GitHub.
