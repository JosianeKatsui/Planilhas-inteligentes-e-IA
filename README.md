# Planilhas-inteligentes-e-IA
Montar um dashboard com tabelas e gráficos contendo informações sobre entradas e saídas financeiras, categorias, descrição, valor, tipo de operação bancária e status. 

https://chatgpt.com/c/6775f624-582c-8003-8f35-001464abdd35

import pandas as pd

# Carregar o arquivo Excel para analisar os dados
file_path = '/mnt/data/dados planilha_dashboard_chatGPD.xlsx'
excel_data = pd.ExcelFile(file_path)

# Exibir os nomes das planilhas para entender a estrutura do arquivo
excel_data.sheet_names
Resultado
['Planilha1']

# Carregar os dados da primeira planilha para análise
df = excel_data.parse('Planilha1')

# Exibir as primeiras linhas da tabela para entender o conteúdo
df.head()
Resultado
        Data  Mês     Tipo    Categoria                Descrição  Valor  \
0 2024-08-01    8  ENTRADA   Renda Fixa           Salário mensal   5000   
1 2024-08-01    8    SAÍDA  Alimentação  Compras no supermercado    550   
2 2024-08-03    8    SAÍDA   Transporte                 Gasolina    300   
3 2024-08-05    8    SAÍDA        Lazer                   Cinema    120   
4 2024-08-07    8    SAÍDA        Saúde    Consulta odontológica    250   

   Operação bancária    Status  
0      Transferência  Recebido  
1  Débito Automático  Pendente  
2  Cartão de Crédito      Pago  
3  Cartão de Crédito      Pago  
4      Transferência      Pago  

import matplotlib.pyplot as plt

# 1. **Distribuição de Gastos por Categoria**
gastos_por_categoria = df[df['Tipo'] == 'SAÍDA'].groupby('Categoria')['Valor'].sum()

# 2. **Saldo Total**
entradas = df[df['Tipo'] == 'ENTRADA']['Valor'].sum()
saidas = df[df['Tipo'] == 'SAÍDA']['Valor'].sum()
saldo_total = entradas - saidas

# 3. **Gastos Pendentes**
gastos_pendentes = df[(df['Tipo'] == 'SAÍDA') & (df['Status'] == 'Pendente')]['Valor'].sum()

# 4. **Média de Gastos**
media_por_categoria = df[df['Tipo'] == 'SAÍDA'].groupby('Categoria')['Valor'].mean()

# 5. **Tendências por Tipo de Operação**
operacoes_mais_usadas = df['Operação bancária'].value_counts()

# Exibir os insights coletados
insights = {
    "Distribuição de Gastos por Categoria": gastos_por_categoria.to_dict(),
    "Saldo Total": saldo_total,
    "Gastos Pendentes": gastos_pendentes,
    "Média de Gastos por Categoria": media_por_categoria.to_dict(),
    "Tipos de Operações Mais Usadas": operacoes_mais_usadas.to_dict()
}

insights
Resultado
{'Distribuição de Gastos por Categoria': {'Alimentação': 1600,
  'Beleza': 330,
  'Educação': 1100,
  'Eletrônicos': 3000,
  'Gastronomia': 570,
  'Lazer': 500,
  'Pet Care': 350,
  'Presentes': 830,
  'Saúde': 970,
  'Serviços': 1400,
  'Transporte': 800,
  'Utilidades Dom.': 250,
  'Utilidades Domésticas': 1250,
  'Vestuário': 1500,
  'Viagem': 1250},
 'Saldo Total': 2800,
 'Gastos Pendentes': 9720,
 'Média de Gastos por Categoria': {'Alimentação': 533.3333333333334,
  'Beleza': 165.0,
  'Educação': 366.6666666666667,
  'Eletrônicos': 1000.0,
  'Gastronomia': 285.0,
  'Lazer': 166.66666666666666,
  'Pet Care': 175.0,
  'Presentes': 276.6666666666667,
  'Saúde': 323.3333333333333,
  'Serviços': 466.6666666666667,
  'Transporte': 266.6666666666667,
  'Utilidades Dom.': 250.0,
  'Utilidades Domésticas': 625.0,
  'Vestuário': 500.0,
  'Viagem': 625.0},
 'Tipos de Operações Mais Usadas': {'Transferência': 18,
  'Débito Automático': 13,
  'Cartão de Crédito': 13}}

  import matplotlib.pyplot as plt

# 1. **Distribuição de Gastos por Categoria**
gastos_por_categoria = df[df['Tipo'] == 'SAÍDA'].groupby('Categoria')['Valor'].sum()

# 2. **Saldo Total**
entradas = df[df['Tipo'] == 'ENTRADA']['Valor'].sum()
saidas = df[df['Tipo'] == 'SAÍDA']['Valor'].sum()
saldo_total = entradas - saidas

# 3. **Gastos Pendentes**
gastos_pendentes = df[(df['Tipo'] == 'SAÍDA') & (df['Status'] == 'Pendente')]['Valor'].sum()

# 4. **Média de Gastos**
media_por_categoria = df[df['Tipo'] == 'SAÍDA'].groupby('Categoria')['Valor'].mean()

# 5. **Tendências por Tipo de Operação**
operacoes_mais_usadas = df['Operação bancária'].value_counts()

