# 📊 Previsão de Estoque Inteligente na AWS com [SageMaker Canvas](https://aws.amazon.com/pt/sagemaker/canvas/)

Bem-vindo! O SageMaker Canvas foi utilizado para criar previsões de estoque baseadas em Machine Learning (ML). Essa ferramenta disponibiliza modelos no-code, sendo sua criação acessível até mesmo para aqueles que não possuem conhecimento avançado em programação. O modelo inteligente gerado pode auxiliar em tomadas de decisão, por exemplo, sobre quanto devo estocar de cada produto em períodos específicos de alta e baixa demanda, entender quais variáveis mais afetam esta e assim realizar uma gestão mais eficiente e otimizada do estoque de um negócio.  

Criei nova conta na AWS e o dataset selecionado para treinamento desse modelo foi:
datasets/dataset-1000-com-preco-variavel-e-renovacao-estoque.csv

### 🚀 O modelo: Passo a Passo por aba do sageMaker Canvas
#### SELECT: Após entrar no Canvas e carregar o arquivo CSV em datasets, foi criado novo modelo em My Models e o dataset carregado anteriormente selecionado. 

#### BUILD: 
Configurando as variáveis de entrada e saída: a target column seleciona foi QUANTIDADE_ESTOQUE. Em Model Type, configurei o modelo como Time series, com ID_PRODUTO na opção "Item ID column", e DATA_EVENTO na opção "Time Stamp Column". Foi escolhido 1 dia para previsão futura em "Specify the number of days you want to forecast into the future.", e habilitada a opção de considerar feriados brasileiros para a previsão.
![image](https://github.com/iasminsantiago/lab-aws-sagemaker-canvas-estoque/assets/76630114/b373de59-a4fb-440a-91be-9e4c6f4d60e4)

Em seguida, em Suggestions, foi selecionado que os valores faltantes em PRECO seriam substituídos pela MEDIAN (mediana), pois dificilmente um produto seria vendido por 0,00, que era a outra opção (ZERO); e valores faltantes em QUANTIDADE_ESTOQUE substituídos por ZERO, considerando que escolher alguma medição como média e mediana poderia diminmuir a acurácia do modelo caso um produto tenha estoque bastante variável. 
Por fim, para evitar que linhas com valores vazios interfiram no bom treinamento dos dados completos disponóveis, foi escolhido em Manage rowsw a opção Drop rows by missing values, que faz linhas com valores faltantes serem removidas, para todas as colunas do dataset. ![image](https://github.com/iasminsantiago/lab-aws-sagemaker-canvas-estoque/assets/76630114/9d699d3c-8e38-4c0f-a63e-15bca05e084b)
![image](https://github.com/iasminsantiago/lab-aws-sagemaker-canvas-estoque/assets/76630114/ac6a04b8-702b-4de4-8867-07e4ce0628f7)

Após isso, foi selecionado Quick Build e iniciado o treinamento do modelo.

#### ANALYZE
As colunas mais impactantes na previsão do modelo: PRECO (51,05 %) e Holiday_BR (12,8 %).
É claro o porquê de seu impacto: o preço alto ou baixo limita o poder de compra de muitos clientes. Os produtos mais baratos são mais fáceis de serem vendidos e necessitarem de maior estoque, além de serem fator decisivo numa decisão de se comprar o produto ou não. Já a variável ligada a feriados/data comemorativas brasileiras claramente influcencia pela maior procura de produtos específicos diante de datas comemorativas, em que o comércio estimula o consumo e/ou compra de presentes. O estoque deve suprir as demandas de acordo com a venda de produtos, para que não haja falta destes e oportunidade de venda seja jogada fora, bem como não haja estoque lotado de produtos comprados que poderão se tornar obsoletos e perder seu valor de mercado. 
![image](https://github.com/iasminsantiago/lab-aws-sagemaker-canvas-estoque/assets/76630114/88ec3436-ecaf-447e-b2d3-66c2a645f0c4)


Foram obtidos os valores de Model status:
Avg. wQL = 0.344 -> um erro baixo nas previsões. Essa métrica usa média ponderada com base na importância de cada item, indicando que as previsões estão próximas da realidade diante das variáveis mais impactantes.

MAPE = 0.936 -> baixo valor, indicando grande precisão nas previsões. 

WAPE = 0.569 ->  considerando a importância de cada item do estoque para seu cálculo, seu valor baixo indica precisão especialmente para a previsão dos ítens mais críticos.

RMSE = 34.950 -> valor muito acima de 1, indicando que há pouca precisão. Porém, tal métrica penaliza grandes gaps em seu cálculo, o que pode indicar que alteração intensa na quantidade de estoque em certos períodos, como maior demanda em feriados nacionais, por exmeplo. 

MASE = 0.832 -> como tem valor menor que 1, indica modelo com previsões mais precisas do que se usássemos uma média histórica. Pode ser que a limpeza de linhas com valores nulos tenha auxiliado na precisão do modelo, visto que informar produtos com estoque nulo, por exemplo, porém na verdade com nformações não preenchidas, possa afetar a média histórica e previsão final 




#### PREDICTION
Não foi possível gerar a predição devido à conta ter um limite "tranform instance", o que tambpém ocorreu quando tentei gerar predições com outros datasets no bootcamp. 
![image](https://github.com/iasminsantiago/lab-aws-sagemaker-canvas-estoque/assets/76630114/608793a9-5844-44ab-a729-c23c72511016)
Porém, em contas que não tem limite, é possível visualziar previsões usando percentis, mostrando cenários ideais (P50), pessimistas (P10) e otimistas (P90) e exportar as previsões como CSV, que virá com as coluans ref. aos percentis também: 

------------- etapa com impedimento:
### 4. Prever

-   Use o modelo treinado para fazer previsões de estoque.
-   Exporte os resultados e analise as previsões geradas.
-   Documente suas conclusões e qualquer insight obtido a partir das previsões.

