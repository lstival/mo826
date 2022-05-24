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
# Metodologia

Nessa seção será apresentado como foi o desenvolvimento desse projeto
para responder à pergunta da proposta de predição.

## Base de dados

Para o desenvolvimento dos algoritmos de predição de mortalidade foram
utilizadas as bases de dados disponibilizadas pelos professores
*scenario01* e *scenario02* que serão denominados conjunto 01 e conjunto
02, onde ambas são comportas por arquivos no formato *.csv* (do inglês,
*comma separed values*) contendo informações sobre pacientes, alergias,
medicamentos entre outras informações sintéticas sobre estes pacientes
fictícios.

Os arquivos disponibilizados são originalmente pela Synthea , sendo uma
organização responsável por gerar e disponibilizar dados realístico
sintéticos sobre pacientes, permitindo a sua utilização em pesquisas
acadêmicas, indústrias e agências governamentais, as informações
consideradas relevantes para responder nossa questão de pesquisa foi
resumida a dois arquivos, sendo eles descritos abaixo:

-   **encounter.csv** Este arquivo possui informações dos encontros
    realizados do paciente com alguma instituição médica, por exemplo,
    uma internação, um exame ou uma consulta, apontando o motivo do
    encontro, acrescido dos valores das despesas e data do evento,
    permitindo assim identificar o intervalo entre o óbito de um
    paciente e data de seu último encontro.

-   **patients.csv** Contem os dados de cada paciente, como, data de
    nascimento, gênero, local de nascimento, valor de plano de saúde,
    código de diversos documentos e data de falecimento. Esse conjunto
    foi o responsável por determinar nossa variável alvo da predição
    (identificar óbito do paciente).
## Pré-Processamento

Dados brutos são os detentores de todas as informações brutas sobre os
pacientes e eventos, porém, sem o processamento adequado essas
informações não podem ser aproveitada, neste trabalho os dois conjuntos
selecionados foram tratados através da biblioteca python Pandas,
facilitando a remoção de valores nulos que optamos por remover do
conjunto, todas as operações de tratamento dos dados realizadas podem
ser resumidas em dois pontos principais, Seleção das informações e
Tipagem dos dados:

### Seleção das informações

Buscando identificar quais pacientes possuem um maior risco de óbito,
foram selecionados aqueles que ao menos uma vez tiveram em seu registro
um encontro cujo motivo fosse apresentado no top dez em mortalidade, a
lista dos motivos utilizados no conjunto 01 é apresentando na
Tabela [1][] já o conjunto 02 é representado pela
Tabela [\[tab:motivos_morte\]][2].

<div id="tab:motivos_morte_01">

| **Letalidade do conjunto 01**               |            |
|:--------------------------------------------|:----------:|
| **Motivo**                                  | **Obitos** |
| Acute myeloid leukemia disease (disorder)   |     26     |
| Myocardial Infarction                       |     23     |
| Chronic congestive heart failure (disorder) |     19     |
| Pneumonia                                   |     14     |
| Sudden Cardiac Death                        |     12     |
| Non-small cell lung cancer (disorder)       |     11     |
| Stroke                                      |     10     |
| Bullet wound                                |     7      |
| COVID-19                                    |     6      |
| Chronic obstructive bronchitis (disorder)   |     5      |

Considerando as causas de morte somente do conjunto 02 são apresentadas
motivos diferentes do conjunto 02, onde o foco não são mais doenças
cardíacas, mas sim doenças mais abragente.

</div>

  [1]: #tab:motivos_morte_01
  [2]: #tab:motivos_morte
<div id="tab:motivos_morte">

| **Letalidade do conjunto 02**               |            |
|:--------------------------------------------|:----------:|
| **Motivo**                                  | **Obitos** |
| Sudden Cardiac Death                        |     20     |
| Myocardial Infarction                       |     19     |
| Chronic congestive heart failure (disorder) |     18     |
| Acute myeloid leukemia disease (disorder)   |     13     |
| Bullet wound                                |     7      |
| Cardiac Arrest                              |     5      |
| Pulmonary emphysema (disorder)              |     4      |
| Concussion injury of brain                  |     4      |
| Pneumonia                                   |     3      |
| Stroke                                      |     3      |

