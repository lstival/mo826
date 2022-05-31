# Equipe ATOM

# Projeto 3 - Reproduzindo o Experimento de um Artigo Científico

O objetivo geral do projeto é reproduzir um experimento (total ou parcialmente) de um artigo científico. O tema do artigo deve estar relacionado a Ciência de Redes e Saúde. Poderão ser aceitos artigos cujos temas que tangenciam a área de saúde. Temas de artigos envolvendo ciência de redes em outros domínios deverão ser negociados previamente com os professores.

Recomenda-se o seguinte índice para encontrar artigos com dados de redes publicados: (https://icon.colorado.edu/#!/networks)[https://icon.colorado.edu/#!/networks]

A equipe tem a liberdade de adaptar e simplificar o experimento, conforme a disponibilidade dos dados, dos algoritmos e do grau de dificuldade na reprodução.

Para a reprodução do experimento, pode ser usada qualquer ferramenta/framework de processamento de redes complexas.

O trabalho será feito em equipes (preferencialmente as mesmas do P2).

## Instruções para o Relatório Final

Segue abaixo o modelo de como deve ser documentada a entrega.
> Tudo o que aparece neste modo de citação, ou indicado entre `<...>`, se refere a algo que deve ser substituído pelo indicado. No modelo são colocados exemplos ilustrativos, que serão substituídos pelos do seu projeto.

Se o relatório for feito em um notebook, o modelo a seguir pode ser colocado dentro do notebook diretamente. Nesse caso, coloque no markdown do projeto (fora do notebook) uma cópia dos dados até a seção de `Apresentação` e um link para o notebook com o relatório.

# Modelo Relatório Final de Projeto P3

# Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [*Ciência e Visualização de Dados em Saúde*](https://ds4h.org), oferecida no primeiro semestre de 2022, na Unicamp.

| **Integrantes do Projeto** |        |                    |
|:---------------------------|:------:|:------------------:|
| **Nome**                   | **RA** | **Especialização** |
| Fabrício Ferreira da Silva | 231900 |     Computação     |
| Leandro Stival             | 263013 |     Computação     |

# Referência bibliográfica do artigo lido
Malik, H. A. M., Mahesar, A. W., Abid, F., Waqas, A., & Wahiddin, M. R. (2017). Two-mode network modeling and analysis of dengue epidemic behavior in Gombak, Malaysia. Applied Mathematical Modelling, 43, 207-220.

https://www.sciencedirect.com/science/article/pii/S0307904X16305753

# Resumo
> Escreva um breve do artigo (com as suas palavras, não deve ser copiado texto do artigo).

# Breve descrição do experimento/análise do artigo que foi replicado
> Descreva brevemente a parte do artigo cujo experimento ou análise foi reproduzido. Explique o que foi usado como entrada e saída.

## Dados usados como entrada
Dataset | Endereço na Web | Resumo descritivo
----- | ----- | -----
Dataset of dengue patients in Selangor | [(https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6689515/pdf/hir-25-182.pdf)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6689515/pdf/hir-25-182.pdf) | Dataset contendo duas informações, a sigla das regiões na málasia com os casos de dengue e a quantidade de casos, essas informações formam um grafo bipartido, que no trabalho original foi projetado ligando as reigões quando elas possuisem a mesma quantidade de casos (dados disponiveis no Apendice 1 do artigo).

# Método
> Método usado para a análise -- adaptações feitas, ferramentas utilizadas, abordagens de análise adotadas e respectivos algoritmos.
> Etapas do processo reproduzido.

# Resultados
> Apresente os resultados obtidos pela sua adaptação.
> Confronte os seus resultados com aqueles do artigo.
> Esta seção opcionalmente pode ser apresentada em conjunto com o método.


|**Conjunto 01**|                    |   Previsto    |      Previsto      |
|:-------------:|:------------------:|:-------------:|:------------------:|
|               |   **Regressão**    | **Negativo**  |    **Positivo**    | 
| **Real**      | **Negativo**       |      13       |           1        |
| **Real**      | **Positivo**       |       2       |           5        |
|               |                    | **Previsto**  |    **Previsto**    |
|               |   **XGBoost**      | **Negativo**  |    **Positivo**    | 
|**Real**       | **Negativo**       |      10       |           4        |
|**Real**       | **Positivo**       |       2       |           5        |
