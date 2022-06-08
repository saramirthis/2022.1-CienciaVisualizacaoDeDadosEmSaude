# Projeto 3 - Reproduzindo o Experimento de um Artigo Científico


## Apresentação


O presente projeto foi originado no contexto das atividades da disciplina de pós-graduação Ciência e Visualização de Dados em Saúde, oferecida no primeiro semestre de 2022, na Unicamp, e desenvolvido pelos alunos:

Nome                           | RA     | Especialização
-------------------------------|--------|------------
Matheus Papa de Almeida        | 183888 | Engenharia Agrícola
Sara Mirthis Dantas dos Santos | 224018 | Aluna Especial - Engenharia Elétrica


## Referência bibliográfica do artigo lido

Larremore DB, Clauset A, Buckee CO (2013) A Network Approach to Analyzing Highly Recombinant Malaria Parasite Genes. PLoS Comput Biol 9(10): e1003268. Disponível em: https://doi.org/10.1371/journal.pcbi.1003268


## Resumo

O artigo analisado utiliza como objeto de estudo os genes *var* do parasita da Malária *Plasmodium falciparum*, que devido à sua recombinação rápida e sua grande diversidade, não podem ser analisadas pelas ferramentas filogenéticas padrão. Para tanto, os autores utilizam técnicas de redes a fim de mapear as regiões altamente variáveis (do inglês highly variable regions, HVRs) provenientes das sequências de genes var. A partir das análises das redes complexas geradas, foi concluído que as restrições recombinacionais de algumas HVRs são correlacionadas, enquanto outras são independentes. Além disso, a estrutura dessas sequências permite que o parasita se diversifique rapidamente, evitando respostas imunes e contribuindo para manutenção da estrutura e da função do antígeno.


## Descrição do experimento do artigo replicado

O experimento realizado pelos autores foi dividido em etapas, com o intuito de focar em sequências de mosaico recombinantes. As etapas foram:

* Identificar regiões altamente variáveis (HVRs) em todas as sequências;
* Comparar sequências em pares dentro de cada HVR;
* Identificar estatisticamente comunidades em cada rede.

No primeiro passo, são utilizados como entrada os dados de sequências de aminoácidos, para que seja feita a identificação das HVRs, como mostra a Figura 1A. A segunda etapa utiliza os dados do processo anterior para gerar redes de recombinação não direcionadas e não ponderadas, na qual os nós da rede representam as sequências e as conexões indicam uma relação recombinante entre os nós, como demonstra a Figura 1B. Já na terceira e última etapa, as comunidades de recombinação são identificadas através de um modelo de blocos estocásticos de grau corrigido, nos quais a saída é uma lista que expressa quais nós pertencem a cada comunidade encontrada (Figura 1C e 1D). Essas comunidades representam grupos de genes var, que se recombinam com mais frequência entre si do que com genes de outras comunidades.

<figure align="center">
  <img src="https://dl.dropbox.com/s/1r4it20n6pvxmti/Figura1.png" alt="HRV_1" style="width:600px;">
  <figcaption align = "center">Figura 1 - Visão geral do método de análise de sequência. (A) Identificação das HVR nas sequências. (B) É feita uma rede para cada HVR, onde cada sequência é um nó, e um link conectando dois nós corresponde a um bloco de sequência compartilhado. (C) O conjunto de conexões de pares acima do limiar de ruído define uma rede complexa que representa eventos recentes de recombinação. (D) As comunidades são identificadas a partir de um modelo generativo probabilístico.</figcaption>
</figure>


## Dados usados como entrada

Dataset                           | Endereço na Web                                                  | Resumo descritivo
----------------------------------|------------------------------------------------------------------|--------------------
data_malaria_PLOSCompBiology_2013 | https://github.com/dblarremore/data_malaria_PLOSCompBiology_2013 | O dataset possui 9 arquivos utilizados para a criação das redes, derivados do mesmo conjunto de sequências genéticas de genes *var* do parasita da malária. Cada um deles está no formato .txt, contendo a lista de pares de vértices adjacentes. As colunas são separadas por vírgula e não possuem cabeçalho. 


## Método

Como visto anteriormente, os autores do artigo utilizam um modelo de bloco estocástico com grau corrigido para a identificação das comunidades. Este modelo recebe uma rede não ponderada e não direcionada e o número k de comunidades. O valor de k escolhido pelos autores foi de 3. A fim de simplificar a análise, utilizamos outro método de detecção de comunidades, que permite a pré-definição do número de comunidades, de modo que os resultados originais e reproduzidos possam ser mais semelhantes. O algoritmo escolhido foi o de clusterização *k-means* (k-médias em português) do aplicativo “clusterMaker2” no Cytoscape, já que ele possibilita a partição dos dados em k *clusters* a partir da distância euclidiana entre os nós. Para isso, foram utilizadas 200 iterações a fim de obter a definição das comunidades. Entretanto, para que pudéssemos usá-lo, foi necessário adicionar uma nova coluna às redes, de modo que conseguíssemos selecionar o atributo de nó para o *cluster* no algoritmo, como mostrado na Figura 2. Isso foi feito duplicando a coluna de nós e a configurando como um atributo de nó (Fig. 3).


<figure align="center">
  <img src="https://dl.dropbox.com/s/r2c0ozboncard42/Figura2.png" alt="HRV_1" style="width:500px;">
  <figcaption align="center">Figura 2 - Configuração para uso do algoritmo k-means.</figcaption>
</figure>

