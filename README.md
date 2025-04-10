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

Comentário: É impossível não destacar a grandiosidade de Marc Márquez com impressionantes 4101 pontos, ele está muito à frente dos demais. Isso mostra não só sua consistência ao longo das temporadas, mas também o domínio absoluto que ele teve por anos no MotoGP. 

Já Maverick Viñales e Johann Zarco, mesmo com menos títulos que outros nomes, apresentam uma pontuação sólida, reflexo de regularidade e presença constante nas corridas. A constância sem necessariamente vencer campeonatos também constrói uma carreira respeitável.

---

### 📌 Consulta 2  
**Lista de todas as motos (modelos) que participaram do campeonato e calcular a eficiência média de pontuação para cada uma delas, ordenando da mais eficiente para a menos eficiente.**

**Responde à pergunta:**  
*"Qual é a moto que mais gera pontos, em média, por corrida nas temporadas?"*

- Compara desempenho médio por fabricante  
- Ver tendências de eficiência ao longo dos anos  
- Suportar análises de desempenho técnico e até de investimentos em escuderias

  ![image](https://github.com/user-attachments/assets/6f8511fc-61ba-4a29-8d15-6488aa9f81de)

Comentário: Essa consulta revela um panorama sobre o verdadeiro desempenho dos modelos de moto no MotoGP ao longo das temporadas. O que mais chama atenção, pessoalmente, é o domínio impressionante da Ducati, com o modelo Desmosedici GP25 disparado na frente com uma média de eficiência de 37 – quase o dobro da segunda colocada, a GP23. Isso mostra como a Ducati vem investindo pesado em inovação e performance nos últimos anos.


---

### 📌 Consulta 3  
**Retorna os 10 pilotos com o maior número de temporadas distintas no campeonato MotoGP**

- Ver quem são os pilotos mais experientes  
- Avaliar longevidade na carreira

  ![image](https://github.com/user-attachments/assets/2283fd10-8b56-4f14-bd04-73da88a1933b)

Marc Márquez, com 18 temporadas um número que reforça o status de lenda viva da modalidade. Ele continua sendo um nome de peso no grid, o que mostra não só talento, mas uma força mental absurda.

Logo atrás, nomes como Johann Zarco, Jack Miller e Miguel Oliveira também impressionam com 15 ou mais temporadas, mostrando uma geração sólida que se manteve competitiva por anos.
---

### 📌 Consulta 4  
**Mostra as 10 equipes com mais pontos somados ao longo de todas as temporadas do MotoGP**

- Avaliar o desempenho histórico das equipes  
- Comparar investimento x resultado  
- Usar em relatórios e dashboards de análise de desempenho da competição

  ![image](https://github.com/user-attachments/assets/1925bb34-2b9c-41f4-80aa-ad3c9d80f355)

O domínio da Red Bull KTM Ajo no topo com 3626 pontos é impressionante. Ela supera com folga nomes históricos como Repsol Honda e Ducati Lenovo. Isso mostra que, embora muitas vezes menos "midiática", a Red Bull KTM Ajo tem sido extremamente eficiente, provavelmente por sua forte presença em categorias como Moto2 e Moto3, formando talentos desde cedo.


---

### 📌 Consulta 5  
**Quais modelos de motos conquistaram o maior número de pole positions ao longo do tempo**

- Comparar eficiência das fabricantes (Yamaha vs Honda vs Ducati…)  
- Identificar tendências por temporada  
- Apoiar decisões em análises de desempenho técnico dos veículos

  ![image](https://github.com/user-attachments/assets/5e9183e6-66e3-4657-9f64-290d9171da22)

Ver a Kalex liderando com 66 poles mostra como ela reina absoluta na Moto2. Ela é consistentemente dominante em uma volta rápida, o que é o coração da classificação. Logo depois, Honda RC213V (64) e Ducati (60) sustentam sua fama com números sólidos, mas o mais curioso é ver a Ducati Desmosedici GP25.


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

Destaca os principais nomes com mais títulos acumulados no dataset, com Johann Zarco e Marc Márquez no topo, ambos com 18 títulos. A listagem ainda inclui pilotos relevantes da geração atual como Bagnaia, Quartararo, Binder e o jovem Pedro Acosta, que já aparece com 5 títulos mesmo em início de carreira. 


---

### 📌 Consulta 7  
**Retorna a temporada mais forte (em pontos) de cada piloto do histórico da MotoGP, mostrando também a eficiência por corrida, e ordenando pelo total de pontos**

- Ver qual foi o auge de performance de cada piloto  
- Comparar pilotos em melhores temporadas individuais (quem chegou mais longe em um ano só)  
- Analisar eficiência: quem soube pontuar mais por corrida, mesmo com menos provas

  ![image](https://github.com/user-attachments/assets/af2d07b2-f937-49d9-9610-206e5f92fe13)

Ela mostra, com precisão, quem realmente entregou resultado ao longo de uma temporada inteira, e não só em vitórias pontuais.

Jorge Martin em 2024 é, sem dúvidas, o maior destaque. 508 pontos e uma eficiência absurda de 25,40 por corrida mostram uma temporada praticamente impecável. Mais impressionante ainda é ver que ele superou Francesco Bagnaia, que também teve um desempenho brilhante com 498 pontos. Esses dois transformaram 2024 em um dos duelos mais fortes da era recente.

Marc Márquez em 2019 ainda impressiona. Mesmo em uma era anterior à atual dominância da Ducati, ele marcou 420 pontos com apenas 19 corridas, mantendo uma média de 22,11 — isso mostra o quanto ele foi dominante naquela fase da Honda. 


Autoavaliação:

O desenvolvimento deste projeto representou uma oportunidade concreta de aplicar os conhecimentos adquiridos em engenharia de dados em um cenário com propósito analítico claro. Desde o carregamento e tratamento do dataset até a modelagem dimensional e a execução de consultas, cada etapa exigiu atenção aos detalhes e compreensão do ciclo completo de um pipeline de dados.

Optei por utilizar o Databricks e modelar os dados no formato de esquema estrela, considerando o volume e o tipo de análise desejada. A estruturação em tabelas dimensionais e fato facilitou a criação de consultas otimizadas e a aplicação de métricas analíticas como eficiência por corrida, taxa de vitórias e taxa de pódios. Essas métricas foram fundamentais para responder perguntas de negócio com profundidade e objetividade.

Durante o processo, pude aprimorar minha habilidade em manipular dados utilizando Python e SQL, além de reforçar boas práticas como padronização de tipos, eliminação de duplicatas e organização semântica das colunas. Também percebi a importância de documentar com mais precisão cada etapa técnica, para tornar o projeto mais claro e reprodutível por terceiros.

Link do meu Databricks: https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/1660807777469565/254179631858457/7016163473829090/latest.html
---

