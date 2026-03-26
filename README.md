# Python-Pandas-Publico-Cinema-Brasil-2014-2025-ANCINE
Utiliza a base de dados da ANCINE com todos os registros de públicos nas salas de cinema entre 2014 e 2025.
O público mais recorrente nas salas de cinema no Brasil é de zero pessoas. Abaixo tem a lista completa.


````python
import pandas as pd # Biblioteca principal para manipulação e análise de dados.
import os           # Biblioteca para interagir com o sistema operativo (pastas e ficheiros).

# Define o caminho da pasta onde estão guardados os centenas de ficheiros CSV da Ancine.
# Cria uma lista com o nome de todos os ficheiros dentro da pasta que terminam com '.csv'.
arquivos = [f for f in os.listdir(caminho) if f.endswith('.csv')]

# Exibe a lista de nomes encontrados para conferência.
print("Arquivos encontrados:", arquivos)
caminho = r"F:\Ponto Data\Reportagens\Cinema Ponta Grossa\Bilheteria diária"
lista_df = [] # Cria uma lista vazia para armazenar temporariamente cada tabela lida.

for arquivo in arquivos:
    # Cria o caminho completo (pasta + nome do ficheiro) para o Python conseguir abrir.
    caminho_arquivo = os.path.join(caminho, arquivo)
    print("Lendo:", arquivo)
    
    # Lê o CSV. O 'sep=None' faz com que o pandas descubra sozinho se o separador é vírgula ou ponto-e-vírgula.
    # 'engine=python' é usado para dar suporte a essa deteção automática.
    df = pd.read_csv(caminho_arquivo, sep=None, engine='python')
    
    # Adiciona a tabela atual à nossa lista de tabelas.
    lista_df.append(df)

# Junta todas as tabelas da lista num único DataFrame (tabela gigante) chamado df_final.
# 'ignore_index=True' refaz a numeração das linhas de 0 até ao fim.
df_final = pd.concat(lista_df, ignore_index=True)

# Define o nome e local do novo ficheiro consolidado.
saida = os.path.join(caminho, "bilheteria_brasil.csv")

# Guarda o DataFrame final em CSV. 
# 'index=False' evita que o Python crie uma coluna de números desnecessária.
# 'encoding=utf-8-sig' garante que caracteres especiais (acentos) sejam lidos corretamente no Excel.
df_final.to_csv(saida, index=False, encoding='utf-8-sig')

print("✅ Arquivo salvo em:", saida)

# Exibe a quantidade total de linhas (neste caso, mais de 33 milhões).
total_linhas = len(df_bilheteria)
print(f"O DataFrame tem {total_linhas} linhas.")

# Mostra informações técnicas: nome das colunas, tipos de dados e memória utilizada (4.5+ GB).
df_bilheteria.info()

# Conta quantas vezes cada valor de público aparece. Útil para ver a frequência de salas cheias ou vazias. O público mais recorrente nas salas de cinema do Brasil é de 0 pessoas.
df_bilheteria['PUBLICO'].value_counts().head(20)

PUBLICO
0     1919643
2     1460173
4     1259505
6     1103506
5     1039027
3     1025646
8      985366
7      963066
9      877461
10     872510
11     810402
12     790863
13     721200
14     695539
15     653582
1      627352
16     618756
17     583554
18     559327
19     526091
Name: count, dtype: int64

# Filtra apenas as sessões onde o público foi ZERO.
df_zero_brasil = df_bilheteria[df_bilheteria["PUBLICO"] == 0]

# Identifica quais os 10 filmes que mais vezes aparecem com público zero no registo.
contagem_filmes = df_zero_brasil["TITULO_BRASIL"].value_counts().head(10)
print("Top 10 filmes que aparecem com público zero:")
print(contagem_filmes)

Top 10 filmes que aparecem com público zero:
TITULO_BRASIL
MULHER-MARAVILHA 1984          12198
TOM & JERRY - O FILME          10496
SUPER MARIO BROS. - O FILME     8811
MINIONS 2: A ORIGEM DE GRU      7931
TROLLS 3 - JUNTOS NOVAMENTE     7863
UM FILME MINECRAFT              7606
LILO & STITCH                   7291
LIGHTYEAR                       6879
TROLLS 2                        6792
MUNDO ESTRANHO                  6749
Name: count, dtype: int64
