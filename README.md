
# Empréstimos e financiamentos: Estados, Municípios e DF

Os empréstimos públicos são uma ferramenta crucial que estados e municípios utilizam para financiar projetos que beneficiam a população. Esses empréstimos permitem a realização de obras de infraestrutura, a melhoria de serviços públicos e o desenvolvimento de programas sociais que de outra forma não seriam possíveis com os recursos disponíveis. Neste curso, vamos explorar os dados sobre esses empréstimos para entender melhor como eles funcionam e como são geridos.

O Papel dos Empréstimos Públicos Governos locais, como estados e municípios, muitas vezes enfrentam a necessidade de realizar investimentos significativos, seja na construção de hospitais, escolas, estradas ou em projetos de desenvolvimento urbano. Porém, esses investimentos geralmente requerem mais recursos financeiros do que o disponível em seus orçamentos anuais. É aí que entram os empréstimos públicos, permitindo que esses entes subnacionais obtenham os fundos necessários para realizar melhorias importantes.

## Processo de Obtenção de Empréstimos

Para entender melhor o contexto dos dados que estamos analisando, vamos detalhar como o processo de obtenção de empréstimos públicos funciona no Brasil:

## Solicitação do Empréstimo:

Estados e municípios, ou suas entidades, identificam uma necessidade de financiamento e solicitam um empréstimo. Esta solicitação é feita com base em projetos específicos que precisam ser financiados. Regulamentação e Normas:

A contratação de operações de crédito por esses entes deve obedecer às normas estabelecidas pela Lei de Responsabilidade Fiscal (LRF) e pelas Resoluções do Senado Federal 40/2001 e 43/2001. Essas normas visam garantir que os empréstimos sejam realizados de forma responsável, sem comprometer a sustentabilidade fiscal dos entes públicos. Verificação de Limites e Condições (PVL):

Antes de contratar um empréstimo, os estados e municípios precisam submeter um Pedido de Verificação de Limites e Condições (PVL) à Secretaria do Tesouro Nacional (STN) ou à instituição financeira credora. Esse processo verifica se o empréstimo está dentro dos limites legais e das condições financeiras estabelecidas. Aprovação e Liberação:

Após a análise, se tudo estiver conforme as normas, o empréstimo é aprovado e os recursos são liberados. Este processo envolve várias etapas de análise e aprovação para garantir que o empréstimo seja viável e sustentável.

### Link do arquivo ipynb.
https://github.com/bormotoff/anl_exploratoria1/blob/main/an_exploratoria1.ipynb