<figure align="center">
  <img src="https://dl.dropbox.com/s/5zysx5lx4eaqon3/Figura3.png" alt="HRV_1" style="width:800px;">
  <figcaption align="center">Figura 3 - Configuração da coluna de nós para uso do algoritmo k-means.</figcaption>
</figure>


## Resultados
Nesta seção, será analisada a comparação da clusterização obtida pelo artigo original e a nossa reprodução simplificada, considerando apenas as redes de HVR 1, 5, 6 e 7. As HVR 8 e 9 não foram processadas por serem redes maiores e exigirem um maior processamento, já as 2, 3 e 4 foram consideradas bastante fragmentadas pelos próprios autores, não sendo possível confiar no resultado do algoritmo. A Figura 4 ilustra a detecção de comunidades para o arquivo HVR_1.txt feito por nós (Fig. 4A) e a detecção para esse mesmo arquivo feito pelos autores (Fig. 4B). As cores utilizadas nos nós dos *clusters* estão de acordo com o artigo, em que azul representa a comunidade 1, verde a 2 e vermelho a 3.


<figure align="center">
  <img src="https://dl.dropbox.com/s/2x30khopei3sc1i/HVR_1.png" alt="HRV_1" style="width:800px;">
  <figcaption align="center">Figura 4 - Comparação de comunidades com a rede HVR 1. (A)  Resultado reproduzido. (B) Resultado original.</figcaption>
</figure>

Comparando os resultados, é possível perceber que as comunidades detectadas pelos autores estão melhor separadas dentro da rede. Utilizando o k-means, as comunidades encontradas mostraram-se mais esparsas, com aparente diferença entre a quantidade de nós em cada uma delas, o que pode ser explicado pela utilização de diferentes algoritmos e softwares. A Figura 5 demonstra a comparação entre a detecção de comunidades feita por nós (Fig. 5A) e feita pelos autores (Fig. 5B) com o arquivo HVR_5.txt.


<figure align="center">
  <img src="https://dl.dropbox.com/s/00f431t4ufu5rcg/HVR_5.png" alt="HRV_1" style="width:800px;">
  <figcaption align="center">Figura 5 - Comparação de comunidades com a rede HVR 5. (A)  Resultado reproduzido. (B) Resultado original.</figcaption>
</figure>


Analisando as comunidades da Figura 5, percebemos que assim como no caso anterior, as comunidades ficaram mais esparsas com a utilização do k-means, além de que no resultado original existem nós que não são interligados por arestas com nenhum outro nó, mas que foram categorizados nas nas comunidades 1 (azul) e 2 (verde). No entanto, é possível identificar semelhanças em algumas conexões e comunidades em ambas as redes. Na Figura 6, temos a detecção de comunidades para o arquivo HVR_6.txt, comparando o resultado obtido por nós (Fig. 6A) e pelos autores (Fig. 6B).


<figure align="center">
  <img src="https://dl.dropbox.com/s/pn1s9cuabh7mv7f/HVR_6.png" alt="HRV_1" style="width:800px;">
  <figcaption align="center">Figura 6 - Comparação de comunidades com a rede HVR 6. (A)  Resultado reproduzido. (B) Resultado original.</figcaption>
</figure>

Comparando os resultados para a sexta região altamente variável, concluímos que há uma semelhança em alguns pontos das redes analisadas, mas assim como nos exemplos anteriores, a rede gerada pelos autores apresenta comunidades melhor divididas e agrupadas, devido ao uso de um algoritmo mais apropriado para o problema. Na Figura 7 é mostrada a comparação das comunidades geradas por nós (Fig. 7A) e pelos autores (Fig. 7B) com o arquivo HVR_7.txt.


<figure align="center">
  <img src="https://dl.dropbox.com/s/ww2qopsuw8z0rhx/HVR_7.png" alt="HRV_1" style="width:600px;">
  <figcaption align="center">Figura  7 - Comparação de comunidades com a rede HVR 7. (A)  Resultado reproduzido. (B) Resultado original.</figcaption>
</figure>


Avaliando os resultados da rede HVR 7 percebemos que, visualmente, as comunidades estão bem semelhantes, porém ainda foi possível notar algumas diferenças com a utilização do algoritmo de clusterização k-means.


## Conclusão

A partir dos resultados obtidos, podemos afirmar que utilizando o algoritmo de clusterização *k-means* no Cytoscape não foi possível chegar ao mesmo resultado obtido pelos autores no experimento. Isso pode ser explicado pelo uso de outro algoritmo de detecção de comunidades, bem como a utilização de um software diferente para a geração e análise das redes.

### Principais dificuldades encontradas

Uma das principais dificuldades encontradas na execução deste trabalho foi a seleção de um artigo com experimentos que fossem compreendidos por nós, alunos fora da área da saúde.
Também tivemos problemas em usar alguns algoritmos de detecção de comunidade no Cytoscape, como o de Markov (MCL) do aplicativo "clusterMaker2", que não criou uma nova rede clusterizada, e o Edge Betweenness do CyFinder, que não conseguiu finalizar a análise para algumas redes testadas.
Em relação ao artigo que reproduzimos (A Network Approach to Analyzing Highly Recombinant Malaria Parasite Genes), testamos a detecção da comunidade com o Edge Betweenness e o k-means, todavia, como o Edge Betweenness não permite a definição prévia da quantidade de comunidades que serão criadas, ele foi desconsiderado, já que os resultados ficaram muito distantes do esperado.
