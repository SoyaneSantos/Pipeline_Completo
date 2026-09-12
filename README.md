# Pipeline de Coleta de Cotações 
**Python | APIs (Coinbase, Yahoo Finance) | yfinance | Pandas**

**Problema:** Como coletar, padronizar e consolidar em tempo real cotações
de ativos de fontes diferentes (Bitcoin via Coinbase, commodities via Yahoo
Finance) para viabilizar análises futuras de variação de preço?

**Solução:** Desenvolvi scripts independentes de coleta para cada fonte
(Bitcoin e commodities — ouro, petróleo WTI e prata), padronizando o retorno
em um DataFrame comum, e um script orquestrador que consolida os dados e
salva em CSV.

**Como solucionei:** Criei módulos separados (`GetBitcoin.py`,
`GetCommodities.py`) para isolar cada fonte de dados, um orquestrador
(`GetPrices.py`) com opções de execução única ou contínua (loop com
salvamento incremental a cada 60s), garantindo formato consistente entre
ativos diferentes.

**Resultado:** Um pipeline funcional de coleta contínua de cotações, com
base estruturada (`cotacoes.csv`) pronta para servir de insumo a cálculos
de KPIs e dashboards futuros.

---

## 📂 Estrutura do Projeto

### **GetBitcoin.py**

- Script responsável por coletar a **cotação atual do Bitcoin** em USD.
- Fonte: API pública da **Coinbase**.
- Retorna um **DataFrame padronizado** com as colunas:
  - `ativo` — símbolo do ativo (`BTC-USD`)
  - `preco` — preço atual
  - `moeda` — moeda de cotação (USD)
  - `horario_coleta` — horário local da coleta
- Pode ser executado de forma independente (`python GetBitcoin.py`) para teste.

---

### **GetCommodities.py**

- Script responsável por coletar a **última cotação** de commodities em USD, no intervalo de 1 minuto.
- Fonte: **Yahoo Finance** via biblioteca `yfinance`.
- Lista de ativos incluídos por padrão:
  - `GC=F` — Ouro
  - `CL=F` — Petróleo WTI
  - `SI=F` — Prata
- Retorna um **DataFrame padronizado** com as colunas:
  - `ativo` — símbolo do ativo
  - `preco` — preço atual
  - `moeda` — moeda de cotação (USD)
  - `horario_coleta` — horário local da coleta
- Pode ser executado de forma independente (`python GetCommodities.py`) para teste.

---

### **GetPrices.py**

- Script orquestrador que combina os resultados de **GetBitcoin** e **GetCommodities**.
- Três variações disponíveis:
  1. **Execução única** — junta e imprime o DataFrame.
  2. **Loop infinito** — coleta e imprime a cada 60 segundos.
  3. **Loop infinito com salvamento** — coleta a cada 60 segundos e **salva/append** em um arquivo CSV consolidado (`cotacoes.csv`).

---

## 🚀 Como Executar

1. **Instalar dependências:**

```
pip install pandas yfinance requests
```

2. **Rodar a coleta de Bitcoin:**

```
python GetBitcoin.py
```

3. **Rodar a coleta de Commodities:**

```
python GetCommodities.py
```

4. **Rodar a coleta consolidada (exemplo com salvamento a cada 60s):**

```
python GetPrices_loop_save.py
```

---

## 📊 Objetivo Futuro

Os dados coletados serão utilizados para:

- Calcular **KPIs diários** como lucro/prejuízo.
- Avaliar variação de preços.
- Criar dashboards de acompanhamento.

---

## ℹ️ Mais Informações

Este projeto faz parte das aulas do curso **Jornada de Dados**. [www.suajornadadedados.com.br]