# Referência

 - [Fonte utilizada na analise](https://github.com/bormotoff/anl_exploratoria1/blob/main/Base_Dados%20-%20Operacoes%20Uniao.csv)
 - [Fonte original dos dados](https://www.google.com/url?q=https%3A%2F%2Fdados.gov.br%2Fdados%2Fconjuntos-dados%2Foperacoes-copem)
 - [Referencia técnica CRISP-DM](https://www.google.com/url?q=https%3A%2F%2Fwww.dataviking.com.br%2Fpost%2Fcrisp-dm-e-util-mesmo%23%3A%7E%3Atext%3DO%2520CRISP%252DDM%2520%28Cross%252D%2Cuma%2520estrutura%2520clara%2520e%2520flex%25C3%25ADvel)



# Documentação

## Entendimento dos dados

Esses dados são informações sobre empréstimos e financiamentos que estados, municípios e o Distrito Federal do Brasil estão buscando para financiar projetos de insfraestrutura entre outros. Vamos entender cada parte:

#### Interessado
Quem está pedindo o empréstimo (por exemplo, um município).
#### UF
A sigla do estado (por exemplo, MG para Minas Gerais).
#### Tipo de Interessado
Se é um município ou um estado.
#### Tipo de Operação:
empréstimo interno.
#### Finalidade
Para que o dinheiro será usado (por exemplo, para projetos em várias áreas).
#### Tipo de Credor
O tipo de instituição que está emprestando o dinheiro (por exemplo, um banco).
#### Credor
O nome do banco ou instituição financeira.
#### Moeda
Em que moeda o empréstimo foi feito (por exemplo, Reais).
#### Valor
Quanto dinheiro está sendo pedido.
#### Número do Processo/PVL
Um número único que identifica o pedido.
#### Código IBGE
Um número que identifica oficialmente o município ou estado.
#### Status
A situação do pedido (por exemplo, se foi aprovado).
#### Data
Quando o status foi atualizado.
#### Analisado
Quem analisou o pedido (por exemplo, um banco ou órgão do governo).


# Preparação dos dados

### Bibliotecas:

#### Frameworks
import pandas as pd

import numpy as np

import fpdf as FPDF
#### Gráficas
import matplotlib.pyplot as plt

import seaborn as sns
#### Avisos
import warnings

warnings.filterwarnings('ignore')

### Leitura dos dados
Para a leitura dos dados foram necessarias algumas etapas de modelagem;

#### 1 - Ajuste nas casas decimais
pd.options.display.float_format = '{:,.2f}'.format
#### 2 - Formatação da base
Base_Credito = pd.read_csv('Base_Dados - Operacoes Uniao.csv', encoding='latin1', sep=';')
#### 3 - Padronização das colunas
Base_Credito.columns = [ Loop.replace(' ', '_') for Loop in Base_Credito.columns ]
Base_Credito.columns
#### 4 - Transformação de colunas
Base_Credito.Valor = Base_Credito.Valor.apply( lambda Loop : Loop.replace('.', '') )
Base_Credito.Valor = Base_Credito.Valor.apply( lambda Loop : Loop.replace(',', '.') )
Base_Credito.Valor = pd.to_numeric( Base_Credito.Valor )
Base_Credito.Valor.head()

Base_Credito.Data = pd.to_datetime( Base_Credito.Data )

Base_Credito.head()
#### 5 - Enriquecimento

##### Região
uf_to_regiao = {

    'AC': 'Norte',

    'AL': 'Nordeste',

    'AM': 'Norte',

    ...
}

Base_Credito['Região'] = Base_Credito.UF.map( uf_to_regiao  )
Base_Credito.head()
##### Datas
Base_Credito['Ano'] = Base_Credito.Data.dt.year
Base_Credito['Mes'] = Base_Credito.Data.dt.month
Base_Credito['Dia'] = Base_Credito.Data.dt.day
Base_Credito.head()
##### Descrição
Base_Credito.describe( include='all' ).transpose()

Após a preparação dos dados iniciamos a fase de construção dos graficos basicos, selecionando as cores e os insights
que iriamos explorar.

## Documentação de cores

| Cor               | Hexadecimal                                                |
| ----------------- | ---------------------------------------------------------------- |
| Cor Azul        | ![#183FFE](https://via.placeholder.com/10/183FFE?text=+) #183FFE |
| Cor Verde       | ![#00D100](https://via.placeholder.com/10/00D100?text=+) #00D100 |
| Cor Amarelo     | ![#FFD000](https://via.placeholder.com/10/FFD000?text=+) #FFD000 |
| Cor Vermelho    | ![#FE0002](https://via.placeholder.com/10/FE0002?text=+) #FE0002 |

As cores foram escolhidas de dentro do logo oficial.
[Logo](https://raw.githubusercontent.com/bormotoff/anl_exploratoria1/main/Fundo_Data_Viking%20-%20Icon.png)


## Insights explorados

### Analise de Finalidades
A primeira análise visa avaliar as razões pelos quais são solicitados Empréstimos. Em um primeiro momento notamos
que o foco das declarações ocorre em "tipos" abrangentes que são Multissetoriais, Aquisição de equipamentos e principalmente Infraestrutura.
Olhando com mais foco no tipo Infraestrutura, encontramos subcategorias como Rede de comunicação, Energia elétrica, aguá & esgoto e Telecomunicações.

[Grafico top 10 finalidades](https://github.com/bormotoff/anl_exploratoria1/blob/main/Analise-finalidades.png)

### Analise de Finalidade por Região
Aprofundamos a analise para regiões para identificar quais as principais necessidades. 
No grafico abaixo notamos que existe pouca variação entre as regiões quantos as finalidades, destacando-se o sudeste que aponta maior foco
em Infraestrutura e equipamentos. Alguns insights podem ser explorados apartir dessa informação como analises demograficas, produção e escoamento

[Grafico finalidadesXregião](https://github.com/bormotoff/anl_exploratoria1/blob/main/Analise-finalidades-regiao.png)

### Distribuição por região e moeda
Os empréstimos são solicitados em diversas moedas diferentes por causa da taxa de juros, apartir dessa possibilidade queremos entender
qual a moeda mais requisitada e se existem perfis por região e mais tarde por cidade.

[Distribuição](https://github.com/bormotoff/anl_exploratoria1/blob/main/Analise-percentual.png)

### Previsão anual
O ano de 2017 teve a maior taxa de solicitações de emprestimo aprovadas de todo periodo historico, selecionamos ele como corte
e puxamos de 2017 até 2024 ainda incompleto. Apartir dai podemos buscar algumas explicações para essa margem que escoram na politica,
situação financeira do país e dos bancos que são os principais Credores desses montantes.
2024 não passará 2017 considerando o fluxo de aprovação e pagamento das cotas, mas apresenta um numero maior de solicitações em analise indicando que alcançara os anos de 2018 e 2019.

[Previsão anual](https://github.com/bormotoff/anl_exploratoria1/blob/main/Analise-Anual.png)
