# Equipe ATOM

# Projeto 4 – Classificação de lesões de substância branca no Lúpus

O objetivo do projeto é concluir se pacientes com Lúpus Eritematoso Sistêmico (SLE) possuem lesões como isquêmicas ou desmielinizantes. Para isso foi optado por utilizar um classificador SVM, pois, os resultados nos dados preliminares demonstraram uma ótima eficiência para separar as classes, e por possuir uma complexionalidade menor e explicabilidade mais fácil foi escolhido como solução.

Além disso, foi utilizada a biblioteca SHAP que computa a "importância" de cada atributo para o classificador, dessa forma tornando o resultado do modelo mais claro, permitindo assim encontrar relações entre o que foi aprendido com o treinamento e característica da imagem médica. Esse processo permite que o profissional da saúde interessado nessa análise possa compreender e confiar (considerando que ele possui domínio sobre o problema) nos resultados obtidos pelo grupo.

# Apresentação

O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação [*Ciência e Visualização de Dados em Saúde*](https://ds4h.org), oferecida no primeiro semestre de 2022, na Unicamp.

| **Integrantes do Projeto** |        |                    |
|:---------------------------|:------:|:------------------:|
| **Nome**                   | **RA** | **Especialização** |
| Fabrício Ferreira da Silva | 231900 |     Computação     |
| Leandro Stival             | 263013 |     Computação     |

# Introdução

O lúpus é uma doença autoimune, o que significa que o nosso sistema imunitário (sistema de defesa) ataca os tecidos próprios, órgãos e células saudáveis do organismo. O lúpus eritematoso sistémico é a forma mais grave da doença, atingindo múltiplas partes do organismo, podendo surgir diversas complicações, fundamentalmente se não existir tratamento adequado e atempado.

Considerando a gravidade que essa doença por ter, o foco desse projeto é analisar as lesões que essa doença causa no cérebro e descobrir se elas são mais se assemelham com lesões isquêmicas ou desmielinizantes.
Para isso foram treinadas duas redes, uma usando CNN e outra usando SVM, colocando como entrada imagens de cérebros com lesões isquêmicas e desmielinizantes. Após a etapa de treinamento e validação dos modelos, foi utilizado como conjunto de teste imagens de lesões causadas pela SLE, e a partir desses resultados concluir com qual de ambas ela é mais parecida, junto da análise de como o classificador aprendeu a diferenciar as imagens. Esse processo será explicado a seguir.


## Ferramentas

Para o desenvolvimento desse projeto foram utilizadas as seguintes ferramentas:

* Skit-Learning: Para a implementação do classificador SVM, junto da normalização das amostras e o processo de busca em grade para a otimização dos hiper parâmetros.
* Numpy: Para a manipulação das imagens em formato de matrizes, por exemplo, normalizando e aplicando as máscaras.
* Pandas: Para manipulações dos atributos extraídos das imagens, construção e separação das amostras (conjunto de pacientes).
* Matplotlib: Manipulação das imagens, desde a leitura, visualizações durante o entendimento dos problemas e avaliações dos resultados.
* Skit-Image: Utilizado os métodos graycomatriz e graycoprops para a extração de atributos das imagens normalizadas.
* GLRLM: Responsável pela extração dos atributos das propriedades da matriz de comprimento de corrida.
* SHAP: Sendo o responsável por realizar a explicação do modelo, apresentando quais foram os atributos de maior impacto para o classificador.

## Preparo e uso dos dados

O pré-processamento dos dados é uma etapa crucial para todo e qualquer projeto, neste trabalho o passo a passo desta etapa está descrito nos tópicos abaixo, lembrando que outras abordagens foram testadas (outras formas de normalização, ordem da aplicação das máscaras antes ou após normalizar, atributos escolhidos). Entretanto, foram mantidas somente aquelas que apresentaram os melhores resultados para nosso classificador e faziam sentidos na nossa interpretação.

* A normalização realizada nas imagens foi a quantizadora, ou seja, diminuindo a quantidade de níveis de cinza da imagem, esse processo foi realizado antes da aplicação da máscara das lesões.
* As máscaras fornecidas foram aplicadas sobre as imagens normalizada, assim zerando todos os pixels que não estavam presente na máscara, esse processo tornou a imagem com valores somente na região de interesse (lesão).
* Com as imagens pré-processadas (normalização e máscara aplicada) foi realizada a extração de atributos, onde onze atributos foram obtidos para cada imagem, sendo cinco referentes o comprimento de corrida e seis para a matriz de co-ocorrência.
* Todas as imagens utilizadas de ambas as classes foram seguiram o preparo descrito acima, visando mantêm a homogeneidade dos dados e replicabilidade do experimento.

# Metodologia
O processo de separar as classes foi realizado pelo treinamento de um modelo baseado em SVM (do inglês, _Support Vector Machine_), cujo funcionamento consiste em realizar limiar capaz de separar as classes para diminuir a quantidade de amostras que se encontram sem pertencer a uma dos espaços criados. Para obter os melhores resultados possíveis para o modelo treinado o seguinte _pipeline_ foi seguido para a separação, preparação e treinamento do conjunto de dados:
  
* Inicialmente os dados foram separados por paciente, para evitar que imagens de pacientes utilizadas para o treinamento fossem utilizadas para validação, assim mantendo a integridade dos conjuntos e evitando o possível vazamento de dados.
  
* Os modelos SVM utilizam de três parâmetros principais para realizar o treinamento, Kernel, gamma e C, sendo a sua escolha realizada através de uma busca em grade (do inglês, _Grid Search_), assim selecionado a melhor combinação possível para os dados fornecidos para o treinamento. Antes do conjunto ser fornecido ao modelo foi aplicado a normalização dos dados, colocando eles todos na mesma escala, uma tarefa importantes, dado que, existiam escalas muito diferentes o que causaram problemas na classificação nos primeiros experimentos.
 
* A qualidade do modelo foi avaliada de duas formas, através da acurácia balanceada (visando evitar classificações enviesadas para somente uma classe) e a matriz de confusão, onde é possível observar a distribuição dos erros (quantidade de amostras erradas para cada classe).
  
* Durante o treinamento a acurácia apresentada pelo modelo foi de 99.8% de acertos, e quando fornecido para os dados de validação e o conjunto de teste apresentou a taxa de 100% de acertos, a matriz de confusão obtida com os dados de validação pode ser observada logo abaixo:
  
| **Matriz de confusao**     |        |                     |    |
|:---------------------------|:------:|:-------------------:|:--:|
| Verdadeiro Negativo        | 86     |   Falso Positivo    | 0  |
| Falso Negativo             | 0      | Verdadeiro Positivo | 77 |

 Escolhemos manter o SVM como classificador devido ele possuir uma boa explicabilidade, sendo essa uma das características mais importantes para um modelo de aprendizado de máquina, quando existe a necessidade de poder demonstrar e entender como aquela descrição foi tomada. Outras decisões realizadas durante a elaboração do trabalho foi a escolha da normalização, escolhemos aquela que visualmente realizou as menores alterações na imagem, porém, em simultâneo, simplificava os valores das imagens.

Normalizar os dados dos atributos obtidos também foi uma decisão importante, pois, as escalas das métricas eram bem diferente (diferença de 6 casas decimais) apresentando resultados não satisfatórios quando não realizadas. Outro ponto importante foi a utilizada da busca em grade permitindo testar diversas combinações de parâmetros para o classificador, tornando os resultados excelentes.
  
  Falando agora mais sobre as decisões de quais atributos utilizar, não foram realizados muitos testes nessa parte, pois, quando ajustamos o modelo os atributos que utilizamos apresentaram uma ótima acurácia e decidimos manter todos eles. A escolha de utilizar comprimento de corrida e matriz de co-ocorrência foi tomada devido esse serem os tópicos que possuímos maior grau de conhecimento o que facilitou o entendimento de como extrair os atributos e interpretar o que foi obtido pela biblioteca SHAP.
  
  Além disso, o SHAP foi uma escolha para facilitar entendermos o que foi aprendido pelo modelo e criar eventuais explicações para um interessado que seja de fora da computação, dado que o nosso objetivo é tornar esse estudo o mais abrangente possível, não nos fechando ao escopo da computação.

# Resultados Obtidos e Discussão
Utilizando das 450 imagens fornecidas para validar de lesões LES, aplicando o mesmo _pipeline_ dos demais dados e realizando a inferência com o modelo SVM treinando, obtidos os seguintes resultados:

| **Lesões LES**  ||     | 
|:--------|:------:|:---:|
| AVC     | 79     | 35% |
| EM      | 146    | 65% |

Onde podemos observar que ambas as classes foram selecionadas pelo classificador, porém, com uma tendência da maioria delas se assemelhar a lesões EM, para identificar como essa separação foi realizada utilizamos a biblioteca _SHAP_ que utiliza de _Shapley values_ sendo essa uma técnica de teoria de jogos, onde se computa o ganho coletivo conforme a contribuição de cada participante.
No caso de explicar um classificador, a biblioteca "simula" os resultados obtidos, porém, alterando os valores das entradas ou até mesmo as ocultando com o intuído de computar "qual o impacto no resultado caso não se utilizasse algum atributo" ou "como valores acima da média das demais desse conjunto impactam no resultado".
Dessa forma foi obtido o seguinte ranking de importância de cada atributo para o classificador:

**<IMAGEM DAS MÈTRICAS>**

A figura demonstra que a correlação entre os pixels na matriz de co-ocorrência e a homogeneidade dos valores foram as métricas com maior impacto no momento da classificação das imagens, criando assim limiares que nos permitem identificar visualmente essa separação. O gráfico pode ser lido onde valores em azul representam valores para o atributo menor do que a média do conjunto, em quando os valores em rosa são superiores.

Logo podemos notar que para classificar uma amostra como lesão EM, a correlação entre os pixels na matriz de co-ocorrência deve ser baixa (grande volume de pontos azuis aglutinados no quanto superior direito), assim como a homogeneidade deve ser alta (devido a praticamente todos os pontos em tons de rosa estarem presentes do lado direito do eixo X).

Uma possível interpretação da correlação nesse escopo é a presença de padrões na textura da imagem, sendo assim, aparentemente lesões EM (logo as LES também devido à similaridade) possuem uma maior não regularidade nos padrões das lesões.
Já os alores de homogeneidade mostram o quão unidos estão essas regiões de tons iguais ou parecidos, como as lesões EM possuem os valores acima da média do conjunto, isso pode indicar que esse tipo de lesão é praticamente uniforme sem interrupções, confirmando a possível interpretação da correlação.

# Conclusão
  Após a idealização e implementação deste trabalho foi possível concluir que as amostras de Lúpus possuem em sua maioria semelhança com as lesões EM sendo essa a classe de imagens segundo nosso classificador SVM mais a classificou. 
  Considerando os resultados obtidos e as interpretações realizadas por nós, podemos concluir que as lesões LES possuem regiões mais uniformes (de forma contínua na imagem) e com um padrão de textura único (para não apresentar falhas devido à lesão ser uniforme).
  
> Principais desafios enfrentados:
Durante a implementação os maiores desafios foram identificar qual a melhor forma de obter os atributos que tínhamos em mente para classificar as imagens, assim como entender o problema, devido a nossa não familiaridade em trabalhar com imagens desse tipo.

> Lições aprendidas:
Durante a implementação adquirimos conhecimentos técnicos sobre processamento de imagens, como, formas de representar imagens sem utilizar a matriz dos pixels, como são realizadas as extrações de imagens médicas além do conhecimento mais específico sobre a área do problema (imagens de lesões cerebrais).

> Trabalhos Futuros:
Possíveis melhorias e até mesmo continuações para esse trabalho seriam: identificar com mais profundidade a importância de cada atributo para o classificador, para que o entendimento sobre o problema se torne mais robusta, permitindo a sua aplicação em casos reais. Utilizar a CNN que aproveitamos a implementação fornecida pelos professores, porém, que não optamos em utilizar devido a já possuímos um classificador com uma robustez aceitável e explicabilidade mais simples.