Lista contendo os dez motivos de encontro com maior letalidade
considerando o conjunto 01, onde é possível constatar que doenças
cardíacas representam maioria dos casos.

</div>
Em posse das possíveis agravantes de mortalidade, foi extraída a lista
dos pacientes que atendiam ao critério de possuir ao menos um evento com
este motivo, criando assim um conjunto de 238 pacientes para o conjunto
01 e 146 para o conjunto 02.

### Tipagem dos dados

Com o conjunto de interesse criado foram realizadas algumas
transformações padrões como, conversão para o formato de data as colunas
que continham essa informação, por exemplo, data de nascimento, data de
óbito e data do encontro. Amostras que possuem dados nulos (*Nan)* nas
variáveis de interesse foram descartadas, visto que, não possuímos
conhecimento sobre os dados necessários para reconstruir essas
informações.

## Seleção das métricas

Predições de classes necessitam de informações para serem realizadas, no
contexto de aprendizado de máquina geralmente de utilizam de várias
independentes *X* para determinar o valor de uma variável dependente
*y*, neste trabalho o conjunto de várias independente foi resumida a
idade do paciente para predizer a variável alvo (óbito em até 15 dias
após um encontro).

Como a idade não é uma informação descrita diretamente nos conjuntos de
dados, ela foi estimada considerada a diferença entre a data de
nascimento do paciente e a data do início do encontro, assim podemos
descrevê-la como “idade do paciente ao dar início a um encontro”.

### Criação da variável alvo

Todo processo de classificação precisa que um rótulo seja informado, e
em algumas situações como a deste trabalho ela não explicitamente
presente no conjunto de dados sendo necessário o tratamento dos dados
para encontrá-la. Sendo assim, para identificarmos quantitativamente a
nosso alvo foram realizados os seguintes paços.

1.  Selecionado os pacientes com ao menos um encontro onde o motivo está
    presente na lista de maior taxa de letalidade apresentado na
    Tabela [\[tab:motivos_morte\]][1] ou
    Tabela [\[tab:motivos_morte_01\]][2] dependendo do conjunto.

2.  Selecionado a data do último evento do paciente.

3.  Selecionada a data de óbito do paciente, caso ela não preencha a
    classe será definida como 0.

4.  Comparar se a data de óbito é no máximo 15 dias após o último
    evento, caso seja a classe é definida como 1.

  [1]: #tab:motivos_morte
  [2]: #tab:motivos_morte_01
Lembrando que nossa classificação tem como foco encontrar as situações
de óbito em até 15 dias após o último evento de um paciente que em algum
momento já registrou um encontro motivado por umas das situações de
maior letalidade.

## Preditores

Predição de classes é geralmente solucionada por uma abordagem de
aprendizado supervisionado, como o foco deste trabalho é identificar o
óbito de pacientes, nos permitiu modelar o problema como uma
classificação binária (ocorreu o óbito após a janela estipulada ou não),
dessa forma foram implementados e treinados dois algoritmos para a
classificação das amostras a Regressão Logística e XGBoost:

### Regressão Logística

Os modelos de regressão logística recebem um conjunto de atributos
utilizados para realizar a combinação linear dos valores e aplicar uma
função sigmoide ao resultado da combinação, retornando assim valores
entre 0 e 1, onde a proximidade com esses limitantes irá definir a qual
classe a amostra pertence . Nesse trabalho foi utilizada o modulo de
regressão implementada na biblioteca Scikit-Learn (versão 0.23.2) da
linguagem de programação Python.

### XGBoost

