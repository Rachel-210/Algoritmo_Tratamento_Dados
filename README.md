# Pipeline de Limpeza e Tratamento de Dados — Empresa Efatá

Este repositório contém o script de tratamento de dados e tratamento de anomalias desenvolvido em Python/Pandas no ambiente Google Colab para a base de dados comercial da **Empresa Efatá**.

---

## 📌 Visão Geral do Projeto

A base de dados inicial continha falhas críticas de formatação, ruídos estruturais, inconsistências de tipo de dados e registros duplicados ou extremos (outliers). O objetivo deste projeto foi construir um pipeline auditável e automatizado capaz de sanitizar a base de dados bruta de **547 linhas** e produzir uma base limpa com **485 registros válidos**.

---

## 🚀 Tecnologias Utilizadas

* **Linguagem:** Python 3.x
* **Biblioteca Principal:** `pandas`
* **Ambiente de Execução:** Google Colab / Jupyter Notebook
* **Fonte de Dados:** Arquivo Excel (`Empresa_efata.xlsx`)

---

## 📊 Mapeamento de Problemas

| Coluna / Item | Problema Identificado |
| :--- | :--- |
| `data` | Múltiplos formatos (`07.10.2026`, ISO, extensão), separadores inconsistentes e valores nulos. |
| `nome_cliente` | Variações de caixa (*upper/lower*), múltiplos espaços, caracteres invisíveis (`\u00a0`) e registros vazios. |
| `produto` | Erros de digitação (*typos* como "tmepero"), caixas altas e espaços excedentes. |
| `quantidade` | Números por extenso ("um"), valores zerados/negativos, *outliers* e valores em formato string. |
| `valor_unitario` | Mistura de texto com moeda (`R$`), vírgulas no lugar de pontos e *outliers* extremos ($3.000,00$ a $15.000,00$). |
| **Geral** | Duplicidades exatas e aproximadas na base. |

---

## ⚙️ Etapas de Solução (Pipeline ETL)

1. **Importação e Carga:** Leitura do arquivo original via Google Drive e análise estatística preliminar.
2. **Padronização Temporal:** Uniformização dos separadores para `/` e conversão controlada via `pd.to_datetime(errors='coerce')`.
3. **Higienização de Strings:** Aplicação de `.str.lower()`, remoção de espaços invisíveis e redução de múltiplos espaços internos.
4. **Tratamento Numérico:** Conversão de números por extenso para numérico, remoção de caracteres de moeda (`R$`) e coerção de tipos (`Int64` e `float64`).
5. **Filtragem de Outliers & Anomalias:** Aplicação de filtros para remover valores menores/iguais a zero e cálculo do método **IQR (Amplitude Interquartil)** para expurgar preços irreais.
6. **Deduplicação:** Eliminação de registros idênticos mantendo a primeira ocorrência (`drop_duplicates(keep='first')`).
7. **Auditoria Cruzada:** Validação com a aba `gabarito_erros` da planilha, confirmando a correção completa de **228 anomalias**.

---

## 📈 Resultados Alcançados

* **Registros Brutos:** 547 linhas
* **Anomalias Catalogadas e Resolvidas:** 228 falhas
* **Base Final Tratada:** 485 registros válidos e prontos para análise de dados / BI