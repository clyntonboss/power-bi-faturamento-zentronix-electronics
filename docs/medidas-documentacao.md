# Medidas -- Vendas

Este documento descreve as medidas criadas no modelo semântico do Power
BI relacionadas à tabela **fPedidos**.

Todas as medidas seguem o padrão de desenvolvimento adotado no projeto:

-   Uso de VAR para armazenar resultados intermediários
-   Uso de COALESCE() para evitar retorno BLANK()
-   Uso de DIVIDE() para evitar erro de divisão por zero
-   Documentação clara da regra de negócio
-   Estrutura padronizada para facilitar manutenção e escalabilidade

------------------------------------------------------------------------

# Medidas da Fato Pedidos

## faturamento

**Descrição**

Faturamento total gerado pelos pedidos registrados no modelo.

**Tabela origem**

`fPedidos`

**Regra de negócio**

Soma os valores da coluna `valor_venda` da tabela `fPedidos`,
representando a receita total das vendas.

``` dax
faturamento =
VAR Resultado =
    SUM(
        fPedidos[valor_venda]
    )
RETURN
    COALESCE(Resultado, 0)
```

------------------------------------------------------------------------

## quantidade_pedidos

**Descrição**

Quantidade total de pedidos registrados.

``` dax
quantidade_pedidos =
VAR Resultado =
    COUNTROWS(
        fPedidos
    )
RETURN
    COALESCE(Resultado, 0)
```

------------------------------------------------------------------------

## quantidade_produtos

**Descrição**

Quantidade total de produtos vendidos.

``` dax
quantidade_produtos =
VAR Resultado =
    SUM(
        fPedidos[quantidade_produto]
    )
RETURN
    COALESCE(Resultado, 0)
```

------------------------------------------------------------------------

## ticket_medio

**Descrição**

Valor médio gasto por pedido.

``` dax
ticket_medio =
VAR Resultado =
    DIVIDE(
        [faturamento],
        [quantidade_pedidos]
    )
RETURN
    COALESCE(Resultado, 0)
```
