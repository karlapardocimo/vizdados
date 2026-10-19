# Dashboard de Vendas 2020

Trabalho da disciplina de Visualização de Dados (IF Sudeste MG, Campus Muriaé).
Painel em Dash + Plotly Express com o desempenho de vendas da empresa em 2020.

## O que o painel mostra

**Parte 1: visão geral**
- Faturamento por mês (gráfico de área)
- Faturamento por representante (barras)
- Faturamento por produto (tabela ordenável)
- Faturamento por regional (pizza/rosca)
- Faturamento por estado (barras)

**Parte 2: gráficos com filtros**
- Filtro de **Mês**: 10 produtos com maior faturamento no período escolhido
- Filtro de **Representante**: evolução mensal comparada à média da equipe
- Filtros de **Cliente** e **Regional**: faturamento por produto do cliente

**Parte 3: filtro encadeado**
- O usuário escolhe o **Estado**; o dropdown de **Cidade** passa a listar só as
  cidades daquele estado. O gráfico mostra o faturamento por cliente.

## Tratamento da base

Feito na função `carregar_dados()` do `app.py`:
- remoção da coluna vazia `Unnamed: 0`;
- remoção de espaços sobrando nos textos (ex.: `"Teclado "`);
- padronização do estado `MINAS` para a sigla `MG`;
- padronização dos clientes `amazon` e `kabum` para `Amazon` e `KaBuM`;
- filtro do ano de 2020 e criação das colunas de mês.

Verificações feitas na base: 3.211 pedidos, sem valores nulos, sem pedidos
duplicados e `Valor_Produto × Quantidade_Vendida = Valor_Total_Venda` em todas
as linhas.

## Rodar no computador

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate    |    Linux/Mac: source .venv/bin/activate
pip install -r requirements.txt
python app.py
```
Abrir http://127.0.0.1:8050 no navegador.

## Deploy no Render

1. Subir esta pasta para um repositório no GitHub (com a pasta `dados/`).
2. No Render: **New > Web Service** e conectar o repositório.
3. Configurar:
   - Runtime: Python
   - Build Command: `pip install -r requirements.txt`
   - Start Command: `gunicorn app:server`
   - Instance Type: Free
4. Aguardar o build e usar o link `https://....onrender.com`.

No plano gratuito o serviço "dorme" após uns 15 minutos sem acesso; o primeiro
acesso depois disso demora cerca de 1 minuto para carregar.

## Deploy no PythonAnywhere (alternativa que não dorme)

1. Criar conta gratuita e enviar a pasta (aba **Files**, ou `git clone` no console Bash).
2. No console Bash: `pip install --user -r requirements.txt`
   (se alguma versão não estiver disponível, retire o número da versão no
   requirements.txt).
3. Aba **Web > Add a new web app > Manual configuration** (escolher a versão do Python).
4. Editar o arquivo WSGI e deixar assim (trocar `SEU_USUARIO`):

```python
import sys
caminho = "/home/SEU_USUARIO/dashboard-vendas"
if caminho not in sys.path:
    sys.path.insert(0, caminho)
from app import server as application
```
5. Clicar em **Reload** e acessar `https://SEU_USUARIO.pythonanywhere.com`.
