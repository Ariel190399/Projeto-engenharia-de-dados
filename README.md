# 🏁 Projeto Engenharia de Dados - MotoGP

**Nome:** Ariel Chaves Escafura  
**MVP do Sprint:** Engenharia de Dados

## 📁 Estrutura do Projeto

1. Carga e qualidade dos dados  
2. Modelagem em esquema estrela  
3. Análise exploratória  
4. Resposta às perguntas de negócio  

---

## 📄 Relatório do Projeto MotoGP

- Upload de um arquivo `.csv` com dados do campeonato MotoGP (2008–2025) via Kaggle para o **Databricks (DBFS)**.
- Criação da tabela `MotoGp`.
- Conversão da tabela temporária para permanente utilizando **Python** e **SQL**.
- Exclusão da tabela temporária original.
- Ajustes de cabeçalho e renomeação de colunas.
- Reordenação das colunas para melhor organização.
- Verificação de duplicatas com SQL.
- Conversão dos tipos de dados com `CAST`, padronizando colunas como `Pontos`, `Colocacao` e `Corridas_participadas` como `INT`.

🔍 **Motivo da escolha do esquema estrela**:  
Devido à necessidade de **consultas simples e análises rápidas**, o modelo estrela foi considerado o mais adequado.

---

## ⭐ Esquema Estrela - Modelagem Dimensional

### 📌 Tabelas Dimensão

#### `dim_piloto` — Dimensão Piloto

| Coluna            | Descrição                                              |
|-------------------|--------------------------------------------------------|
| id_piloto         | Identificador único                                    |
| nome_piloto       | Nome do piloto (ex: Marc Marquez)                      |
| numero_moto       | Número da moto (ex: 93)                                |
| pais_origem       | País de nascimento (ex: Spain, Italy)                  |
| titulos_mundiais  | Total de títulos conquistados (MAX por piloto)        |

---

#### `dim_equipe` — Dimensão Equipe

| Coluna       | Descrição                             |
|--------------|----------------------------------------|
| id_equipe    | Identificador único                    |
| nome_equipe  | Nome da equipe (ex: Repsol Honda Team) |

---

#### `dim_moto` — Dimensão Moto

| Coluna       | Descrição                               |
|--------------|------------------------------------------|
| id_moto      | Identificador único                      |
| modelo_moto  | Modelo da moto (ex: Ducati GP23)         |

---

#### `dim_tempo` — Dimensão Tempo

| Coluna   | Descrição                                 |
|----------|--------------------------------------------|
| id_tempo | Identificador único                        |
| ano      | Ano da temporada (ex: 2023)                |
| classe   | Categoria (MotoGP, Moto2, Moto3, etc.)     |

---

### 📊 Tabela Fato: `fato_temporada_piloto`

| Coluna               | Descrição                                          |
|----------------------|----------------------------------------------------|
| id_piloto            | FK → dim_piloto                                     |
| id_equipe            | FK → dim_equipe                                     |
| id_moto              | FK → dim_moto                                       |
| id_tempo             | FK → dim_tempo                                      |
| vitorias             | Vitórias na temporada                               |
| podios               | Pódios conquistados                                 |
| poles                | Pole positions conquistadas                         |
| volta_mais_rapida    | Quantidade de voltas mais rápidas                   |
| pontos               | Total de pontos na temporada                        |
| colocacao            | Posição final no campeonato                         |
| corridas_participadas| Corridas disputadas                                 |

---

## 🧮 Métricas Calculadas

| Métrica              | Fórmula                                        |
|----------------------|------------------------------------------------|
| eficiência_pontos    | Pontos / Corridas                              |
| taxa_vitorias        | (Vitórias / Corridas) * 100                    |
| taxa_podios          | (Pódios / Corridas) * 100                      |

---

## 🔍 Consultas Desenvolvidas

### ✅ Consulta 1
**Top 10 pilotos com mais pontos na história (todas temporadas e classes).**  
- nome_piloto  
- numero_moto  
- pais_origem  
- total de pontos  

---

### ✅ Consulta 2  
**Eficiência média de pontuação por modelo de moto.**  
- Ordena da mais eficiente para a menos eficiente  
- Responde: _"Qual moto gera mais pontos, em média, por corrida?"_  

---

### ✅ Consulta 3  
**Top 10 pilotos com mais temporadas distintas.**  
- Mede a **experiência e longevidade na competição**.  

---

### ✅ Consulta 4  
**Top 10 equipes com maior pontuação somada em todas as temporadas.**  
- Avalia **desempenho histórico das equipes**.  
- Apoia decisões estratégicas e comparações.  

---

### ✅ Consulta 5  
**Modelos de moto com mais pole positions acumuladas.**  
- Compara fabricantes (Honda, Ducati, Yamaha...)  
- Suporta **análises técnicas** de desempenho.  

---

### ✅ Consulta 6  
**Pilotos campeões mundiais com pelo menos 1 título.**  
- nome_piloto  
- numero_moto  
- pais_origem  
- total de títulos  

---

### ✅ Consulta 7  
**Melhor temporada (em pontos) de cada piloto.**  
- Mostra auge de performance individual  
- Calcula também a eficiência por corrida

---

## 💡 Exemplos de Perguntas Respondidas

- Qual foi o melhor piloto da temporada 2021?  
- Qual moto teve a maior média de pontos por corrida?  
- Como foi a eficiência de cada piloto por classe?  
- Qual equipe mais pontuou na história?  
- Quem são os pilotos com mais temporadas?

---

## 🛠️ Ferramentas Utilizadas

- 📊 **Databricks + DBFS**
- 🐍 **Python**
- 🧠 **SQL (Spark SQL)**
- 📁 **CSV (Kaggle Dataset)**

---

> Projeto acadêmico desenvolvido para fins de aprendizado em Engenharia de Dados.
