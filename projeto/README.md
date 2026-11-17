# Projeto: Simulação Molecular de Proteínas em Água com Diferentes Forças Iônicas

## 1. Objetivo

Investigar, por dinâmica molecular clássica, como a **temperatura** e a **força iônica da solução** influenciam a estabilidade estrutural, a flexibilidade e as interações por ligações de hidrogênio de pequenas proteínas em água, e relacionar esses efeitos com a **carga elétrica** da proteína e sua dependência com o **pH**.

## 2. Proteínas disponíveis

Cada grupo deverá estudar uma das proteínas abaixo:

| Grupo | PDB      | Proteína            | Candidat@ |
| ----- | -------- | ------------------- | --------- |
| 1     | **1L2Y** | Trp-cage            |           |
| 2     | **1CRN** | Crambin             |           |
| 3     | **1VII** | Villin headpiece    |           |
| 4     | **1E0L** | WW domain           |           |
| 5     | **1PGB** | Protein G B1 domain |           |
| 6     | **1UBQ** | Ubiquitin           |           |

## 3. Condições de simulação

As simulações devem ser realizadas em **OpenMM/Python** com:

| Item                 | Valor                 |
| -------------------- | --------------------- |
| Campo de força       | `amber14-all.xml`     |
| Água                 | `amber14/tip3pfb.xml` |
| Modelo de água       | TIP3P-FB              |
| Temperatura          | 300 K e 400 K         |
| pH                   | 7                     |
| Pressão              | 1 atm                 |
| Ensemble de produção | NPT                   |
| Tempo de produção    | 10 ns                 |
| Passo de integração  | 2 fs                  |
| Eletrostática        | PME                   |

Cada grupo deverá simular os quatro sistemas abaixo:

| Sistema | T (K) | Força iônica (mol/L)        | Usado na comparação de    |
| ------- | ----: | --------------------------- | ------------------------- |
| A       |   300 | 0,00 (apenas neutralização) | força iônica; temperatura |
| B       |   300 | 0,15                        | força iônica              |
| C       |   300 | 0,50                        | força iônica              |
| D       |   400 | 0,00 (apenas neutralização) | temperatura               |

Total: 4 simulações × 10 ns = 40 ns de produção por grupo.

> ⚠️ Use *checkpoints* e grave os arquivos no Google Drive (Aula 07): uma simulação de 10 ns pode não caber numa única sessão do Colab.

## 4. Workflow mínimo

Cada grupo deverá:

1. Baixar a estrutura da proteína no **RCSB PDB**.
2. Preparar a estrutura:
	- remover moléculas não desejadas (águas do cristal, ligantes, íons);
	- corrigir átomos/resíduos faltantes, se necessário;
	- **remover todos os hidrogênios** presentes no arquivo (estruturas de RMN já vêm com hidrogênios);
	- adicionar os hidrogênios com `Modeller.addHydrogens(forcefield, pH=7.0)`.
3. Calcular a **curva de titulação** da proteína (seção 5.1).
4. Solvatar a proteína em água TIP3P-FB.
5. Adicionar íons para neutralizar e ajustar a força iônica.
6. Minimizar a energia.
7. Equilibrar o sistema em NVT e depois em NPT.
8. Rodar **10 ns de produção em NPT** para cada sistema (A a D).
9. Analisar as trajetórias com **MDTraj**.

## 5. Análises obrigatórias

### 5.1 Carga da proteína em função do pH (sem simulação)

1. **Pelo OpenMM:** para valores de pH entre 1 e 14 (passo de 0,5), prepare a proteína com `addHydrogens(forcefield, pH=...)` e calcule a carga líquida somando as cargas parciais do campo de força (como na Aula 07).
2. **Por Henderson–Hasselbalch:** calcule a carga líquida contínua

$$
Q(\text{pH}) = \sum_{\text{básicos}} \frac{1}{1 + 10^{\,\text{pH} - \text{p}K_a}} \;-\; \sum_{\text{ácidos}} \frac{1}{1 + 10^{\,\text{p}K_a - \text{pH}}},
$$

incluindo os terminais N e C, Asp, Glu, His, Cys livres, Tyr, Lys e Arg. Cisteínas em pontes dissulfeto **não** são tituláveis. Use p*K*a de referência e cite a fonte. Valores típicos:

| Grupo     | Terminal C | Asp | Glu | His | Cys | Terminal N | Tyr  | Lys  | Arg  |
| --------- | ---------: | --: | --: | --: | --: | ---------: | ---: | ---: | ---: |
| p*K*a     | 3,1 | 3,9 | 4,1 | 6,0 | 8,3 | 8,0 | 10,1 | 10,5 | 12,5 |
| Tipo      | ácido | ácido | ácido | básico | ácido | básico | ácido | básico | básico |

3. Faça um único gráfico com as duas curvas, estime o **ponto isoelétrico** (pI) pela curva de Henderson–Hasselbalch e compare com o pI experimental da literatura.

### 5.2 Análises das trajetórias

| Análise                    | O que avaliar                                     |
| -------------------------- | ------------------------------------------------- |
| **RMSD**                   | Estabilidade global da proteína                   |
| **RMSF**                   | Flexibilidade por resíduo                         |
| **Raio de giro**           | Compactação da proteína                           |
| **Ligações de hidrogênio** | Estabilidade interna e/ou interação proteína–água |

Os gráficos devem comparar:
	- as duas temperaturas com a mesma força iônica (A × D);
	- as três forças iônicas na mesma temperatura (A × B × C).

## 6. Resultados esperados

O grupo deverá apresentar:

1. gráfico de **carga líquida × pH** (OpenMM e Henderson–Hasselbalch), com o pI estimado;
2. gráfico de **RMSD × tempo**;
3. gráfico de **RMSF × resíduo**;
4. gráfico de **raio de giro × tempo**;
5. análise de **ligações de hidrogênio**;
6. tabela comparando as três forças iônicas.

Tabelas sugeridas:

| Proteína | Nº de resíduos ácidos | Nº de resíduos básicos | Carga em pH 7 | pI (Henderson–Hasselbalch) | pI experimental |
| -------- | --------------------: | ---------------------: | ------------: | -------------------------: | --------------: |
|          |                       |                        |               |                            |                 |

| Força iônica (300 K) | RMSD médio | RMSF médio | Rg médio | Nº médio de H-bonds |
| -------------------: | ---------: | ---------: | -------: | ------------------: |
|           0,00 mol/L |            |            |          |                     |
|           0,15 mol/L |            |            |          |                     |
|           0,50 mol/L |            |            |          |                     |

## 7. Discussão biológica

Ao final, o grupo deverá pesquisar e apresentar:
- o que é a proteína estudada;
- em qual organismo ou contexto ela aparece;
- qual sua função conhecida e em que ambiente (pH, meio) ela atua;
- se está relacionada a algum bioprocesso;
- por que essa proteína é interessante como sistema de simulação molecular.

## 8. Entregáveis

Cada grupo deverá entregar:
1. scripts Python de simulação;
2. scripts Python de análise;
3. gráficos e tabelas-resumo;
4. apresentação final de **15 minutos**.

## 9. Perguntas orientadoras

A conclusão deve responder:
1. Qual é a carga da proteína em pH 7? Ela é muito ou pouco carregada?
2. As curvas de titulação do OpenMM e de Henderson–Hasselbalch concordam? Por que o OpenMM gera uma curva em degraus, e quais grupos ele não titula?
3. Em que faixa de pH a proteína seria retida numa resina de troca catiônica? E numa de troca aniônica?
4. A proteína permaneceu estável durante 10 ns?
5. A temperatura afeta a estrutura de proteína?
6. A força iônica alterou o RMSD? O efeito é compatível com a carga da proteína em pH 7?
7. Quais regiões foram mais flexíveis?
8. A proteína ficou mais compacta ou expandida?
9. As ligações de hidrogênio foram preservadas?
10. Os resultados têm relação com a função biológica da proteína?

## 10. Observações importantes

- As simulações de **10 ns** são curtas. Portanto, as conclusões devem ser apresentadas como tendências observadas na janela simulada, e não como conclusões definitivas sobre estabilidade termodinâmica.
- Na simulação, os estados de protonação são **fixos**, escolhidos no início a partir de p*K*a de referência. Na proteína real, o p*K*a de cada resíduo depende do seu ambiente, e os prótons trocam continuamente com o solvente.