# Exibir os insights coletados
insights = {
    "Distribuição de Gastos por Categoria": gastos_por_categoria.to_dict(),
    "Saldo Total": saldo_total,
    "Gastos Pendentes": gastos_pendentes,
    "Média de Gastos por Categoria": media_por_categoria.to_dict(),
    "Tipos de Operações Mais Usadas": operacoes_mais_usadas.to_dict()
}

insights

# Configurações para melhorar a aparência dos gráficos
plt.style.use('seaborn')

# 1. Gráfico de distribuição de gastos por categoria
plt.figure(figsize=(10, 6))
gastos_por_categoria.sort_values().plot(kind='barh', color='skyblue', edgecolor='black')
plt.title('Distribuição de Gastos por Categoria', fontsize=14)
plt.xlabel('Valor (R$)', fontsize=12)
plt.ylabel('Categoria', fontsize=12)
plt.tight_layout()
plt.savefig('/mnt/data/gastos_por_categoria.png')
plt.show()

# 2. Gráfico do saldo total
plt.figure(figsize=(6, 6))
plt.pie([entradas, saidas], labels=['Entradas', 'Saídas'], autopct='%1.1f%%', colors=['green', 'red'], startangle=140)
plt.title('Proporção de Entradas e Saídas', fontsize=14)
plt.tight_layout()
plt.savefig('/mnt/data/saldo_total.png')
plt.show()

# 3. Gráfico de gastos pendentes
plt.figure(figsize=(6, 6))
pendentes = [gastos_pendentes, saidas - gastos_pendentes]
plt.pie(pendentes, labels=['Pendentes', 'Pagos'], autopct='%1.1f%%', colors=['orange', 'blue'], startangle=140)
plt.title('Proporção de Gastos Pendentes', fontsize=14)
plt.tight_layout()
plt.savefig('/mnt/data/gastos_pendentes.png')
plt.show()

# 4. Gráfico de média de gastos por categoria
plt.figure(figsize=(10, 6))
media_por_categoria.sort_values().plot(kind='barh', color='lightcoral', edgecolor='black')
plt.title('Média de Gastos por Categoria', fontsize=14)
plt.xlabel('Valor Médio (R$)', fontsize=12)
plt.ylabel('Categoria', fontsize=12)
plt.tight_layout()
plt.savefig('/mnt/data/media_por_categoria.png')
plt.show()

# 5. Gráfico de tipos de operações mais usadas
plt.figure(figsize=(8, 5))
operacoes_mais_usadas.plot(kind='bar', color='purple', edgecolor='black')
plt.title('Tipos de Operações Mais Usadas', fontsize=14)
plt.xlabel('Operação Bancária', fontsize=12)
plt.ylabel('Frequência', fontsize=12)
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('/mnt/data/tipos_operacoes.png')
plt.show()

# Configurações para melhorar a aparência dos gráficos
plt.style.use('seaborn')

# 1. Gráfico de distribuição de gastos por categoria
plt.figure(figsize=(10, 6))
gastos_por_categoria.sort_values().plot(kind='barh', color='skyblue', edgecolor='black')
plt.title('Distribuição de Gastos por Categoria', fontsize=14)
plt.xlabel('Valor (R$)', fontsize=12)
plt.ylabel('Categoria', fontsize=12)
plt.tight_layout()
plt.savefig('/mnt/data/gastos_por_categoria.png')
plt.show()

# 2. Gráfico do saldo total
plt.figure(figsize=(6, 6))
plt.pie([entradas, saidas], labels=['Entradas', 'Saídas'], autopct='%1.1f%%', colors=['green', 'red'], startangle=140)
plt.title('Proporção de Entradas e Saídas', fontsize=14)
plt.tight_layout()
plt.savefig('/mnt/data/saldo_total.png')
plt.show()

# 3. Gráfico de gastos pendentes
plt.figure(figsize=(6, 6))
pendentes = [gastos_pendentes, saidas - gastos_pendentes]
plt.pie(pendentes, labels=['Pendentes', 'Pagos'], autopct='%1.1f%%', colors=['orange', 'blue'], startangle=140)
plt.title('Proporção de Gastos Pendentes', fontsize=14)
plt.tight_layout()
plt.savefig('/mnt/data/gastos_pendentes.png')
plt.show()

# 4. Gráfico de média de gastos por categoria
plt.figure(figsize=(10, 6))
media_por_categoria.sort_values().plot(kind='barh', color='lightcoral', edgecolor='black')
plt.title('Média de Gastos por Categoria', fontsize=14)
plt.xlabel('Valor Médio (R$)', fontsize=12)
plt.ylabel('Categoria', fontsize=12)
plt.tight_layout()
plt.savefig('/mnt/data/media_por_categoria.png')
plt.show()

# 5. Gráfico de tipos de operações mais usadas
plt.figure(figsize=(8, 5))
operacoes_mais_usadas.plot(kind='bar', color='purple', edgecolor='black')
plt.title('Tipos de Operações Mais Usadas', fontsize=14)
plt.xlabel('Operação Bancária', fontsize=12)
plt.ylabel('Frequência', fontsize=12)
plt.xticks(rotation=45)
plt.tight_layout()
plt.savefig('/mnt/data/tipos_operacoes.png')
plt.show()
