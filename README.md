Portfólio de Data Science: Análise Exploratória do Registro de Câncer de Poços de Caldas
1. A Missão: Extraindo Insights de Dados de Saúde
Olá! Este projeto é uma análise exploratória de um conjunto de dados com 5.861 registros de pacientes de câncer, disponibilizado no Kaggle. O objetivo central é investigar os dados para identificar padrões e tendências, transformando informações brutas em insights que possam ser comunicados de forma clara e eficaz.

A análise busca responder a perguntas cruciais, como:

Qual o perfil demográfico dos pacientes (gênero, idade, origem)?

Existe alguma relação entre a idade no momento do diagnóstico e o gênero do paciente?

Como a idade no diagnóstico se relaciona com o tempo de sobrevida após o tratamento?

Fonte dos Dados: O dataset utilizado é "base_nao_identificada_3702.csv", contendo informações anonimizadas do Registro de Câncer de Base Populacional (RCBP) de Poços de Caldas.

2. A Preparação: Limpeza e Engenharia de Atributos
Uma análise robusta começa com dados bem preparados. Nesta etapa, o foco foi na organização e transformação do dataset para viabilizar as investigações. As principais ações foram:

Renomeação de Colunas: As colunas foram padronizadas para nomes mais curtos e intuitivos, facilitando a manipulação com Pandas.

Conversão de Tipos de Dados: Colunas de data (data_diagnostico, data_obito) foram convertidas para o formato datetime, o que é essencial para cálculos de tempo.

Engenharia de Atributos (Feature Engineering): Foi criada a coluna tempo_sobrevida_dias, calculada a partir da diferença entre a data do óbito e a data do diagnóstico. Este novo atributo foi fundamental para analisar a progressão da doença.

3. A Análise: Descobertas Visuais
Nesta seção, apresento os principais insights obtidos por meio de visualizações de dados, que contam a história contida nos registros.

Insight 1: Perfil dos Pacientes por Gênero
A primeira investigação focou na distribuição dos casos entre homens e mulheres. O gráfico de pizza revela uma distribuição quase perfeitamente equilibrada no dataset.

Descoberta: A incidência de casos registrados é praticamente igual entre os sexos, com 50.1% para mulheres e 49.9% para homens. Isso indica que, nesta amostra, a doença não apresenta uma predisposição significativa por gênero.

<img width="541" height="411" alt="image" src="https://github.com/user-attachments/assets/4323b2dd-9fae-4402-b657-7404b90ccdfe" />



![Gráfico de Pizza da Distribuição por Gênero]

Insight 2: Idade do Diagnóstico e Gênero
A análise aprofundou-se na relação entre a idade e o gênero no momento do diagnóstico. O boxplot a seguir é revelador.

Descoberta: A mediana de idade no diagnóstico para homens é visivelmente mais baixa do que para mulheres. Este é um insight crucial que sugere que, para a população analisada, os homens tendem a ser diagnosticados em uma idade mais precoce, o que pode ter implicações para campanhas de prevenção e conscientização.

<img width="587" height="446" alt="image" src="https://github.com/user-attachments/assets/49bdb777-43d7-41a1-b839-b964eae699cd" />


![Boxplot de Idade por Gênero]

Insight 3: Relação entre Idade e Tempo de Sobrevida
Este gráfico de dispersão investiga a correlação entre a idade do paciente no diagnóstico e seu tempo de sobrevida em dias.

Descoberta: Há uma alta concentração de pacientes com tempo de sobrevida muito curto (próximo de zero dias), independentemente da idade. Isso sugere fortemente que muitos diagnósticos estão sendo realizados em estágios avançados da doença, quando as chances de sobrevida são menores.

<img width="1183" height="590" alt="image" src="https://github.com/user-attachments/assets/94ca2fc8-d005-492f-8819-3418ed635974" />


![Gráfico de Dispersão Idade vs. Sobrevida com Anotações]

4. As Conclusões e Próximos Passos
Esta análise exploratória forneceu insights valiosos sobre o perfil dos pacientes de câncer em Poços de Caldas. Identificamos diferenças demográficas importantes no diagnóstico e padrões preocupantes no tempo de sobrevida que apontam para a necessidade de diagnósticos mais precoces.


Ferramentas Utilizadas:

Linguagem: Python

Bibliotecas de Análise: Pandas


# ## 1. Preparação do Ambiente e Limpeza dos Dados
# A base de qualquer análise sólida é a qualidade dos dados.
# Esta etapa inicial foca em "forjar a matéria-prima" para as descobertas futuras.

import pandas as pd
import matplotlib.pyplot as plt

