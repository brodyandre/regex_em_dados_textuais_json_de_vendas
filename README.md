# 📝 Limpeza e Transformação de Dados de Vendas com Python e Regex

Este projeto realiza a limpeza e transformação de dados de vendas extraídos de um arquivo JSON, utilizando a linguagem Python e a biblioteca Pandas para manipulação e tratamento dos dados. A seguir, é apresentada uma descrição detalhada de cada etapa do processo.

---

## 📌 Estrutura do Projeto

* `dados_vendas_clientes.json`: Arquivo JSON contendo os dados brutos de vendas e clientes.
* `script_limpeza_transformacao.py`: Script Python para limpeza e transformação dos dados.
* `README.md`: Documentação do processo, descrevendo cada etapa de forma detalhada.

---

## 🔧 Dependências Necessárias

Para executar o projeto, certifique-se de ter instaladas as seguintes dependências:

```bash
pip install pandas numpy
```
## 🐍 Linguagem Utilizada: Python
<div align="center"> <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/python/python-original.svg" alt="Python" width="80" height="80"/> </div>
Python é uma linguagem de programação de alto nível, interpretada e de fácil leitura, amplamente utilizada para análise e manipulação de dados. Neste projeto, Python é a base para o processamento dos dados de vendas, utilizando bibliotecas poderosas como Pandas e NumPy para facilitar a limpeza, transformação e análise dos dados.

---

## 📂 Importação das Bibliotecas

As bibliotecas essenciais para a manipulação e tratamento dos dados são importadas no script:

```python
import numpy as np
import json
import pandas as pd
import re
import unicodedata
```

---

## 📥 Leitura dos Dados

O arquivo JSON é carregado e transformado em um DataFrame do Pandas para facilitar a manipulação dos dados:

```python
with open('/content/dados_vendas_clientes.json', 'r') as file:
    dados = json.load(file)

dados = pd.DataFrame(dados)
dados.head()
```

---

## 🛠️ Normalização dos Dados

A normalização é aplicada para facilitar o acesso às informações:

```python
dados = pd.json_normalize(dados['dados_vendas'])
dados
```

---

## ✏️ Renomeação das Colunas

As colunas são renomeadas para uma melhor interpretação:

```python
colunas = list(dados.columns)
dados = dados.rename(columns={
    colunas[0]: 'data_da_venda',
    colunas[1]: 'cliente',
    colunas[2]: 'valor_da_compra'
})
dados
```

---

## 🔄 Tratamento de Dados

Para manipulação de listas aninhadas, o método `explode()` é utilizado:

```python
colunas = list(dados.columns)
dados = dados.explode(colunas[1:])
dados.reset_index(drop=True, inplace=True)
dados
```

---

## 🔤 Remoção de Acentos e Caracteres Especiais

Uma função foi criada para remover acentuação e caracteres especiais dos nomes dos clientes:

```python
def remove_acentos_caracteres_especiais(texto):
    nfkd = unicodedata.normalize('NFKD', texto)
    texto_sem_acentos = u"".join([c for c in nfkd if not unicodedata.combining(c)])
    texto_limpo = re.sub(r'[^a-zA-Z\s]', '', texto_sem_acentos).lower()
    return texto_limpo

dados['cliente'] = dados['cliente'].apply(remove_acentos_caracteres_especiais)
dados
```

---

## 💲 Formatação dos Valores Monetários

Os valores das compras são convertidos para tipo float:

```python
dados['valor_da_compra'] = dados['valor_da_compra'].str.replace('R$', '', regex=False)
dados['valor_da_compra'] = dados['valor_da_compra'].str.replace('.', '', regex=False)
dados['valor_da_compra'] = dados['valor_da_compra'].str.replace(',', '.', regex=False)
dados['valor_da_compra'] = dados['valor_da_compra'].astype(float)
dados
```

---

## 📅 Conversão da Data

A coluna de data é convertida para o tipo datetime:

```python
dados['data_da_venda'] = pd.to_datetime(dados['data_da_venda'])
dados.info()
dados
```

---

## ✅ Exemplos Práticos de Análise

Após a limpeza e transformação, podemos realizar algumas análises para extrair insights:

### 🔹 Valor Total de Vendas:

```python
valor_total = dados['valor_da_compra'].sum()
print(f"Valor Total de Vendas: R$ {valor_total:,.2f}")
```

### 🔹 Número de Vendas por Cliente:

```python
vendas_por_cliente = dados['cliente'].value_counts()
print(vendas_por_cliente)
```

### 🔹 Análise Temporal - Vendas por Mês:

```python
dados['mes_ano'] = dados['data_da_venda'].dt.to_period('M')
vendas_por_mes = dados.groupby('mes_ano')['valor_da_compra'].sum()
print(vendas_por_mes)
```

---

## 🎯 Conclusão

Os dados estão devidamente tratados e organizados, prontos para serem analisados. Esse processo garante maior confiabilidade e qualidade na análise dos dados de vendas.

---

## 🖥️ Autor

* Luiz André de Souza ([brodyandre](https://github.com/brodyandre))
  ""
