# Projeto: Simulação Molecular de Proteínas em Água com Diferentes Forças Iônicas

## 1. Objetivo

Investigar, por dinâmica molecular clássica, como a **força iônica da solução** influencia a estabilidade estrutural, a flexibilidade e as interações por ligações de hidrogênio de pequenas proteínas em água.

## 2. Proteínas disponíveis

Cada grupo deverá estudar a sua proteína correspondente à tabela abaixo:

| Grupo | Nomes | PDB | Proteína |
|---|---|---|---|
| 1 | Davi T., Vinicius, Andre | **1PGB** | Protein G B1 domain |
| 2 | Davi B., Larissa, Mateus | **1CRN** | Crambin |
| 3 | Maira, Patrícia, Bernardo, Davidson | **1UBQ** | Ubiquitin |

<!-- | Grupo | PDB | Proteína |
|---|---|---|
| 1 | **1L2Y** | Trp-cage |
| 2 | **1CRN** | Crambin |
| 3 | **1VII** | Villin headpiece |
| 4 | **1E0L** | WW domain |
| 5 | **1PGB** | Protein G B1 domain |
| 6 | **1UBQ** | Ubiquitin | -->

## 3. Condições de simulação

As simulações devem ser realizadas em **OpenMM/Python** com:

| Item | Valor |
|---|---|
| Campo de força | `amber14-all.xml` |
| Água | `amber14/tip4pew.xml` |
| Modelo de água | TIP4P-Ew |
| Temperatura | 300 K |
| Pressão | 1 atm |
| Ensemble de produção | NPT |
| Tempo de produção | 10 ns |
| Passo de integração | 2 fs |
| Eletrostática | PME |

Cada grupo deverá simular a proteína em três forças iônicas:

| Sistema | Força iônica |
|---|---:|
| A | 0,00 mol/L ou apenas neutralização |
| B | 0,15 mol/L |
| C | 0,50 mol/L |

## 4. Workflow mínimo

Cada grupo deverá:

1. Baixar a estrutura da proteína no **RCSB PDB**.
2. Preparar a estrutura:
   - remover moléculas não desejadas;
   - corrigir átomos/resíduos faltantes, se necessário;
   - adicionar hidrogênios;
   - definir protonação em pH aproximadamente 7.
3. Solvatar a proteína em água TIP4P-Ew.
4. Adicionar íons para neutralizar e ajustar a força iônica.
5. Minimizar a energia.
6. Equilibrar o sistema em NVT e depois em NPT.
7. Rodar **10 ns de produção em NPT** para cada força iônica.
8. Analisar as trajetórias com **MDTraj**.

OBS: Cada integrante do grupo deverá gerar um arquivo de trajetória e o grupo deverá apresentar os dados de cada trajetória independente. 

## 5. Análises obrigatórias

Cada grupo deverá calcular e discutir:

| Análise | O que avaliar |
|---|---|
| **RMSD** | Estabilidade global da proteína |
| **RMSF** | Flexibilidade por resíduo |
| **Raio de giro** | Compactação da proteína |
| **Ligações de hidrogênio** | Estabilidade interna e/ou interação proteína–água |

Os gráficos devem comparar as três forças iônicas sempre que possível.

## 6. Resultados esperados

O grupo deverá apresentar:

1. gráfico de **RMSD × tempo**;
2. gráfico de **RMSF × resíduo**;
3. gráfico de **raio de giro × tempo**;
4. análise de **ligações de hidrogênio**;
5. tabela comparando as três forças iônicas.

Tabela sugerida:

| Força iônica | RMSD médio | RMSF médio | Rg médio | Nº médio de H-bonds |
|---:|---:|---:|---:|---:|
| 0,00 mol/L |  |  |  |  |
| 0,15 mol/L |  |  |  |  |
| 0,50 mol/L |  |  |  |  |

## 7. Discussão biológica

Ao final, o grupo deverá pesquisar e apresentar:

- imagem da estrutura da proteína gerada no VMD;
- o que é a proteína estudada;
- em qual organismo ou contexto ela aparece;
- qual sua função conhecida;
- se está relacionada a algum bioprocesso;
- por que essa proteína é interessante como sistema de simulação molecular.

## 8. Entregáveis

Cada _integrante do grupo_ deverá entregar:

1. Notebook Jupyter de simulação MD;
2. Notebook Jupyter de análise com gráficos e tabela-resumo;

O _grupo_ deverá entregar:
1. Apresentação final de **10 a 12 minutos**.

## 9. Perguntas orientadoras

A conclusão deve responder:

1. A proteína permaneceu estável durante 10 ns?
2. A força iônica alterou o RMSD?
3. Quais regiões foram mais flexíveis?
4. A proteína ficou mais compacta ou expandida?
5. As ligações de hidrogênio foram preservadas?
6. Os resultados têm relação com a função biológica da proteína?

## 10. Observação importante

As simulações de **10 ns** são curtas. Portanto, as conclusões devem ser apresentadas como tendências observadas na janela simulada, e não como conclusões definitivas sobre estabilidade termodinâmica.

**DATA DA APRESENTAÇÃO**: 14/7/2026 - 9hs às 12hs
