----
# Projeto Biosampa

```text

    _|_|_|    _|                                                                            
    _|    _|      _|_|      _|_|_|    _|_|_|  _|_|_|  _|_|    _|_|_|      _|_|_|
    _|_|_|    _|  _|    _|  _|_|    _|    _|  _|    _|    _|  _|    _|  _|    _|
    _|    _|  _|  _|    _|      _|  _|    _|  _|    _|    _|  _|    _|  _|    _|
    _|_|_|    _|    _|_|    _|_|_|    _|_|_|  _|    _|    _|  _|_|_|      _|_|_|
                                                              _|              
                                                              _|
```

![Badge Databricks](https://img.shields.io/badge/Databricks-Community-red)
![Badge Python](https://img.shields.io/badge/Python-PySpark-blue)
![Badge Delta](https://img.shields.io/badge/Storage-Delta_Lake-orange)
![Badge Governance](https://img.shields.io/badge/Governance-Unity_Catalog-green)

----

## Data Governance & Lakehouse Demo com Databricks Unity Catalog

Este projeto demonstra um pipeline completo de Engenharia de Dados (Arquitetura Medalhão) integrado a um fluxo de Governança de Dados utilizando Databricks e Unity Catalog.

O objetivo é simular um cenário real de proteção ambiental (Projeto Biosampa), onde o acesso a dados sensíveis de fauna deve ser controlado rigorosamente.

## Arquitetura

O projeto segue a arquitetura **Lakehouse** com as camadas:
1.  **Bronze (Raw):** Ingestão direta de dados públicos da Prefeitura de SP (CSV) com tratamento de encoding e schema drift.
2.  **Silver (Clean):** Limpeza, tipagem de dados e aplicação de regras de negócio.
3.  **Gold (Aggregated):** KPIs prontos para consumo de BI.

## Destaque: Governança Baseada em Tags

Diferente do modelo tradicional de concessão de acessos manuais, este projeto implementa uma **Governança Orientada a Metadados**:

* **Definição:** As tabelas recebem `TAGS` (ou `TBLPROPERTIES`) que atuam como "contratos imutáveis" (ex: `governance_purpose = pesquisa`).
* **Validação:** Um script Python atua como "Guardião", verificando automaticamente se o propósito da solicitação de acesso é compatível com a Tag da tabela.
* **Auditoria:** Toda concessão de acesso é registrada em uma tabela de log (`audit_access_log`).

### Fluxo de Validação (Código Demonstrativo)

```python
# Exemplo da lógica implementada
if purpose_proposed in allowed_purposes:
    print("ACESSO APROVADO: Propósito compatível com a Tag.")
    execute_grant()
else:
    print("ACESSO NEGADO: Violação de Governança.")
