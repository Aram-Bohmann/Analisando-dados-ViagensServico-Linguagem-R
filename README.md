# 📊 Análise de Dados - Viagens a Serviço do Setor Público

[![R](https://img.shields.io/badge/R-276DC3?style=for-the-badge&logo=r&logoColor=white)](https://www.r-project.org/)
[![RStudio](https://img.shields.io/badge/RStudio-75AADB?style=for-the-badge&logo=rstudio&logoColor=white)](https://posit.co/)
[![dplyr](https://img.shields.io/badge/dplyr-1.1.0-blue?style=for-the-badge)](https://dplyr.tidyverse.org/)
[![ggplot2](https://img.shields.io/badge/ggplot2-3.4.0-orange?style=for-the-badge)](https://ggplot2.tidyverse.org/)

> **Análise exploratória de dados abertos de viagens a serviço**  
> Projeto desenvolvido no curso "Análise de Dados em Linguagem R" - Enap (2025)

Análise completa de gastos com viagens a serviço do setor público brasileiro em 2019, utilizando dados abertos do Portal da Transparência. O projeto demonstra técnicas de ETL, análise exploratória e visualização de dados com R.

---

## 📖 Sobre o Projeto

Este repositório contém uma análise de dados completa desenvolvida em **Linguagem R**, focada em entender e otimizar os gastos com viagens a serviço no setor público brasileiro.

### 🎯 Contexto

Com milhões de reais gastos anualmente em viagens a serviço, é fundamental:
- 📊 Entender **onde** o dinheiro público está sendo investido
- 🏢 Identificar **quais órgãos** gastam mais
- 🌍 Mapear **quais destinos** são mais frequentes
- 📅 Analisar **padrões temporais** de viagens
- 💰 Subsidiar **decisões estratégicas** para redução de custos

---

## 🎯 Definição do Problema

### Objetivo Principal
Analisar os gastos com viagens a serviço do setor público para identificar oportunidades de otimização e redução de custos.

### Perguntas de Pesquisa

| # | Pergunta | Tipo de Análise |
|---|----------|-----------------|
| 1️⃣ | **Qual é o valor gasto por órgão?** | Análise Descritiva |
| 2️⃣ | **Qual é o valor gasto por cidade?** | Análise Geográfica |
| 3️⃣ | **Qual é a quantidade de viagens por mês?** | Análise Temporal |

---

## 📊 Metodologia

### Pipeline de Análise
```
📥 Obtenção dos Dados
    ↓
🔍 Exploração Inicial (EDA)
    ↓
🧹 Limpeza e Transformação
    ↓
📈 Análise Descritiva
    ↓
📊 Visualização de Resultados
    ↓
💡 Insights e Conclusões
```

### Técnicas Utilizadas

#### 1️⃣ Obtenção dos Dados
```r
# Carregamento com parâmetros customizados
viagens <- read.csv(
  file = "2019_Viagem.csv",
  sep = ';',      # Separador brasileiro
  dec = ','       # Decimal brasileiro
)
```

#### 2️⃣ Exploração Inicial
- ✅ Análise de dimensões (`dim()`)
- ✅ Estatísticas descritivas (`summary()`)
- ✅ Verificação de tipos (`glimpse()`)
- ✅ Identificação de valores ausentes (`is.na()`)

#### 3️⃣ Transformação de Dados
```r
# Conversão de datas
viagens$data.inicio <- as.Date(viagens$Período, "%d/%m/%Y")

# Formatação para análise temporal
viagens$data.inicio.formatada <- format(viagens$data.inicio, "%Y-%m")
```

#### 4️⃣ Análise Estatística
- 📊 Histogramas de distribuição
- 📦 Boxplots para outliers
- 📈 Cálculo de desvio padrão
- 🔢 Análise de frequências

---

## 📈 Principais Resultados

### 1️⃣ Gastos por Órgão (Top 15)

**Análise:** Identificação dos 15 órgãos com maiores gastos em passagens.
```r
# Agregação e ordenação
p1 <- viagens %>%
  group_by(Nome.do.órgão.superior) %>%
  summarise(valor = sum(Valor.passagens)) %>%
  arrange(desc(valor)) %>%
  top_n(15)
```

**Visualização:**
- Gráfico de barras horizontal
- Ordenado por valor decrescente
- Identificação clara dos maiores gastadores

**Insights Esperados:**
- 🏛️ Órgãos federais tendem a gastar mais
- 💼 Ministérios com atuação nacional lideram
- 📊 Distribuição desigual de gastos

---

### 2️⃣ Gastos por Cidade (Top 15)

**Análise:** Mapeamento dos destinos mais custosos.
```r
# Agregação por destino
p2 <- viagens %>%
  group_by(Destinos) %>%
  summarise(valor = sum(Valor.passagens)) %>%
  arrange(desc(valor)) %>%
  top_n(15)
```

**Visualização:**
- Gráfico de barras com rótulos de valores
- Cor customizada (#0ba791)
- Destinos ordenados por gasto

**Insights Esperados:**
- ✈️ Capitais concentram maiores gastos
- 🌆 Brasília como hub central
- 💰 Correlação entre distância e custo

---

### 3️⃣ Quantidade de Viagens por Mês

**Análise:** Padrão temporal de viagens ao longo de 2019.
```r
# Série temporal
p3 <- viagens %>%
  group_by(data.inicio.formatada) %>%
  summarise(qtd = n_distinct(Identificador.do.processo))
```

**Visualização:**
- Gráfico de linha temporal
- Pontos marcando cada mês
- Identificação de sazonalidade

**Insights Esperados:**
- 📅 Picos em determinados meses
- 🏖️ Reduções em períodos de recesso
- 🔄 Padrões cíclicos

---

## 🔍 Análise Exploratória Detalhada

### Estatísticas Descritivas
```r
# Resumo completo
summary(viagens$Valor.passagens)

# Min.   1st Qu.  Median    Mean  3rd Qu.    Max. 
# 0.00   XXX.XX   XXX.XX  XXX.XX  XXX.XX  XXXXX.XX

# Desvio padrão
sd(viagens$Valor.passagens)
```

### Tratamento de Outliers
```r
# Filtro de valores extremos
passagens_filtro <- viagens %>%
  select(Valor.passagens) %>%
  filter(Valor.passagens >= 200 & Valor.passagens <= 5000)
```

**Justificativa:**
- Valores muito baixos podem ser erros
- Valores muito altos podem distorcer análises
- Foco na distribuição central

### Análise de Situação
```r
# Categorização
viagens$Situação <- factor(viagens$Situação)

# Distribuição percentual
prop.table(table(viagens$Situação)) * 100
```

---

## 📊 Visualizações

### Pacotes Utilizados
```r
library(dplyr)      # Manipulação de dados
library(ggplot2)    # Visualizações profissionais
```

### Exemplos de Gráficos

#### Histograma de Distribuição
```r
# Distribuição de valores de passagens
hist(viagens$Valor.passagens,
     main = "Distribuição de Valores de Passagens",
     xlab = "Valor (R$)",
     ylab = "Frequência")
```

#### Boxplot de Outliers
```r
# Identificação visual de outliers
boxplot(viagens$Valor.passagens,
        main = "Análise de Outliers",
        ylab = "Valor (R$)")
```

#### Gráfico de Barras (ggplot2)
```r
# Visualização profissional
ggplot(p1, aes(x = reorder(orgao, valor), y = valor)) +
  geom_bar(stat = "identity", fill = "#0ba791") +
  coord_flip() +
  labs(title = "Gastos por Órgão",
       x = "Órgão",
       y = "Valor Total (R$)") +
  theme_minimal()
```

---

## 🗂️ Estrutura do Dataset

### Colunas Principais

| Coluna | Tipo | Descrição |
|--------|------|-----------|
| `Nome.do.órgão.superior` | Character | Órgão responsável pela viagem |
| `Destinos` | Character | Cidade(s) de destino |
| `Valor.passagens` | Numeric | Custo das passagens (R$) |
| `Período...Data.de.início` | Character | Data de início (dd/mm/yyyy) |
| `Identificador.do.processo` | Character | ID único da viagem |
| `Situação` | Factor | Status da viagem |

### Dados Ausentes
```r
# Verificação de NAs
colSums(is.na(viagens))
```

---

## 🚀 Como Executar

### Pré-requisitos
```r
# R 4.0+ instalado
R.version

# RStudio Desktop (recomendado)
# Download: https://posit.co/download/rstudio-desktop/
```

### Instalação de Pacotes
```r
# Instalar pacotes necessários
install.packages("dplyr")
install.packages("ggplot2")

# Carregar bibliotecas
library(dplyr)
library(ggplot2)
```

### Execução

1. **Clone o repositório**
```bash
git clone https://github.com/Aram-Bohmann/Analisando-dados-ViagensServico-Linguagem-R.git
cd Analisando-dados-ViagensServico-Linguagem-R
```

2. **Baixe o dataset**
   - Acesse: [Portal da Transparência](http://www.portaltransparencia.gov.br/viagens)
   - Período: 2019
   - Formato: CSV (separador `;`, decimal `,`)

3. **Ajuste o caminho do arquivo**
```r
viagens <- read.csv(
  file = "caminho/para/2019_Viagem.csv",
  sep = ';',
  dec = ','
)
```

4. **Execute o script**
   - Abra o arquivo `.R` no RStudio
   - Execute linha por linha ou `Ctrl + Shift + Enter`

---

## 💡 Principais Insights

### 🏛️ Concentração de Gastos
- Poucos órgãos concentram a maioria dos gastos
- Oportunidade de otimização focada

### 🌍 Padrão Geográfico
- Capitais dominam destinos
- Viagens longas custam mais
- Possibilidade de videoconferências

### 📅 Sazonalidade
- Picos em meses específicos
- Planejamento pode reduzir custos
- Identificação de períodos ociosos

---

## 🛠️ Stack Tecnológica

### Linguagem & IDE
![R](https://img.shields.io/badge/R_4.3+-276DC3?style=flat-square&logo=r&logoColor=white)
![RStudio](https://img.shields.io/badge/RStudio-75AADB?style=flat-square&logo=rstudio&logoColor=white)

### Bibliotecas Principais
![dplyr](https://img.shields.io/badge/dplyr-blue?style=flat-square)
![ggplot2](https://img.shields.io/badge/ggplot2-orange?style=flat-square)

### Funções Chave Utilizadas
- `read.csv()` - Leitura de dados
- `as.Date()` - Conversão de datas
- `group_by()`, `summarise()` - Agregação
- `filter()`, `select()` - Manipulação
- `ggplot()` - Visualização
- `hist()`, `boxplot()` - Gráficos base

---

## 🎓 Contexto Acadêmico

### Curso
**Análise de Dados em Linguagem R**  
**Instituição:** Enap (Escola Nacional de Administração Pública)  
**Ano:** 2025  
**Carga Horária:** 20 horas

### Competências Desenvolvidas

1. **📊 Análise de Dados** - EDA completa em R
2. **🧹 Limpeza de Dados** - Tratamento de missing values e outliers
3. **📈 Visualização** - Gráficos com ggplot2
4. **💻 Programação R** - Uso de tidyverse
5. **💡 Pensamento Analítico** - Extração de insights
6. **📝 Documentação** - Código comentado e README

---

## 📚 Fonte de Dados

### Portal da Transparência

🔗 **URL:** [portaltransparencia.gov.br/viagens](http://www.portaltransparencia.gov.br/viagens)

**Características:**
- 📅 **Período:** 2019
- 🔓 **Acesso:** Dados Abertos
- 📊 **Formato:** CSV
- 🇧🇷 **Escopo:** Federal
- ♻️ **Atualização:** Contínua

### Sobre os Dados Abertos

Os dados utilizados fazem parte da iniciativa de **transparência do governo federal**, seguindo princípios da **Lei de Acesso à Informação (LAI)**.

---

## 🚀 Melhorias Futuras

### Análises Adicionais

- [ ] **Análise por região** - Distribuição geográfica detalhada
- [ ] **Custo por km** - Eficiência de deslocamento
- [ ] **Análise de urgência** - Planejamento vs última hora
- [ ] **Comparação anual** - Evolução histórica (2017-2023)
- [ ] **Previsão de gastos** - Modelo preditivo com ML

### Melhorias Técnicas

- [ ] **Dashboard interativo** - Shiny App
- [ ] **Automatização** - Pipeline ETL
- [ ] **Testes estatísticos** - Significância de diferenças
- [ ] **Clustering** - Agrupamento de padrões
- [ ] **Relatório automático** - R Markdown

---

## 🤝 Contribuindo

Contribuições são bem-vindas! Este é um projeto educacional aberto.

### Como Contribuir

1. Fork o projeto
2. Crie uma branch (`git checkout -b feature/analise-adicional`)
3. Commit suas mudanças (`git commit -m 'Adiciona análise X'`)
4. Push para a branch (`git push origin feature/analise-adicional`)
5. Abra um Pull Request

### Áreas de Contribuição

- 📊 **Novas análises** - Perguntas adicionais
- 📈 **Visualizações** - Gráficos alternativos
- 🧹 **Limpeza de dados** - Tratamentos robustos
- 📝 **Documentação** - Melhorias no README
- 🐛 **Correções** - Bugs e otimizações

---

## 📝 Licença

Este projeto foi desenvolvido para fins **educacionais** e está disponível para:

✅ Uso em estudos e aprendizado  
✅ Modificação e adaptação  
✅ Distribuição com créditos  

---

## 📞 Contato

**Desenvolvedor:** Aram Bohmann Leite da Luz

[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:arambohmannleitedaluz@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/aram-luz-1b0ab1321)
[![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Aram-Bohmann)
[![Portfolio](https://img.shields.io/badge/Portfolio-FF5722?style=for-the-badge&logo=google-chrome&logoColor=white)](https://aram-bohmann.github.io/Site-Portfolio/)

---

## 🙏 Agradecimentos

- **Enap** - Pelo curso de excelência em R
- **Portal da Transparência** - Pelos dados abertos
- **Comunidade R** - pelas bibliotecas incríveis
- **RStudio/Posit** - Pela IDE profissional

---

<div align="center">

### ⭐ Se este projeto foi útil para você, considere dar uma estrela!

**Desenvolvido com 💙 e 📊 no curso Enap 2025**

*"Dados abertos para decisões inteligentes"*

</div>