# Carregando o dataset. A codificação 'latin1' foi necessária devido a caracteres especiais.
df_dados = pd.read_csv('/content/base_nao_identificada_3702.csv', encoding='latin1', sep=None, engine='python')

# As colunas originais são longas e pouco práticas. A renomeação facilita a manipulação.
novas_colunas = ['codigo_paciente', 'nome_rcbp', 'sexo', 'data_nascimento',
                 'idade', 'raca_cor', 'nacionalidade', 'naturalidade_estado',
                 'naturalidade', 'grau_instrucao', 'estado_civil', 'codigo_profissao',
                 'nome_profissao', 'estado_endereco', 'cidade_endereco',
                 'descricao_topografia', 'codigo_topografia',
                 'descricao_morfologia', 'codigo_morfologia',
                 'descricao_doenca', 'codigo_doenca',
                 'descricao_doenca_infantil', 'codigo_doenca_infantil',
                 'descricao_doenca_adulto_jovem', 'codigo_doenca_adulto_jovem',
                 'indicador_caso_raro', 'meio_diagnostico', 'extensao',
                 'lateralidade', 'estadiamento', 'tnm', 'status_vital', 'tipo_obito',
                 'data_obito', 'data_ultimo_contato', 'data_diagnostico',
                 'metastase_distancia', 'unnamed_37']

df_dados.columns = novas_colunas

# ## 2. Análise Demográfica: Quem são os pacientes?

# ### 2.1. Distribuição por Gênero
# O primeiro passo é entender a composição do nosso dataset.
contagem_sexo = df_dados['sexo'].value_counts()

# Criando um gráfico de barras para uma visualização clara.
fig, ax = plt.subplots(figsize=(8, 5))
contagem_sexo.plot(kind='bar', color=['skyblue', 'salmon'], ax=ax)

# Adicionando o número exato em cada barra para precisão.
for i, valor in enumerate(contagem_sexo):
    ax.text(i, valor + 20, str(valor), ha='center', va='bottom', fontsize=12)

plt.title('Distribuição de Pacientes por Gênero', fontsize=16)
plt.xlabel('Gênero', fontsize=12)
plt.ylabel('Número de Pacientes', fontsize=12)
plt.xticks(rotation=0)
plt.box(False) # Remove a moldura para um visual mais limpo
plt.show()

Bibliotecas de Visualização: Matplotlib



# ### 2.2. Análise da Idade no Diagnóstico por Gênero
# A idade é um fator crítico em oncologia. Vamos investigar se há diferenças entre os gêneros.

# Criando um boxplot para comparar as distribuições de idade.
fig, ax = plt.subplots(figsize=(8, 6))
df_dados.boxplot(column='idade', by='sexo', ax=ax)

plt.title('Distribuição da Idade no Diagnóstico por Gênero', fontsize=16)
plt.suptitle('') # Remove o título automático gerado pelo pandas
plt.xlabel('Gênero', fontsize=12)


# ## 3. Engenharia de Atributos: Calculando o Tempo de Sobrevida
# Para entender o prognóstico, é fundamental calcular o tempo entre o diagnóstico e o óbito.
# Esta etapa cria a métrica "tempo_sobrevida_dias".

# Convertendo as colunas para o formato datetime para permitir cálculos.
df_dados['data_diagnostico'] = pd.to_datetime(df_dados['data_diagnostico'], errors='coerce')
df_dados['data_obito'] = pd.to_datetime(df_dados['data_obito'], errors='coerce')

# O 'errors=coerce' transforma datas inválidas em NaT (Not a Time), que serão tratadas como nulas.

# Calculando a diferença em dias.
df_dados['tempo_sobrevida_dias'] = (df_dados['data_obito'] - df_dados['data_diagnostico']).dt.days

# ### 3.1. Análise da Relação entre Idade e Sobrevida
# Um gráfico de dispersão pode nos ajudar a visualizar se há uma correlação entre a idade do paciente e seu tempo de sobrevida.

# Filtrando o DataFrame para incluir apenas os pacientes com dados de sobrevida válidos.
df_sobrevida = df_dados.dropna(subset=['tempo_sobrevida_dias'])

# Visualizando a relação.
plt.figure(figsize=(10, 6))
plt.scatter(df_sobrevida['idade'], df_sobrevida['tempo_sobrevida_dias'], alpha=0.5)

plt.title('Relação entre Idade no Diagnóstico e Tempo de Sobrevida', fontsize=16)
plt.xlabel('Idade no Diagnóstico (Anos)', fontsize=12)
plt.ylabel('Tempo de Sobrevida (Dias)', fontsize=12)
plt.grid(True, linestyle='--', alpha=0.6)
plt.show()

plt.ylabel('Idade (Anos)', fontsize=12)
plt.show()
