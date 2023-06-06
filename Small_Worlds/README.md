# Small Worlds

UNIVERSIDADE FEDERAL DO RIO GRANDE DO NORTE - UFRN

DCA0209 - ALGORITMOS E ESTRUTURA DE DADOS II

Trabalho 02 - Unidade 02

Por: José Augusto Agripino de Oliveira

Neste repositório estaremos abordando uma prática realizada na disciplina de Algoritmos e Estrutura de Dados II (DCA0209), disciplina do Curso de Engenharia de Computação da Universidade Federal do Rio Grande do Norte, por orientação do professor [Ivanovitch Medeiros Dantas da Silva](https://github.com/ivanovitchm).

Este [notebook](https://github.com/AugustoOliveira099/Data-Structure-II/blob/main/Small_Worlds/Small_Worlds.ipynb) consiste no estudo do tráfego aéreo brasileiro, com um dataset composto por dados disponibilizados pela ANAC. O dataset foi obtido por meio dos códigos disponíveis no [Github de Álvaro](https://github.com/alvarofpp/dataset-flights-brazil).

Para realizar esse projeto, vamos utilizar as seguintes bibliotecas: NetworkX, nxivz, matplotlib, pandas e seaborn.

O dataset gerado a partir dos códigos não possui apenas aeroportos nacionais, mas intenacionais também, acerca dos voos para fora do país. Para que a análise seja feita apenas com os aeroportos do Brasil, foi gerado, a partir de uma filtragem, o [dataset](https://github.com/AugustoOliveira099/Data-Structure-II/blob/main/Small_Worlds/data/air_traffic_brasil.graphml) requerido. Ademais, irei realizar uma modificação nesse dataset, que é a inserção de mais duas linhas referentes à região do país e ao estado ao qual o aroporto pertence.

Todas as imagens geradas nessa analise estão disponíveis na pasta "images" para donwload, assim como os datasets utilizados, que estão na pasta "data".


# 1 - Assortatividade

A primeira análise realizada foi a de assortatividade, que refere-se a conexão entre semelhantes por meio de alguma propriedade do nó. A análise feita foi com base nas regiões do Brasil. O gráfico abaixo mostra como a rede se liga.

![Alt text](https://github.com/AugustoOliveira099/Data-Structure-II/blob/main/Small_Worlds/Images/assortativity.png)

Ao utilizar a função nxviz.attribute_assortativity_coefficient() com a rede de voos brasileiros, foi gerado um coeficiente com valor de $0.37291232237638355$.

- $1 >= assortatividade >= 0 \Rightarrow$ A rede é assortativa
- $0 > assortatividade >= -1 \Rightarrow$ A rede é disassortativa

Logo, o resultado mostra que é uma rede assortativa. Isso significa que os aeroportos de uma mesma região tendem a fazer voos entre si.


# 2 - Análise bivariada

Agora será analisada a assortividade em relação ao grau. 

Caso seja maior ou igual a $0$ - rede assortativa -, os nós com um grau alto tendem a se conectar com nós de grau alto também. Já se menor que $0$, a rede age de forma contrária: conforme o grau dos nós aumenta, mais conexões existem entre os nós de grau elevado e os nós de grau baixo.

A seguir estão os gráficos gerados a partir do grafo de todos os voos no brasil e também os gráficos gerados por meio da análise apenas das regiões.

## Assortividade do grau - Brasil

![Alt text](https://github.com/AugustoOliveira099/Data-Structure-II/blob/main/Small_Worlds/Images/Brazil_degree_assortativity.png)

## Assortividade do grau - Região Norte

![Alt text](https://github.com/AugustoOliveira099/Data-Structure-II/blob/main/Small_Worlds/Images/NORTE_degree_assortativity.png)

## Assortividade do grau - Região Nordeste

![Alt text](https://github.com/AugustoOliveira099/Data-Structure-II/blob/main/Small_Worlds/Images/NORDESTE_degree_assortativity.png)

## Assortividade do grau - Região Centro-Oeste

![Alt text](https://github.com/AugustoOliveira099/Data-Structure-II/blob/main/Small_Worlds/Images/CENTRO-OESTE_degree_assortativity.png)

## Assortividade do grau - Região Sudeste

![Alt text](https://github.com/AugustoOliveira099/Data-Structure-II/blob/main/Small_Worlds/Images/SUDESTE_degree_assortativity.png)

## Assortividade do grau - Região Sul

![Alt text](https://github.com/AugustoOliveira099/Data-Structure-II/blob/main/Small_Worlds/Images/SUL_degree_assortativity.png)

Com isso, é possível ver que em todas as situações estudadas, quanto maior o número de conexões de um aeroporto, mais ele se conecta com aeroportos com um volume de conexões menor.


# 3 - Componentes conectados

Agora vamos examinar a quantidade de componentes conectados dos aeropostos do Brasil e as propriedades de cada um deles: quatidade de elementos e porcentagem em relação às regiões.

Ao executar o código, obtemos que existem 6 componentes, no qual um deles possui 505 elementos e os outros cinco possuem apenas 1. Desse modo, a análise torna-se desnecessária para os componentes que possuem apenas um elemento, uma vez que 100% do componente pertence a apenas uma região. Segue abaixo quais são esses aeroportos e a qual região eles pertencem.

- Componente 1: $505$ elementos. (A análise será feita a seguir);
- Componente 2: $1$ elemento (SNBG - 100% na região Sudeste);
- Componente 3: $1$ elemento (SSBE - 100% na região Centro-Oeste)
- Componente 4: $1$ elemento (SBER - 100% na região Norte);
- Componente 5: $1$ elemento (SBJH - 100% na região Sudeste);
- Componente 6: $1$ elemento (SNGR - 100% na região Norte);

Para o "Componente 1", temos os seguintes resultados para os $505$ elementos:

- Norte: $24.9505$%
- Nordeste: $18.8119$%
- Sul: $15.0495$%
- Sudeste: $23.3663$%
- Centro-Oeste: $17.8218$%

# 4 - Caminho mais curto

Nesta etapa, iremos simular algumas viagens entre as regiões do Brasil, levando em consideração a conexão entre as regiões e os aeroportos que existem no dataset. As viagens que devemos simular são as seguintes:

- Cidade no Norte (1) e uma cidade no Sul (2)
- Cidade no Sul (2) e uma cidade no Nordeste (3)
- Cidade no Nordeste (3) e uma cidade no Centro-Oeste (4)
- Cidade no Centro-Oeste (4) e uma cidade no Sudeste (5)

Para analizar o caminho mais curto para chegar ao destino, utilizaremos a função nx.shortest_path(), que nos auxiliará mostrando o nome de cada um dos aeroportos que será visitado durante a viagem.

Os aeroportos estudados foram definidos a partir do seguinte dicionário:

```
airports_shortest_path = {
    "norte": "SBRB",             # Rio Branco, AC
    "nordeste": "SBSG",          # São Gonçalo do Amarante, RN
    "centro-oeste": "SBAN",      # Anápolis, GO
    "sudeste": "SBGR",           # Grarulhos, SP
    "sul": "SBPA"                # Porto Alegre, RS
}
```

Os voos mais curtos entre Rio Branco/AC e Porto Alegre/RS e entre Porto Alegre/RS e São Gonçalo do Amarante/RN são diretos. 

Já os caminhos mais curtos para os voos partindo de São Gonçalo do Amarante/RN com destino a Anápolis/GO e partindo de Anápolis/GO com destino a Grarulhos/SP possuem uma conexão em Goiânia/GO.

## Custo estimado

Ao pesquisar os preços dos dois primeiros cenários, não foi encontrado nenhum voo direto. Uma das possibilidades é que as companhias aéreas fizeram esses voos por algum tempo e pararam  por não ser rentável.

Já para os dois cenários seguintes não é possível verificar os preços, uma vez que a base aérea de Anápolos é da FAB(Força Aérea Brasileira) e não aparece nos sites de pesquisa.


# 5 - Coeficiente de clustering

Ao calcular o clutering médio da rede de voos brasileira, por meio da função nxviz.average_clustering(), obtemos o resultado de $0.6298820670024339$.

Como: 

\begin{equation}
  0 <= Clustering_Médio <= 1
\end{equation}

Isso significa que a rede é bem conectada, já que passa de $0,5$, mas existem alguns nós com mais conexões que outros.

Ao utilizar a mesma função, mas apenas para os grafos de cada região, obetivemos os seguintes resultados:

- Coeficiente de clustering da região Norte:  0.6129174483757457
- Coeficiente de clustering da região Nordeste:  0.4901000932205856
- Coeficiente de clustering da região Centro_Oeste:  0.5665069530501541
- Coeficiente de clustering da região Sudeste:  0.610573487003218
- Coeficiente de clustering da região Sul:  0.5829106238880671

Conforme os valores acima, o coeficiente de clustering mais alto é o da região nordeste, logo, é a região mais bem conectada entre si.

Ademais, a região menos bem conectada entre si é a região Nordeste, ou seja, existem alguns aeroportos mais bem conectados do que seus vizinhos.
