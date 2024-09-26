# Concepção da coisa


## Arquitetura:

- modularizar estrutura
- modularizar grafo
    - dados brutos em json
    - dentro do objeto json, um array para nós e outro para linhas
    - cada nó ou linha é um sub-objeto JSON
    - dentro de cada nó do tipo resultado, o conjunto formado pelos campos de hipótese, tese e prova é também encapsulado em um sub-sub-objeto
    - talvez fazer o mesmo para outros nós que necessitem? tipo definições com condicionais
- na pagina de visualizacao, d3 puxa o grafo do json
- graphology ou outro software puxa o grafo e permite analises


## Tipos fundamentais de nó, com subtipos:

- axioma (ax)
- definição (def)
- resultado
    - proposição (pro)
    - corolário (cor)
    - propriedades corolárias (cor-props)
    - lema (lem)
    - teorema (teo)
    - exercício (exe), com indicação de que foi verificado ou não
- exemplos (eg)

Todo nó tem que ter tipo e subtipo para facilitar o código


## Alguns atributos desejáveis aos nós:

- numero de id --- **COMO GERAR?**
- nome
- tipo
- subtipo (se não houver, é o mesmo do subtipo)
- posição (já previamente calculada)
- descrição
- prova (se for do tipo resultado)
- posicionamento espacial na visualização do grafo
- se é dependência, seguindo algumas categorias:
    - referência externa (indicando fonte se houver específica)
    - apêndice
    - resultado que eu obtive de maneira independente
- indicadores de análise de redes (e.g. centralidade)


## Posicionamento dos nós e outras questões espaciais:

- Posicionamento fixo, exceto talvez nós conexos (tais com exemplos)
- Algum espaço para a descrição e a prova (espaço maior para nós com maior descrição)
- Coletivos e nós mais próximos:
    - nós de exemplo imediatamente abaixo daquilo que está sendo exemplificado
    - nós de definições próximas imediatamente abaixo (ex.: crescente, estritamente crescente, decrescente, estritamente decrescente)
    - coletivos de propriedades de estruturas e operações
    - coletivos de progressão e agrupamento manuais tomando precedência sobre coletivos calculados algoritmicamente
    - talvez capítulo ou seção do livro, para ajudar no posicionamento via progressão
- Necessário algoritmo para cálculo do espaço ocupado por cada nó baseado em sua descrição
    - e para o cáculo da distância do exemplo? ver em interatividade
- Necessário haver uma constante de escala para mudar todos os elementos (exemplo, padding, grossura de bordas, tamanho de fontes, espaçamento relativo etc)
    - é possível fazer escalas extremamente pequenas?


## Alguns atributos desejáveis às arestas:

- arestas que remetem a apêndices mais fracas
- arestas que remetem a teorias externas mais fracas também
- arestas que remetem a definições muito recorrentes ou fundamentais, mas que não são imediatamente sucessoras, devem ser bem apagadas, possivelmente
- onde as arestas conectam (na descrição ou, se resultado, na hipótese, tese ou prova)
- para ajudar no posicionamento e na análise, posso especificar a conexão lógica entre os nós
    - nós de exemplo recebem aresta com tipo exemplo
    - corolários
    - lemas
    - propriedades-corolárias
    - resultados imediatos de definições
    - SERÁ QUE ISSO TERIA DE SER MANUAL OU PODERIA SER AUTOMATIZADO?

Acho que alguns desses atributos deverão ser calculados separadamente no graphology


## Interatividade

- Zoom out muito grande tira o nome do nó e deixa só o nó, arredondando-o
- Hover para ver descrição (abre como se estivesse descendo)
- Hover dá um highlight nas arestas
- Hover ocorre em toda a caixa (nome do nó ou nome + descrição)
- Clicar no nó para fixar descrição (deixa bordas grossas)
- Possibilidade de visualizar toda a rede com descrição ativada
- Botão de prova para ver a prova de um resultado
- Nós de exemplo ficam quase grudados, seja com descrição aberta ou com descrição fechada -- necessário mudar a posição do exemplo nesse caso? Ou botão para ir e abrir descrição do nó de exemplos
- Possibilidade de fazer e visualizar agrupamentos manuais (por exemplo, categorias ou grupos de resultados, defs etc que eu acredito subjetivamente serem relevantes visualizados em conjunto)
- Possibilidade de comparar nós diretamente (exemplo: comparar duas definições parecidas)
- Ferramentas de análise exploratória de redes:
    - Clica no nó e exibe indicadores de centralidade e fluxo
    - Possibilidade de ver arestas sucessivas, caminhos que um nó viabiliza, entre comunidades da rede inclusive
    - Algoritmos de clusterização
    - Possibilidade de arestas que vêem só os nós de um tipo conectados ao nó em questão, e também separando por jusante e montante, exemplo: ver os resultados anteriores que levam ao resultado em questão; ver as dependências de um nó
    - Em uma primeira versão do projeto, acho que tudo teria que ser pré-computado, até para poder ser visualizado no Github. Teria que pensar em como estocar isso. Por exemplo, um caminho do item anterior é algo elaborado, seria um objeto json à parte?


## Visualização

- Caminho das arestas --- **COMO FAZER?** --- acho que vou precisar de algoritmo
- No nó do tipo def, se precisar ter o nome definido dentro do texto, marcá-lo bem (negrito e sublinhado)
- Limite de tamanho horizontal das caixas? quabrando linhas se necessário
- No canto superior esquerdo de cada caixa, o tipo/subtipo do nó em cor e letra diferente
- Uma cor para cada subtipo, exceto talvez exemplos herdando cores do exemplificado
- Bordas das caixas arredondadas
- Separação (linha) em cor clara entre o campo do tipo/nome e o campo da descrição, não contígua com as bordas da caixa
- Borda em cor fechada, quando fixada descrição add reforço interno em cor clara
- Dependências externas nas laterais
- Talvez indicação/marcador/coloração de resultados que eu mesmo obtive, resultados ainda não verificados etc
- Deve haver uma escala geral de visualização. Isso é difícil porque tudo é afetado, to tamanho da fonte ao posicionamento dos nós à grossura das borders


## Facilidades

- Interface de criação de nós e arestas
- Pesquisa de nós para recuperar as ids para criar arestas na hora de criar nós


## Princípios de criação

Evidentemente, diferentes critérios de criação de nós e, principalmente, de criação de arestas, são possíveis. Talvez eu possa adotar um critério minimalista, de forma que definições que já contenham embutidas outras definições sejam exclusivamente usadas. Isso ajudaria a sondar até mesmo a utilidade de definições compostas.



## Ambição além do limite

Em qual mundo existiria a possibilidade de implementar a demonstração de cada resultado como um grafo em si?

Desse modo seria possível compreender melhor cada prova e especificar as arestas entre da teoria (ou seja, tal definição entra em tal etapa do teorema).

Teríamos um hipergrafo?