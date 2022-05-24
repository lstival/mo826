# mo826
Trabalhos da disciplina mo826 (Ciência e Visualização de Dados em Saúde)

# Apresentação

O presente projeto foi originado no contexto das atividades da
disciplina de pós-graduação Ciência e Visualização de Dados em Saúde,
oferecida no primeiro semestre de 2022, na Unicamp.

<div id="tab:integrantes">

| **Integrantes do Projeto** |        |                    |
|:---------------------------|:------:|:------------------:|
| **Nome**                   | **RA** | **Especialização** |
| Fabrício Ferreira da Silva | 231900 |     Computação     |
| Leandro Stival             | 263013 |     Computação     |

</div>

<span id="tab:integrantes" label="tab:integrantes"></span>

Código disponível em
<https://github.com/lstival/mo826/blob/main/P2.ipynb>

# Contextualização da Proposta

O objetivo desse trabalho foi desenvolver um algoritmo capaz de
responder a seguinte pergunta: O paciente irá morrer dentro de 15 dias
após um motivo de encontro de alta letalidade? Para responder essa
pergunta, o grupo desenvolveu um algoritmo de predição utilizando duas
bases de dados fornecidas pela disciplina, uma com 1121 e outra com 1174
pacientes antes da seleção das amostras e a realização do
pré-processamento, que mostram diversos dados dos pacientes como e ID,
idade, endereço, se já morreu, entre outros; e outras duas bases que
mostram os encontros desses pacientes, a data, o motivo, entre outros.

Foram selecionados para o treinamento modelo aqueles pacientes que o
motivo do encontro foi um dos 10 mais letais. Foram considerados os 10
motivos mais letais, pois os demais possuíam poucos casos de morte,
assim apresentando baixa atratividade como candidatos a possuir amostras
da classe alvo. Outro ponto de destaque neste trabalho é a variável
utilizada para predição, onde os resultados dos testes apontaram que
utilizando somente a idade desses pacientes melhorou o algoritmo de
predição, agregando mais uma variável para ajudar nesse processo os
resultados se mostraram inferiores.

Complementando esse relatório, está sendo disponibilizado através do
Google Classroom um repositório do GitHub contendo todo o código
utilizado com a metodologia descrita e os resultados apresentados.

## Ferramentas

Foram usadas as seguintes ferramentas durante o desenvolvimento do
projeto em python:

-   Pandas: biblioteca python para manipular os dados fornecidos.

-   Numpy: biblioteca python para manipular arrays e para cálculos
    matemáticos.

-   Sklearn: biblioteca python de machine learning usada na predição.

-   xgboost: biblioteca python de machine learning usada na predição.
