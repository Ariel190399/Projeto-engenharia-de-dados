# 🏍️ MVP do Sprint: Engenharia de Dados

**Nome:** Ariel Chaves Escafura

---

## 📁 Estrutura do Projeto

1. Carga e qualidade dos dados  
2. Modelagem em esquema estrela  
3. Análise exploratória  
4. Resposta às perguntas de negócio  

---

## 📄 Relatório Projeto MotoGP

- Fiz o upload através do site Kaggle, de um documento CSV para o Databricks através do DBFS, contendo dados do campeonato de motos MotoGP dos anos de 2008 até 2025.
- Criei a tabela com o nome: `"MotoGp"`.
- Alterei a tabela de temporária para permanente, utilizando **Phyton** e **SQL**.
- Excluí a tabela temporária.
- Coloquei a primeira coluna da tabela como cabeçalho e alterei seus respectivos nomes.
- Alterei a ordem das colunas.
- Fiz um comando SQL para confirmar se havia duplicatas.
- Alterei os tipos de dados das colunas com base no conteúdo real. Padronizei os tipos de dados com `CAST`, garantindo que colunas como **Pontos**, **Colocação** e **Corridas_participadas** sejam `INT`.  
  Isso evita erros futuros e melhora o desempenho das consultas.
- Optei por modelar os dados através do **esquema estrela**. Através da minha análise, cheguei à conclusão de que este esquema era a melhor escolha para dados em questão, pois irei fazer consultas simples e realizar análises rápidas.

---

## ⭐ Criação do Esquema Estrela

### 🔹 Dimensões

---

### `dim_piloto` — Dimensão Piloto  
**Objetivo:** Armazena informações únicas de cada piloto, independentemente da temporada ou equipe.

| Coluna           | Descrição                                       |
|------------------|-------------------------------------------------|
| id_piloto        | Identificador único                             |
| Nome_piloto      | Nome do piloto (ex: Marc Marquez)               |
| Numero_moto      | Número usado na moto (ex: 93)                   |
| Pais_origem      | País de nascimento (ex: Spain, Italy)           |
| Titulos_mundiais | Total de títulos conquistados na carreira (MAX) |

---

### `dim_equipe` — Dimensão Equipe  
**Objetivo:** Armazena os nomes das equipes que participaram ao longo das temporadas.

| Coluna       | Descrição                                          |
|--------------|-----------------------------------------------------|
| id_equipe    | Identificador único                                |
| nome_equipe  | Nome completo da equipe (ex: Repsol Honda Team)    |

---

### `dim_moto` — Dimensão Moto  
**Objetivo:** Registra os modelos de motos utilizadas nas corridas.

| Coluna        | Descrição                                 |
|---------------|--------------------------------------------|
| id_moto       | Identificador único                        |
| modelo_moto   | Nome do modelo (ex: Honda RC213V, Ducati GP23) |

---

### `dim_tempo` — Dimensão Tempo  
**Objetivo:** Organiza o tempo através da temporada (ano) e da classe de corrida.

| Coluna    | Descrição                                      |
|-----------|------------------------------------------------|
| id_tempo  | Identificador único                            |
| ano       | Ano da temporada (ex: 2023)                    |
| classe    | Categoria (MotoGP, Moto2, Moto3, 125cc etc.)   |

---

## 📊 `fato_temporada_piloto` — Tabela Fato  

**Objetivo:** Guarda os resultados da participação dos pilotos em cada temporada, combinando com equipe, moto e classe.

**Chaves Estrangeiras:**

- `id_piloto` → liga com `dim_piloto`  
- `id_equipe` → liga com `dim_equipe`  
- `id_moto` → liga com `dim_moto`  
- `id_tempo` → liga com `dim_tempo`  

| Coluna                | Descrição                                      |
|-----------------------|-----------------------------------------------|
| Vitorias              | Vitórias na temporada                         |
| Podios                | Pódios conquistados                           |
| Poles                 | Pole positions                                |
| Volta_mais_rapida     | Quantas vezes teve a volta mais rápida        |
| Pontos                | Total de pontos somados                       |
| Colocacao             | Colocação final no campeonato                 |
| Corridas_participadas | Quantas corridas disputou                     |