O XGBoost trabalha com *Gradient Tree Boosting*, onde cada previsor é
uma árvore de decisão, e cada nova árvore é treinada com base no erro da
árvore treinada na iteração anterior. Em outras palavras, o algoritmo
treina de maneira sequencial um *ensemble* com diversos preditores,
tentando ajustar o próximo com base no erro residual do seu antecessor.
Neste trabalho foi utilizada a biblioteca XGBoost  da linguagem de
programação Python.

## Métricas de avaliação

### Acurácia

Mostra o percentual de amostras preditas corretamente, a métrica mais
comum para avaliar modelos, porém não é a mais eficaz para todas as
situações, principalmente por ser sensível a amostras desbalanceadas.

### Acurácia balanceada

Apresenta a acurácia considerando o valor a quantidade de amostras nas
classes, melhor métrica para conjuntos desbalanceados.

### Matriz de confusão

Permite visualizar como está disposto o erro do modelo, possibilitando
identificar “onde está ocorrendo o erro”, demonstrando se os resultados
estão favorecendo alguma classe, ou se o classificador está retornando a
mesma classe para todas as amostras.
# Resultados Obtidos

Os classificadores utilizados foram treinados de duas maneiras
distintas, somente no conjunto 01, somente no conjunto 02, e uma
terceira abordagem foi realizada onde o modelo treinado em um conjunto
foi testado no outro, visando validar a capacidade de generalização do
classificador.

## Resultados no conjunto 01

Os resultados podem ser observados na Tabela [1][], demonstrando que
nenhum dos classificadores apresentou acurácia, balanceada ou não
superior a 70%.

<div id="tab:resultados_01">

| **Resultas para o conjunto 01** |              |                         |
|:--------------------------------|:------------:|:-----------------------:|
| **Modelo**                      | **Acurácia** | **Acurácia Balanceada** |
| Regressão                       |     71%      |           66%           |
| XGBoost                         |     79%      |           75%           |

Resultados obtidos após o treinamento dos classificadores para o
conjunto 01, onde é possível observar que a acurácia balanceada
apresenta valores menores do que acurácia clássica, esse fenômeno ocorre
provavelmente devido à quantidade de elementos por cada classe não ser
balanceado.

</div>

  [1]: #tab:resultados_01
## Resultados no conjunto 02

Os resultados podem ser observados na Tabela [1][], demonstrando que o
treinamento do classificador XGBoost apresentou melhores resultados
quando comparado com a Regressão Logística.

<div id="tab:resultados_02">

| **Resultas para o conjunto 02** |              |                         |
|:--------------------------------|:------------:|:-----------------------:|
| **Modelo**                      | **Acurácia** | **Acurácia Balanceada** |
| Regressão                       |     72%      |           66%           |
| XGBoost                         |     78%      |           73%           |

Resultados obtidos após o treinamento dos classificadores 02, assim como
para o conjunto 01 os valores de acurácia balanceada são inferiores.

</div>

## Resultados entre conjuntos

Os resultados alternando o conjunto de treino com o teste apresentaram
resultados tão bons quanto em seus respectivos conjuntos de testes, as
tabelas [2][] e [\[tab:resultados_02_no_01\]][3] apresentam confirmar a
generalização dos valores de predição dos classificadores

<div id="tab:resultados_01_no_02">

| **Resultas para o treinamento em 01 e teste no conjunto 02** |              |                         |
|:-------------------------------------------------------------|:------------:|:-----------------------:|
| **Modelo**                                                   | **Acurácia** | **Acurácia Balanceada** |
| Regressão                                                    |     67%      |           66%           |
| XGBoost                                                      |     80%      |           70%           |

Resultados obtidos após o treinamento dos classificadores no conjunto 01
e testado no conjunto 02, é possível observar que a regressão apresentou
resultados inferiores do que o testado no próprio conjunto, porém, a
XGBoost obteve uma melhoria em seus resultados.

