# Equipe ATOM

# Projeto 3 - Reproduzindo o Experimento de um Artigo Científico

O objetivo geral do projeto é reproduzir um experimento (total ou parcialmente) de um artigo científico. O tema do artigo deve estar relacionado a Ciência de Redes e Saúde. Poderão ser aceitos artigos cujos temas que tangenciam a área de saúde. Temas de artigos envolvendo ciência de redes em outros domínios deverão ser negociados previamente com os professores.
<!-- 
Recomenda-se o seguinte índice para encontrar artigos com dados de redes publicados: (https://icon.colorado.edu/#!/networks)[https://icon.colorado.edu/#!/networks]

A equipe tem a liberdade de adaptar e simplificar o experimento, conforme a disponibilidade dos dados, dos algoritmos e do grau de dificuldade na reprodução.

Para a reprodução do experimento, pode ser usada qualquer ferramenta/framework de processamento de redes complexas.

O trabalho será feito em equipes (preferencialmente as mesmas do P2).

## Instruções para o Relatório Final

Segue abaixo o modelo de como deve ser documentada a entrega.
> Tudo o que aparece neste modo de citação, ou indicado entre `<...>`, se refere a algo que deve ser substituído pelo indicado. No modelo são colocados exemplos ilustrativos, que serão substituídos pelos do seu projeto.

Se o relatório for feito em um notebook, o modelo a seguir pode ser colocado dentro do notebook diretamente. Nesse caso, coloque no markdown do projeto (fora do notebook) uma cópia dos dados até a seção de `Apresentação` e um link para o notebook com o relatório.

# Modelo Relatório Final de Projeto P3 -->

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
<!-- > Escreva um breve do artigo (com as suas palavras, não deve ser copiado texto do artigo). -->
Este artigo trabalhou com dados sobre casos de dengue na malásia, principalmente na região de Gombak onde segundo estudos citados, vem sofrendo com a grande quantidade de casos desde do começo da década passada. Dessa forma, os casos de dengue por distrito foram agrupados e modelados como uma rede bipartida com pesos, sendo o primeiro conjunto de vértices os grupos _clusteres_ de casos e o segundo grupo a quantidade de casos, em seguida foi realizada a projeção isolando as regiões em uma rede separada, os pesos para a rede projetada foram realizadas de três maneiras distintas, forma binária, por soma e NewMan.

Com a projeção dos grupos construída, foram realizadas diversas métricas como: força de cada vértice (Centralidade de Freeman's), clusterização, distância e centralidade, e realizando a análise dos valores obtidos e as cruzando com informações geográficas de cada região com casos, foi possível concluir que essas métricas se mostraram efetivas em localizar as principais reigões (com maior quantidade de casos) e que elas estavam interligadas, assim, fornecendo informações importantes para os orgãos competentes atuarem nesses locais de maior incidência de casos.

# Breve descrição do experimento/análise do artigo que foi replicado
Das várias métricas apresentadas pelo artigo, a força dos vértices para os três tipos de projeções foi a selecionada para ser repliacada, com o objetivo de replicar os principais grupos de casos de dengue, a entrada de dados foi a quantidade de casos que cada reigão registraram entre 2013 e 2014, que foi projetada como uma rede bipartida com pesos, já a saída é as quais os grupos com maior força de vértice para cada representação junto da apresentação gráfica dessa informação.

<!-- > Descreva brevemente a parte do artigo cujo experimento ou análise foi reproduzido. Explique o que foi usado como entrada e saída. -->

## Dados usados como entrada
Dataset | Endereço na Web | Resumo descritivo
----- | ----- | -----
Dataset of dengue patients in Selangor | [https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6689515/pdf/hir-25-182.pdf](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6689515/pdf/hir-25-182.pdf) | Dataset contendo duas informações, a sigla das regiões na mlásia com os casos de dengue e a quantidade de casos, essas informações formam um grafo bipartido, que no trabalho original foi projetado ligando as reigões quando elas possuisem a mesma quantidade de casos (dados disponiveis no Apendice 1 do artigo).

# Método
A implementação realizada para reproduzir o artigo selecionado teve algumas alterações, sendo elas: utilizar a base de dados dos mesmos autores, porém apresentada em outro artigo (dado que, os dados originais não se encontravam mais disponiveis), assim como a realização da representação visual da projeção do grafo, visto que realizamos ela através do software _Gephi_ devido a dificuldades técnicas com a biblioteca _networkX_. Outro ponto que demandou alterações foi a remoção de alguns vértices da rede, uma vez que eles se encontravam em duplicidade (quatro no total).

Ademais, as equações e formas de tratamento dos dados (criação da rede e projeção) seguiram a risca como os autores originalmente a descreveram, porém, não foi possível implementar todas as métrica citadas no artigo devido ao tempo que foi demandando para encontram um artigo que possuísse base de dados disponível.

### Ferramentas utilizadas

A construção da rede bipartida e as formas de representar a projeção foram implementadas utilizando a biblioteca _NetworkX_ na linguagem de programação _Python_, onde já se encontram métodos prontos que facilitam a representação de uma rede, como: criação de grafos direcionados e não direcionados, onde ambos foram utilizados em nossa implementação (dado que, as projeções dos grupos são representações direcionados com pesos). Outra facilidade apresentada por essa biblioteca é a consulta das informações de um vértice (grau, vizinhos, centralidade, entre outros).

Além da biblioteca _NetworkX_, o software _Gephi_ foi utilizado para apresentar de forma visual uma das projeções, para ilustar aquilo que os autores do artigo concluíram ao análisar a rede. Para isso, exportamos uma das nossas projeções através do método _write_gexf_ disponível na _NetworkX_, o que possibilitou a utilização desse software para complementar nossos resultados.

### Algoritmos
Em pose dos dados, toda a implementação realizada desde a construção da rede até a representação gráfica dos grupos com as maiores quantidade de casos, pode ser resumida a 5 etapas, sendo elas: Leitura e tratamento dos dados, construção da rede bipartida, projeção da rede bipartida, obtenção da força dos vértices e representação visual da força dos vértices.

Mesmo os dados sendo fornecidos diretamente de um artigo, foi necessário o uso de algumas técnicas de pré-processamento, nesse caso foi a remoção de informações duplicadas. Feito isso a construção da rede bipartida se deu ligando o vértice de cada grupo com o vértice que representava a quantidade de casos que ele apresentou. A projeção foi implementada interligando os grupos que possuiam ligações com a mesma quantidade de casos.

Utilizando a projeção como ponto de partida, foram aplicadas as três formas de criar os pesos, a mais básica são os pesos binários, onde cada aresta recebeu o peso 1 para representar a ligação entre os vértices e 0 quando não existe uma aresta (como não existem arestas que não conectam dois vértices, logo todas possuem peso 1). Uma forma um pouco sofisticada é os pesos através da soma, que nada mais é que o peso da aresta ser a quantidade de casos que essa região possui. A última forma implementada foi a NewMan que se assemelha com a soma, com a única diferença que os valores são normalizadas.

Em posse de todas as implementações, foi feita a Centralidade de Freeman's que no artigo é denominada como força do vértice, essa medida foi aplicada a todos os vértices das projeções por soma e NewMan, esses valores basicamente representam a soma de todos os peso das arestas que incidem em um vértice. Como a rede é direcionada, essa métrica se torna interessante de ser aplicada para identificar o fluxo dentro da rede (segundo os autores).

Assim como no artigo original, foi apresentado um histograma contendo os dados das representações para as localidades de prefixo _GL_. Realizamos uma pequena alteração na imagem (uma linha horizontal vermelha) para facilitar a identificação de quais regiões possuem os valores acima de 80% dos casos.

### Abordagem e análise

Visando manter a maior coêrencia possível com o artigo original, reproduzimos as mesmas formas manualmente (sem utilizar de métodos presentes nas bibliotecas) para tornar os resultados mais pareáveis o possível (considerando as limitações da fonte de dados e grupos adicionais). Assim, analisamos quais são os principais grupos (vértices com maior força) e obtivemos as mesmas conclusões que o artigo original.

<!-- > Método usado para a análise -- adaptações feitas, ferramentas utilizadas, abordagens de análise adotadas e respectivos algoritmos.
> Etapas do processo reproduzido. -->

# Resultados

Um ponto de atenção é que como os dados não parecem ser exatamente os mesmos dos exemplos apresentados no artigo, nossos resultados não batem exatamente com o do experimento original, porém, apresentam uma grande semelhança no comportamento (alguns grupos que apresentam uma quantidade grande de casos estão interligados e existem regiões com poucos casos isoladas). Assim, podemos concluir que os resultados não são os mesmos, mas apresentam certa correlação. Focamos em comparar os grupos de regiões definidas como _GL_ uma vez que existem outras duas siglas presentes nos dados disponibilizados, porém elas não são citadas no artigo (acreditamos que seja uma extenção da base de dados original).

A representação gráfica através do histograma nos possibilitou comparar os resultados com o artigo original, e observamos que assim como eles, nossas projeções possuem grupos com grande quantidade de casos conectados entre sí, e grupos onde poucos casos estão presentes acabam se isolando. O que confirma a teoria de _clustering_ entre reigões que tem muitos casos, e atuar diretamente nelas (procurando por focos de dengue) pode resolver o problema de forma mais efetiva (devido a tendência de espalhamento como citam os autores do artigo).

A única diferença é que algumas reigões aparecem diferentes entre o orginal e a nossa releitura, provavelmente devido a alguma alteração dos dados entre os que foram utilizados no artigo e nos que foram disponibilizados posterioremente.

* Maiores detalhes das fórmulas e os valores dos principais grupos estão presentes no [_Notebook_](https://github.com/lstival/mo826/blob/main/P3/P3_MO826.ipynb)*
<!-- 

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
|**Real**       | **Positivo**       |       2       |           5        | -->