---

## 📐 Métricas calculadas

| Métrica            | Fórmula                                 |
|--------------------|------------------------------------------|
| eficiencia_pontos  | Pontos / Corridas (média de pontos)      |
| taxa_vitorias      | (Vitórias / Corridas) * 100              |
| taxa_podios        | (Pódios / Corridas) * 100                |

---

## 💡 Exemplo de uso: Responde perguntas como:

- “Qual foi o melhor piloto da temporada 2021?”  
- “Qual moto teve a maior média de pontos?”  
- “Como foi a eficiência de cada piloto por classe?”  

---

## 🔍 Consultas

---

### 📌 Consulta 1  
**Listar os 10 pilotos com mais pontos somados em toda a história do MotoGP (todas as temporadas e classes), mostrando:**

- Nome do piloto  
- Número da moto  
- País de origem  
- Total de pontos acumulados  

![image](https://github.com/user-attachments/assets/7206c399-d734-4154-b9d5-dd5aa8d92ae6)

---

### 📌 Consulta 2  
**Lista de todas as motos (modelos) que participaram do campeonato e calcular a eficiência média de pontuação para cada uma delas, ordenando da mais eficiente para a menos eficiente.**

**Responde à pergunta:**  
*"Qual é a moto que mais gera pontos, em média, por corrida nas temporadas?"*

- Compara desempenho médio por fabricante  
- Ver tendências de eficiência ao longo dos anos  
- Suportar análises de desempenho técnico e até de investimentos em escuderias

  ![image](https://github.com/user-attachments/assets/6f8511fc-61ba-4a29-8d15-6488aa9f81de)


---

### 📌 Consulta 3  
**Retorna os 10 pilotos com o maior número de temporadas distintas no campeonato MotoGP**

- Ver quem são os pilotos mais experientes  
- Avaliar longevidade na carreira

  ![image](https://github.com/user-attachments/assets/2283fd10-8b56-4f14-bd04-73da88a1933b)


---

### 📌 Consulta 4  
**Mostra as 10 equipes com mais pontos somados ao longo de todas as temporadas do MotoGP**

- Avaliar o desempenho histórico das equipes  
- Comparar investimento x resultado  
- Usar em relatórios e dashboards de análise de desempenho da competição

  ![image](https://github.com/user-attachments/assets/1925bb34-2b9c-41f4-80aa-ad3c9d80f355)


---

### 📌 Consulta 5  
**Quais modelos de motos conquistaram o maior número de pole positions ao longo do tempo**

- Comparar eficiência das fabricantes (Yamaha vs Honda vs Ducati…)  
- Identificar tendências por temporada  
- Apoiar decisões em análises de desempenho técnico dos veículos

  ![image](https://github.com/user-attachments/assets/5e9183e6-66e3-4657-9f64-290d9171da22)


---

### 📌 Consulta 6  
**Quais pilotos conquistaram títulos mundiais, quantos títulos cada um possui e de onde são?**

- Mostra os pilotos campeões mundiais  
- Total de títulos de cada um  
- Número da moto  
- País de origem  
- Filtra apenas quem tem ao menos um título  
- Ordena do mais vitorioso para o menos

  ![image](https://github.com/user-attachments/assets/098cc4e3-d123-4cfe-834b-5d5e6a03b26d)


---

### 📌 Consulta 7  
**Retorna a temporada mais forte (em pontos) de cada piloto do histórico da MotoGP, mostrando também a eficiência por corrida, e ordenando pelo total de pontos**

- Ver qual foi o auge de performance de cada piloto  
- Comparar pilotos em melhores temporadas individuais (quem chegou mais longe em um ano só)  
- Analisar eficiência: quem soube pontuar mais por corrida, mesmo com menos provas

  ![image](https://github.com/user-attachments/assets/af2d07b2-f937-49d9-9610-206e5f92fe13)


---