</div>

  [1]: #tab:resultados_02
  [2]: #tab:resultados_01_no_02
  [3]: #tab:resultados_02_no_01
<div id="tab:resultados_02_no_01">

| **Resultas para o treinamento em 02 e teste no conjunto 01** |              |                         |
|:-------------------------------------------------------------|:------------:|:-----------------------:|
| **Modelo**                                                   | **Acurácia** | **Acurácia Balanceada** |
| Regressão                                                    |     81%      |           79%           |
| XGBoost                                                      |     81%      |           75%           |

Resultados obtidos após o treinamento dos classificadores no conjunto 02
e testado no conjunto 01, diferente do resultado da
Tabela [\[tab:resultados_01_no_02\]][1] a regressão manteve uma
consistência melhor que XGBoost, mas ambos possuem valores aceitaveis

</div>

# Discussão

Os resultados apresentados refletem a performance do classificador sobre
os dados de teste, porém, durante a implementação foram testadas outras
abordagens e variáveis durante o treinamento, como a utilização da
latitude e longitude para como informações ao modelo, assim como o
gênero do paciente.

Entretanto, os resultados obtidos durante o treinamento com essas
informações adicionais se mostraram inferiores a fornecer somente a
idade do paciente, sendo assim, foi optado por somente utilizar essa
métrica para nossas predições.

# Conclusão

Ambos os conjuntos se mostraram aptados a serem utilizados para o
treinamento de classificadores capazes de identificar a mortalidade em
pacientes utilizando as métricas obtidas a partir dos conjuntos, outro
ponto importante é a capacidade de generalização do classificador, visto
que, o treinamento realizado em um conjunto consegue prever os
resultados no outro.

Além dos resultados quantitativos dos modelos foi possível adquirir um
conhecimento em uma nova área para os membros do grupo, visto que, ambos
não possuíam experiência em tratas e processar dados para algoritmos de
aprendizado de máquina no contexto da saúde. Servindo como oportunidade
de aprendizado e fixação dos conceitos apresentados em aula e vídeos
disponibilizados pelos docentes.

  [1]: #tab:resultados_01_no_02
## Dificuldades

-   Muitas métricas com valores em nulo *Nan* impossibilitando a sua
    utilização, por reduzir muito o tamanho do conjunto de dados, por
    exemplo, gênero do paciente foi uma ideia inicial, porém, que se
    mostro inviável.

-   Quantidade de amostras após seleção dos critérios para gerar as
    variáveis de interesse, tornando os resultados não tão
    generalizáveis quanto o esperado (os resultados bons entre conjunto
    tão pequenos não são tão confiáveis).

## Pontos em aberto

Este trabalho deixa alguns pontos que podem ser explorados em futuras
investigações a este conjunto de dados ou em problemas de classificação
de mortalidade, como, por exemplo, classificar situações onde o paciente
irá ultrapassar o valor coberto por seu plano de saúde.

Uma abordagem diferente poderia ser aplicada, utilizando séries
temporais para identificar sazonalidades nos casos de óbitos, ou
procurar por correlações entre os períodos (manhã, tarde e noite) em que
eventos ocorrem a pacientes que faleçam.


CHEN, T.; GUESTRIN, C. Xgboost: A scalable tree boosting system. In: Proceedings of the 22nd acm sigkdd international conference on knowledge discovery and data mining. EUA: XGBoost, 2016. p. 785–794.

GRON, A. Hands-On Machine Learning with Scikit-Learn and TensorFlow: Concepts, Tools, and Techniques to Build Intelligent Systems. 1st. ed. EUA: O’Reilly Media, Inc., 2017. ISBN 1491962291.

WALONOSKI, J. et al. Synthea: An approach, method, and software mechanism for generating synthetic patients and the synthetic electronic health care record. Journal of the American Medical Informatics Association, v. 25, n. 3, p. 230–238, 08 2017. ISSN 1527-974X. Disponível em: <https://doi.org/10.1093/jamia/ocx079>.
