# Citizen Assist AI — Energia Renovável e Consumo Residencial

**Global Solution 2026.2 | FIAP — Ciência da Computação | Soluções em Energias Renováveis e Sustentáveis**

O **Citizen Assist AI** é um protótipo acadêmico de orientação sobre consumo consciente de eletricidade, eficiência energética e geração solar fotovoltaica. A solução combina dados públicos, cálculos realizados em Python e uma interface conversacional integrada à **Gemini API** para apresentar recomendações em linguagem acessível.

O estudo analisa uma **residência hipotética com quatro moradores em São Paulo (SP)**. Seus indicadores são estimativas fundamentadas em premissas declaradas — **não** são medições de uma residência real, um projeto executivo de energia solar ou uma promessa de economia na fatura.

## Sumário

1. [Equipe](#equipe)
2. [Problema, público e objetivos](#problema-público-e-objetivos)
3. [Arquitetura e funcionalidades](#arquitetura-e-funcionalidades)
4. [Organização dos arquivos](#organização-dos-arquivos)
5. [Fontes de dados e pré-requisitos](#fontes-de-dados-e-pré-requisitos)
6. [Como preparar o ambiente no Google Colab](#como-preparar-o-ambiente-no-google-colab)
7. [Como executar os três notebooks](#como-executar-os-três-notebooks)
8. [Como configurar e utilizar a Gemini API](#como-configurar-e-utilizar-a-gemini-api)
9. [Cálculos e resultados](#cálculos-e-resultados)
10. [Testes e evidências](#testes-e-evidências)
11. [Arquivos gerados e preservação dos resultados](#arquivos-gerados-e-preservação-dos-resultados)
12. [Problemas frequentes](#problemas-frequentes)
13. [Limitações e segurança](#limitações-e-segurança)
14. [Referências](#referências)
15. [Entrega acadêmica](#entrega-acadêmica)

## Equipe

| Integrante | Registro acadêmico |
| --- | --- |
| Matheus Borges Soares | RM 574085 |
| Eric Hernandes Penhalbel | RM 570237 |
| Enzo Ricardo Silva | RM 571333 |

**Instituição:** FIAP  
**Curso:** Ciência da Computação  
**Componente curricular:** Soluções em Energias Renováveis e Sustentáveis  
**Projeto:** Citizen Assist AI — Global Solution 2026.2

## Problema, público e objetivos

O consumo residencial de energia elétrica pode ser difícil de interpretar, especialmente quando o morador precisa comparar mudanças de hábitos com investimentos em fontes renováveis. A falta de informações claras sobre **kW, kWh, tarifa, geração e retorno financeiro** pode levar a expectativas incorretas.

O público do Citizen Assist AI é a **família de baixa renda com a conta de luz atrasada**. Para ela, a necessidade energética é concreta: diminuir uma conta que já está difícil de pagar, sem fazer dívida nova. O estudo usa um cenário simulado de **quatro pessoas** em São Paulo, atendidas pela distribuidora de referência Enel Distribuição São Paulo, e compara três alternativas para a mesma necessidade e o mesmo período (um ano):

1. **Tarifa Social / Desconto Social** (direito, sem investimento);
2. **Eficiência energética**: banho 20% mais curto (hábito, sem investimento);
3. **Energia solar fotovoltaica** (renovável, com investimento).

**Resultado principal:** para essa família, a ordem recomendada é Tarifa Social e parcelamento da conta primeiro (cerca de R$ 758 por ano, se a família tiver direito), banho mais curto em seguida (cerca de R$ 156 por ano), e solar só depois de quitar a dívida e com orçamento real (investimento estimado de R$ 3,3 mil a R$ 3,8 mil). As três orientações estão no assistente de Python (ver a seção Integração ao Citizen Assist AI).

O projeto procura:

- caracterizar o consumo residencial por equipamento, com dados e premissas explícitos;
- contextualizar o estudo com dados públicos da **ANEEL** e da **EPE**;
- avaliar o potencial de **geração solar fotovoltaica** com informações do **INPE/LABREN**;
- comparar geração renovável com uma medida concreta de **eficiência energética**;
- calcular os efeitos de combinar as duas alternativas;
- oferecer orientações em português por meio de um assistente conversacional;
- registrar verificações de consistência, cenários de teste e limitações das recomendações.

## Arquitetura e funcionalidades

O desenvolvimento foi dividido em **três notebooks Jupyter**, organizados de acordo com o fluxo de análise:

```text
Dados públicos (ANEEL, EPE e INPE/LABREN)
                   │
                   ▼
  Notebook 01 — Consumo residencial
  • leitura e tratamento de tarifas e consumo estadual
  • simulação dos equipamentos da residência
  • cálculo do consumo e custo energético de referência
                   │
                   ▼
  Notebook 02 — Energia solar e eficiência
  • seleção de ponto da grade solar para São Paulo
  • estimativa de geração e dimensionamento fotovoltaico
  • análise de sensibilidade do Performance Ratio
  • redução do tempo de uso do chuveiro elétrico
  • comparação técnica, econômica e ambiental
                   │
                   ▼
  Notebook 03 — Citizen Assist AI
  • consolidação dos indicadores em uma base estruturada
  • integração Python + Google Gen AI (Gemini API)
  • consulta por texto e interface com ipywidgets
  • verificações matemáticas e cenários funcionais
  • registro de avaliações e exportação de evidências
```

O assistente utiliza os **dados calculados localmente** como contexto para geração das respostas. A Gemini API **não é a fonte dos indicadores energéticos**: sua função é interpretar perguntas e comunicar os resultados. As instruções do assistente procuram impedir valores inventados, mas respostas geradas por IA ainda exigem revisão crítica.

### Tecnologias utilizadas

- **Python 3** e **Jupyter Notebook / Google Colab**;
- **pandas** e **NumPy**, para organização e cálculos;
- **Matplotlib**, para gráficos;
- **PyArrow**, para leitura do Parquet da ANEEL;
- **openpyxl**, para leitura da planilha Excel da EPE;
- **google-genai**, SDK de integração à Gemini API;
- **ipywidgets**, para a interface conversacional no Notebook 03;
- **CSV** e **JSON**, para preservar resultados e evidências.

## Organização dos arquivos

A organização escolhida para o repositório é:

```text
Citizen-Assist-AI/
│
├── README.md
│
├── Arquivos_Jupyter/
│   ├── 01_analise_consumo_residencial_formatado.ipynb
│   ├── 02_potencial_solar_e_geracao_fotovoltaica.ipynb
│   └── 03_integracao_validacao_e_conclusao.ipynb
│
└── Dados/
    ├── Sem Refinamento/
    │   ├── aneel_tarifas_original.parquet       # Dados brutos da ANEEL
    │   ├── epe_consumo_mensal_original.xlsx     # Dados brutos da EPE
    │   └── tilted_latitude_means_SP.csv         # Dados solares do INPE/LABREN
    │
    └── Processados/
        ├── epe_sp_residencial_mensal.csv
        ├── perfil_residencial_simulado.csv
        ├── resumo_consumo.json
        ├── resultado_geracao_fotovoltaica.csv
        ├── resumo_potencial_solar.json
        ├── sensibilidade_performance_ratio.csv
        ├── base_conhecimento_energia.json
        ├── validacao_resultados_energia.csv
        ├── roteiro_testes_funcionais.csv
        └── registro_testes_usuarios.csv
```

**Observações sobre a árvore:** os nomes dos três notebooks correspondem às versões examinadas; caso os arquivos sejam renomeados para publicação, atualize os caminhos apresentados neste documento. Os arquivos de dados e resultados listados são **nomes esperados pelo código**, não uma afirmação de que todos já estejam disponíveis no GitHub. Os arquivos em `Dados/Processados` são produzidos durante a execução e podem ser incluídos após a geração.

### Atenção aos caminhos usados pelo código

A estrutura pública do repositório utiliza **`Dados/Sem Refinamento`** e **`Dados/Processados`**, com iniciais maiúsculas e nomes em português. Entretanto, os notebooks foram desenvolvidos com estes caminhos **internos de execução**:

| Notebook | Caminho utilizado pelo código | Finalidade |
| --- | --- | --- |
| 01 | `/content/GS_SERS/data/raw/` | Entrada dos arquivos ANEEL e EPE |
| 01 | `/content/GS_SERS/data/processed/` | Saída dos dados tratados |
| 01 | `/content/GS_SERS/references/` | Registro `fontes.json` |
| 02 | `dados/tilted_latitude_means_SP.csv` | Entrada solar **relativa ao diretório atual** |
| 02 | Diretório atual da sessão | Saída dos CSV/JSON fotovoltaicos |
| 03 | `/content/GS_SERS/data/processed/` | Saída da base e das evidências |
| 03 | `/content/GS_SERS/data/processed/resumo_consumo.json` | Comparação opcional com o Notebook 01 |
| 03 | `/content/GS_SERS/resumo_potencial_solar.json` ou diretório atual | Comparação opcional com o Notebook 02 |

**Não troque apenas o nome da pasta no GitHub esperando que o código se adapte automaticamente.** A seção de preparação abaixo copia os arquivos necessários para os caminhos que os notebooks realmente utilizam, sem alterar o conteúdo das análises.

> **Linux e Colab diferenciam maiúsculas de minúsculas:** `Dados`, `dados` e `data` são diretórios distintos.

## Fontes de dados e pré-requisitos

### 1. ANEEL — tarifas de aplicação

Acesse o [Portal de Dados Abertos da ANEEL](https://dadosabertos.aneel.gov.br/) e obtenha o conjunto de **tarifas de aplicação das distribuidoras** em formato **Parquet**, correspondente à estrutura usada no Notebook 01.

Salve o arquivo com este nome:

```text
Dados/Sem Refinamento/aneel_tarifas_original.parquet
```

O Notebook 01 verifica colunas como `SigAgente`, `DatInicioVigencia`, `DatFimVigencia`, `DscSubGrupo`, `DscClasse`, `VlrTUSD` e `VlrTE`. A seleção adotada utiliza o agente `ELETROPAULO`, classe e subclasse **Residencial**, subgrupo **B1**, modalidade **Convencional**, **Tarifa de Aplicação** e a vigência de **04/07/2026 a 03/07/2027**. Se a versão baixada tiver outra estrutura ou período, a seleção poderá retornar registros diferentes ou interromper a execução.

### 2. EPE — consumo mensal de energia elétrica

Acesse [Dados do Consumo Mensal de Energia Elétrica — EPE](https://www.epe.gov.br/pt/publicacoes-dados-abertos/dados-abertos/dados-do-consumo-mensal-de-energia-eletrica) e baixe a planilha Excel usada para o estudo. Salve como:

```text
Dados/Sem Refinamento/epe_consumo_mensal_original.xlsx
```

O Notebook 01 espera a aba **`CONSUMO E NUMCONS SAM UF`**, com campos `DataExcel`, `UF`, `Classe`, `TipoConsumidor`, `Consumo` e `Consumidores`. A análise filtra **SP / Residencial** e distingue as séries de consumidores **Cativo** e **Livre**. Os indicadores estaduais contextualizam o estudo: não são medições da casa simulada.

### 3. INPE/LABREN — irradiação no plano inclinado

Acesse a [tabela solar de São Paulo do Atlas Brasileiro de Energia Solar](https://labren.ccst.inpe.br/atlas2_tables/SP_inc-en.html), referente à **2ª edição (2017)**. Utilize o CSV de irradiação no plano inclinado e salve como:

```text
Dados/Sem Refinamento/tilted_latitude_means_SP.csv
```

A leitura do Notebook 02 está configurada com `sep=";"`, `decimal="."` e `encoding="utf-8"`. São esperadas coordenadas `LAT`, `LON`, identificador `ID`, médias mensais (`JAN` a `DEC`) e indicador `ANNUAL`. Caso o download oficial tenha formato ou nome diferente, confira-o antes da execução; não renomeie um arquivo incompatível apenas para superar o erro de leitura.

### Ambiente recomendado

- Conta Google com acesso ao **Google Colab**;
- conexão com a internet para obtenção dos dados e instalação das bibliotecas;
- acesso aos arquivos originais listados acima;
- **chave válida da Gemini API apenas para as funcionalidades conversacionais**;
- cota de API disponível para realizar consultas online, quando desejado.

Os **cálculos locais e as 14 verificações matemáticas do Notebook 03 não dependem da chave Gemini**.

## Como preparar o ambiente no Google Colab

### Passo 1 — Obter o repositório

Abra o [Google Colab](https://colab.research.google.com/), conecte-se a um ambiente de execução Python e abra o notebook desejado. Para disponibilizar as pastas do repositório na sessão, execute uma célula **antes das células do notebook**:

```python
# Substitua pela URL real do repositório da equipe.
!git clone https://github.com/USUARIO/REPOSITORIO.git /content/Citizen-Assist-AI
```

**Importante:** esse comando serve para um ambiente novo e pressupõe que a pasta de destino ainda não exista. Se você já clonou o repositório nesta sessão, não o clone novamente; utilize a pasta existente. Em um repositório privado, a clonagem exigirá autorização adequada.

**Alternativa sem Git:** carregue os notebooks e arquivos brutos manualmente para o ambiente e ajuste a variável `REPOSITORIO` abaixo ao diretório onde os colocou.

### Passo 2 — Preparar os caminhos exigidos pelos notebooks

Execute esta célula de preparação **em cada ambiente Colab novo, antes de abrir ou executar a análise correspondente**:

```python
from pathlib import Path
from shutil import copy2
import os

# Pasta clonada no passo anterior.
REPOSITORIO = Path('/content/Citizen-Assist-AI')
BRUTOS = REPOSITORIO / 'Dados' / 'Sem Refinamento'
PROCESSADOS_REPO = REPOSITORIO / 'Dados' / 'Processados'

# Caminhos internos esperados pelos notebooks 01 e 03.
RUNTIME = Path('/content/GS_SERS')
RAW_INTERNO = RUNTIME / 'data' / 'raw'
PROCESSADOS_INTERNO = RUNTIME / 'data' / 'processed'
REFERENCIAS_INTERNO = RUNTIME / 'references'

# Caminho relativo exigido pelo Notebook 02.
SOLAR_INTERNO = REPOSITORIO / 'dados'

for pasta in [RAW_INTERNO, PROCESSADOS_INTERNO,
              REFERENCIAS_INTERNO, SOLAR_INTERNO, PROCESSADOS_REPO]:
    pasta.mkdir(parents=True, exist_ok=True)

arquivos_entrada = {
    'aneel_tarifas_original.parquet': RAW_INTERNO / 'aneel_tarifas_original.parquet',
    'epe_consumo_mensal_original.xlsx': RAW_INTERNO / 'epe_consumo_mensal_original.xlsx',
    'tilted_latitude_means_SP.csv': SOLAR_INTERNO / 'tilted_latitude_means_SP.csv',
}

for nome, destino in arquivos_entrada.items():
    origem = BRUTOS / nome
    if origem.is_file():
        copy2(origem, destino)
        print('OK:', origem.name, '→', destino)
    else:
        print('NÃO ENCONTRADO:', origem)

# Reutiliza, se existirem, os resumos já exportados pelas outras etapas.
resumos_anteriores = {
    'resumo_consumo.json': PROCESSADOS_INTERNO / 'resumo_consumo.json',
    'resumo_potencial_solar.json': RUNTIME / 'resumo_potencial_solar.json',
}
for nome, destino in resumos_anteriores.items():
    origem = PROCESSADOS_REPO / nome
    if origem.is_file():
        copy2(origem, destino)

# Essencial: o Notebook 02 usa o caminho relativo "dados/...".
os.chdir(REPOSITORIO)
print('Diretório atual:', Path.cwd())
```

O código acima **não altera os notebooks**. Ele apenas espelha os arquivos do repositório nos locais previstos pelo código original. Caso um arquivo seja marcado como `NÃO ENCONTRADO`, baixe ou disponibilize a fonte correspondente antes de executar a análise que o utiliza.

> **Sessões distintas:** normalmente cada notebook aberto em uma sessão Colab tem seu próprio sistema de arquivos temporário. Por isso, a preparação pode precisar ser repetida. Os resultados do Notebook 01 não aparecem automaticamente no ambiente do Notebook 03; salve-os primeiro em `Dados/Processados` ou copie-os para a nova sessão.

### Passo 3 — Conferir o diretório de trabalho

Antes de iniciar o **Notebook 02**, o diretório atual precisa conter a pasta minúscula `dados` criada no passo anterior:

```python
from pathlib import Path
print(Path.cwd())
print((Path('dados') / 'tilted_latitude_means_SP.csv').is_file())
```

O segundo comando deve apresentar **`True`**. Se apresentar `False`, volte aos passos de preparação, confira o arquivo de entrada e o diretório atual. A montagem do Google Drive presente no Notebook 02 **não substitui** essa preparação, porque o carregamento do CSV usa um caminho relativo local.

### Passo 4 — Instalar as bibliotecas

Os notebooks já possuem células de instalação/importação. Execute-as na ordem original:

**Notebook 01:**

```python
%pip install -q pandas numpy matplotlib pyarrow openpyxl
```

**Notebook 02:** importa `os`, `json`, `numpy`, `pandas` e `matplotlib`; no Colab, essas bibliotecas de análise normalmente já estão disponíveis. Caso necessário, instale-as na sessão.

**Notebook 03:**

```python
%pip install -q -U google-genai ipywidgets
```

Se o ambiente solicitar reinicialização após atualização de pacotes, reinicie o kernel e execute as células de configuração novamente.

## Como executar os três notebooks

### Notebook 01 — Análise do consumo residencial

**Arquivo:** `Arquivos_Jupyter/01_analise_consumo_residencial_formatado (7).ipynb`

1. Prepare as pastas e os arquivos **ANEEL Parquet** e **EPE Excel** conforme a seção anterior.
2. Execute a instalação de dependências e a seção **1 — Organização do ambiente**. O notebook utilizará `/content/GS_SERS` como `BASE_DIR`.
3. Execute a seção **2 — Dados tarifários (ANEEL)**. Ela valida colunas, filtra a distribuidora, a modalidade B1 residencial e a vigência especificada, remove duplicatas e calcula a tarifa de referência a partir de **TUSD + TE**.
4. Execute a seção **3 — Dados de consumo (EPE)**. Ela lê a aba estadual, filtra SP/Residencial, avalia cobertura temporal e duplicidades, calcula indicadores por unidade consumidora e gera gráficos.
5. Execute a seção **4 — Perfil residencial simulado**. Ela calcula o consumo mensal por equipamento e estima o custo com base na tarifa de referência.
6. Execute as seções finais de exportação, verificação de arquivos e referências.

**Arquivos de saída principais:**

- `data/processed/epe_sp_residencial_mensal.csv`;
- `data/processed/perfil_residencial_simulado.csv`;
- `data/processed/resumo_consumo.json`;
- `references/fontes.json`.

**Se faltar um arquivo bruto**, a célula de leitura tenta oferecer `files.upload()` no Colab. Em caso de upload manual, o arquivo será copiado para o nome interno esperado. A execução pode falhar se a planilha ou a estrutura do Parquet não corresponderem às versões utilizadas na análise.

### Notebook 02 — Potencial solar e eficiência energética

**Arquivo:** `Arquivos_Jupyter/02_potencial_solar_e_geracao_fotovoltaica (7).ipynb`

1. Confira a disponibilidade de `dados/tilted_latitude_means_SP.csv` **no diretório atual da sessão**.
2. Execute as seções **3 e 4**, que definem os parâmetros da casa simulada e importam as bibliotecas.
3. A célula da seção **5** monta o Google Drive (`drive.mount('/content/drive')`). Autorize se estiver usando Colab. O código de leitura do CSV, contudo, continua usando o caminho relativo `dados/...`.
4. Execute a seção **6** para ler e inspecionar o CSV do INPE/LABREN.
5. Execute as seções **7 e 8** para selecionar o ponto de grade mais próximo da coordenada central aproximada de São Paulo e converter os valores de **Wh/m².dia** para **kWh/m².dia**.
6. Execute as seções **9 e 10** para estimar a geração anual por kWp, o dimensionamento fotovoltaico e a comparação mensal entre geração e consumo.
7. Execute as seções **11 e 12** para analisar a sensibilidade a **PR = 0,75 / 0,80 / 0,85** e revisar as limitações.
8. Execute as seções **14 e 15** para avaliar a redução de 20% no tempo total de uso do chuveiro, comparar as duas alternativas e verificar seu efeito combinado.
9. Execute as exportações e a seção **16**, que organiza os resultados em uma base de conhecimento e demonstra orientações energéticas sem exigir Gemini.

**Arquivos de saída principais**, gravados no diretório atual da sessão:

- `resultado_geracao_fotovoltaica.csv`;
- `sensibilidade_performance_ratio.csv`;
- `resumo_potencial_solar.json`.

O notebook inclui **duas etapas de exportação do resumo solar**. A segunda gravação utiliza o mesmo nome `resumo_potencial_solar.json` e representa a versão final após a análise de sensibilidade. Preserve a saída final.

### Notebook 03 — Integração, validação e conclusão

**Arquivo:** `Arquivos_Jupyter/03_integracao_validacao_e_conclusao (5).ipynb`

1. Execute a seção **2**, que consolida os parâmetros em Python, recalcula os indicadores e monta `base_conhecimento_energia`.
2. A seção **2.4** faz uma comparação **opcional** com `resumo_consumo.json` e `resumo_potencial_solar.json`, quando esses arquivos estão acessíveis. **O assistente utiliza a base interna consolidada**, portanto os resumos não são carregados como sua fonte de execução obrigatória.
3. Execute a seção **3.1** para instalar `google-genai` e `ipywidgets`.
4. Configure o Secret **`GEMINI_API_KEY`** e execute a seção **3.2**; veja o tutorial detalhado na próxima seção.
5. A seção **3.3** realiza **uma chamada real** de teste de conexão. Execute-a somente se desejar testar a API e houver cota disponível.
6. Execute a seção **4** para preparar as instruções do assistente e a função `responder_citizen_assist()`.
7. A seção **5.1** faz **outra chamada real** ao modelo. A seção **5.2** cria uma interface com campo de pergunta e botões para enviar perguntas e limpar o histórico da sessão.
8. Execute a seção **6.1** para aplicar as **14 verificações matemáticas locais**, independentes da API.
9. Examine a seção **6.2** com os cenários **F01–F09**. A seção **6.3** só consulta a API em lote se `EXECUTAR_TESTES_API = True`.
10. Consulte a seção **6.4**, que documenta respostas de referência e a avaliação da equipe. **Não confunda respostas exemplares/simuladas com saídas produzidas pela Gemini.**
11. A seção **7** fornece instrumentos de registro anônimo de avaliações com usuários. Registre somente testes efetivamente realizados.
12. Leia a seção **8**, com conclusão técnica, limitações e exportação de dados.

**Saídas principais:**

- `data/processed/base_conhecimento_energia.json`;
- `data/processed/validacao_resultados_energia.csv`;
- `data/processed/roteiro_testes_funcionais.csv`;
- `data/processed/registro_testes_usuarios.csv`.

> É possível executar e conferir as seções **2**, **6.1**, **6.2**, **7** e **8** sem acesso à Gemini API. A seção **4** também prepara as funções localmente, mas uma resposta conversacional online depende de credencial, modelo disponível e cota.

## Como configurar e utilizar a Gemini API

### Passo 1 — Criar uma chave

1. Acesse o [Google AI Studio — API Keys](https://aistudio.google.com/app/apikey).
2. Entre com sua conta Google e crie ou selecione o projeto necessário, seguindo as instruções da plataforma.
3. Gere uma chave da **Gemini API** e guarde-a em local seguro.
4. Consulte os [modelos](https://ai.google.dev/gemini-api/docs/models) e os [limites/preços](https://ai.google.dev/gemini-api/docs/rate-limits) aplicáveis à conta antes de executar consultas. A disponibilidade e as cotas podem variar.

**Nunca** inclua a chave no README, no notebook, em prints públicos, no JSON gerado ou em commits Git. Se uma chave for divulgada por engano, revogue-a e crie outra.

### Passo 2 — Inserir a chave no Colab

1. Abra o **Notebook 03** no Google Colab.
2. Na barra lateral esquerda, clique em **Secrets** (ícone de chave).
3. Crie um novo Secret com o nome **exato** `GEMINI_API_KEY`.
4. Cole a chave no campo de valor, **sem colocá-la em uma célula de código**.
5. Habilite a permissão de acesso desse notebook ao Secret.
6. Execute a célula da seção **3.2**. O código usa `from google.colab import userdata` e `userdata.get('GEMINI_API_KEY')`.

A mensagem **`Cliente Gemini configurado (autenticação ainda não testada)`** indica que o cliente foi inicializado; ela **não** comprova, isoladamente, que o modelo respondeu. O teste real é realizado na seção **3.3**.

### Passo 3 — Conferir o modelo escolhido

O Notebook 03 contém:

```python
MODELO_GEMINI = 'gemini-3.5-flash'
```

Esse é o identificador configurado na versão do projeto. **Não é garantia de acesso na sua conta.** Se a API informar que o modelo não está disponível, altere essa única linha para um identificador acessível à conta, consultando a documentação oficial. A seção **3.4** permite realizar uma listagem opcional de modelos, alterando `CONSULTAR_MODELOS = False` para `True`.

### Passo 4 — Testar a conexão e enviar perguntas

- **Seção 3.3:** envia uma única mensagem de teste e informa se houve resposta.
- **Seção 5.1:** envia uma pergunta de demonstração sobre as duas alternativas energéticas.
- **Seção 5.2:** disponibiliza um campo de texto e os botões **Enviar pergunta** e **Limpar conversa**.

Perguntas úteis para demonstração:

```text
Quanto consome mensalmente a residência estudada?
Como reduzir o consumo do chuveiro e qual economia estimada?
Qual a potência fotovoltaica necessária no cenário simulado?
O que muda se combinar economia de energia e geração solar?
Se eu gerar o mesmo consumo anual, minha conta vai zerar?
Quais fontes foram usadas e quais são as limitações da análise?
```

O histórico de conversa é mantido **apenas na memória da sessão Python**. A função `limpar_historico()` e o botão da interface apagam esse histórico local. O conteúdo das perguntas enviado à Gemini é transmitido ao serviço externo; não envie dados pessoais ou sigilosos.

### Configuração em Jupyter local (alternativa)

Fora do Colab, a chave pode ser disponibilizada por uma **variável de ambiente** `GEMINI_API_KEY`, sem escrever seu valor no código:

**Windows PowerShell (sessão atual):**

```powershell
$env:GEMINI_API_KEY = 'SUA_CHAVE_PRIVADA'
```

**Linux/macOS (sessão atual):**

```bash
export GEMINI_API_KEY='SUA_CHAVE_PRIVADA'
```

Substitua o texto de exemplo **somente no terminal privado**, nunca em arquivos que serão enviados ao GitHub. Em execução local, ajuste os diretórios fixos do Notebook 01 e do Notebook 03 para a pasta desejada e **ignore/comente a célula `drive.mount()` do Notebook 02**, que é específica do Google Colab. Execute o Notebook 02 a partir da raiz do repositório e mantenha o CSV acessível como `dados/tilted_latitude_means_SP.csv`, ou ajuste seu caminho no código.

## Cálculos e resultados

### 1. Caracterização da residência

O consumo foi calculado com equipamentos e tempos de utilização simulados:

| Equipamento | Consumo estimado (kWh/mês) |
| --- | ---: |
| Chuveiro elétrico | 82,50 |
| Geladeira | 28,00 |
| Televisão | 15,00 |
| Notebook | 10,80 |
| Lâmpadas LED | 8,10 |
| Máquina de lavar | 6,00 |
| **Total** | **150,40** |

Para equipamentos modelados por potência e tempo de utilização, o cálculo simplificado é:

```text
E_mensal (kWh) = potência (W) × quantidade × horas/dia × dias/mês × fator_uso / 1000
```

A geladeira utiliza uma **premissa direta de consumo de 28 kWh/mês**, evitando supor funcionamento permanente na potência nominal. Todos os valores são hipóteses do estudo.

A análise considera uma tarifa de referência composta por:

| Parcela | Tarifa (R$/kWh) |
| --- | ---: |
| TUSD | 0,47242 |
| TE | 0,31696 |
| **TUSD + TE** | **0,78938** |

Assim:

- **Consumo mensal estimado:** 150,40 kWh;
- **Consumo anual estimado:** 1.804,80 kWh;
- **Custo energético mensal simplificado:** aproximadamente **R$ 118,72**;
- **Custo energético anual simplificado:** aproximadamente **R$ 1.424,67**.

**Atenção:** esses custos não representam necessariamente a fatura final. Tributos, bandeiras e outras parcelas de faturamento não foram incorporados ao cálculo simplificado.

### 2. Geração fotovoltaica

O Notebook 02 utiliza o Atlas Brasileiro de Energia Solar (INPE/LABREN, 2017), com seleção de um ponto de grade próximo à região central de São Paulo.

| Indicador | Valor de referência |
| --- | ---: |
| Ponto de grade selecionado | ID 5622 |
| Irradiação média diária anual do Atlas | 4,658 kWh/m².dia |
| Performance Ratio (PR) adotado | 0,80 |
| Geração estimada para 1 kWp | aproximadamente 1.359,14 kWh/ano |
| Consumo anual a compensar energeticamente | 1.804,80 kWh/ano |
| Potência fotovoltaica preliminar | aproximadamente **1,328 kWp** |

A geração mensal simplificada para um sistema de referência é calculada por:

```text
Geração_mensal (kWh) ≈ HSP_média_diária × dias_do_mês × potência_kWp × PR
```

O dimensionamento anual preliminar é:

```text
Potência_necessária (kWp) = consumo_anual (kWh) / geração_anual_de_1_kWp (kWh)
```

O notebook também examina **PR = 0,75, 0,80 e 0,85** e compara mensalmente a geração estimada com o consumo. **Equivalência energética anual não significa autossuficiência em todos os meses nem fatura zerada.**

### 3. Alternativa de eficiência energética

A medida analisada reduz em **20% o tempo total diário do chuveiro elétrico na residência**:

| Indicador | Cenário original | Cenário reduzido |
| --- | ---: | ---: |
| Potência do chuveiro | 5,5 kW | 5,5 kW |
| Tempo total diário da residência | 30 min | 24 min |
| Consumo mensal do chuveiro | 82,50 kWh | 66,00 kWh |

Resultados:

- **Economia mensal:** 16,50 kWh;
- **Economia anual:** 198,00 kWh;
- **Economia financeira anual estimada:** R$ 156,30;
- **Consumo residencial após a redução:** 133,90 kWh/mês ou 1.606,80 kWh/ano.

A medida é **comportamental**, depende da adesão dos moradores e não exige alterações na instalação elétrica. Os 30 minutos são o **tempo total da residência**, não 30 minutos por pessoa.

### 4. Alternativas combinadas

Após reduzir a demanda elétrica, o dimensionamento fotovoltaico preliminar também diminui:

| Indicador | Antes da eficiência | Depois da eficiência |
| --- | ---: | ---: |
| Demanda anual | 1.804,80 kWh | 1.606,80 kWh |
| Potência solar estimada | 1,328 kWp | 1,182 kWp |

A potência estimada diminui aproximadamente **0,146 kWp (10,97%)**. O resultado sustenta uma conclusão central do projeto: **reduzir o desperdício primeiro pode diminuir o tamanho do sistema renovável necessário**.

### 5. Comparação técnica, econômica e ambiental

| Critério | Geração solar fotovoltaica | Eficiência no chuveiro |
| --- | --- | --- |
| Natureza do benefício | Eletricidade gerada por fonte renovável | Eletricidade que deixa de ser consumida |
| Resultado anual do cenário | Geração equivalente a 1.804,80 kWh/ano | Economia de 198 kWh/ano |
| Custo inicial | R$ 3,3 mil a R$ 3,8 mil estimados (preço médio de R$ 2,45 a R$ 2,84/Wp; piso, não orçamento) | Sem compra de equipamentos no cenário simulado |
| Economia financeira | Até R$ 1.425/ano pela tarifa de referência; retorno simples indicativo de 2,3 a 2,6 anos; com Tarifa Social, no máximo R$ 667/ano | R$ 156,30/ano estimados pela tarifa de referência |
| Viabilidade | Depende de telhado, orientação, sombreamento, projeto e conexão | Depende principalmente dos hábitos dos moradores |
| CO₂ evitado (fator do SIN 2025: 0,0461 tCO₂/MWh) | cerca de 83 kg/ano (39 a 140 kg pela faixa mensal); fabricação e fim de vida não incluídos | cerca de 9 kg/ano |

### 6. Terceira alternativa para o público de baixa renda: Tarifa Social (Notebook 03, seção 2.5)

| Alternativa | Investimento | Economia estimada por ano | CO₂ evitado por ano | Depende de |
|---|---|---|---|---|
| Tarifa Social (primeiros 80 kWh sem cobrança) | R$ 0 | R$ 757,80 | 0 (é desconto no preço) | CadÚnico atualizado e conta no nome da família |
| Desconto Social (isenção da CDE até 120 kWh, cerca de 11,8%) | R$ 0 | R$ 134,13 | 0 | CadÚnico atualizado |
| Banho 20% mais curto | R$ 0 | R$ 156,30 | 9,1 kg | Hábito dos moradores |
| Solar 1,33 kWp | R$ 3.253 a R$ 3.771 | até R$ 1.424,67 (R$ 666,87 com Tarifa Social) | 83,2 kg | Telhado, projeto, poupança ou crédito |

**Fontes dos números novos (consultadas em 13/11/2026):** preço médio residencial de R$ 2,45/Wp no 1º trimestre de 2026 (Radar Solfácil, divulgado pela Cenário Energia em 09/06/2026) e de R$ 2,84/Wp para sistemas de 4 kWp em junho de 2025 (Greener); fator médio de emissão do SIN em 2025 de 0,0461 tCO₂/MWh, com meses entre 0,0215 e 0,0778 (MCTI; **conferir na planilha oficial do SIRENE antes da entrega**, porque o valor anual foi obtido de fonte secundária que cita a planilha); regras da Tarifa Social e do Desconto Social da ANEEL e do MME, as mesmas da base do assistente de Python.

**Leitura:** a solar é a única alternativa que gera energia renovável e a que mais economiza em reais, mas é a única que exige investimento. Para quem já está com a conta atrasada, o investimento é a barreira. A Tarifa Social rende mais que o chuveiro, sem custo, e a solar perde mais da metade do benefício quando a família já tem a gratuidade dos 80 kWh. O CO₂ evitado é pequeno nas duas alternativas porque a eletricidade brasileira já é majoritariamente renovável. Os valores são **estimativas**: não incluem tributos, bandeiras, custo de disponibilidade nem a cobrança gradual do uso da rede para quem gera (Lei 14.300/2022), e a análise de ciclo de vida dos painéis não foi feita.

## Integração ao Citizen Assist AI

Os resultados deste estudo estão em dois lugares:

1. **No assistente de Python (`citizen-assist-ai`)**, opção **"Orientações de energia (gastar menos na conta)"** do menu principal. O módulo `energia.py` carrega `data/energia.json` com três orientações calculadas aqui (EN01 Tarifa Social, EN02 chuveiro, EN03 solar), cada uma com fonte, data de verificação e limitações, e tem uma **calculadora da economia do banho** que usa a mesma fórmula e a mesma tarifa do Notebook 02. O teste `teste_energia.py` confere que a calculadora reproduz 82,5 kWh, 66,0 kWh, 16,5 kWh e R$ 156,30 deste estudo. Cada consulta entra no histórico do assistente com origem `energia`.
2. **No Notebook 03**, como base de conhecimento para o assistente conversacional com a Gemini API (seções 2 a 5).

**Contribuição desta disciplina no repositório compartilhado:** `energia.py`, `data/energia.json` e `teste_energia.py` no repositório de Python, e todo o conteúdo deste repositório GS-SERS.

## Testes e evidências

### Testes matemáticos automatizados — Notebook 03, seção 6.1

Foram implementadas **14 verificações locais**, sem consumo de cota da Gemini API, para conferir:

1. consistência entre consumo mensal e anual;
2. soma dos componentes tarifários TUSD + TE;
3. geração anual por kWp em relação ao Notebook 02;
4. potência fotovoltaica inicial;
5. economia mensal do chuveiro;
6. economia anual do chuveiro;
7. consumo anual após eficiência;
8. economia financeira anual estimada;
9. potência fotovoltaica após eficiência;
10. redução percentual da potência;
11. custo estimado da solar (potência × preço por Wp);
12. CO₂ evitado pela solar (geração × fator do SIN);
13. economia da Tarifa Social (80 kWh × tarifa);
14. existência de fontes, limitações e comparação na base estruturada.

**Resultado registrado na versão analisada: 14/14 verificações aprovadas (execução de 13/11/2026).** A célula gera `df_verificacoes`; falhas relevantes provocam `AssertionError`.

### Cenários de avaliação funcional — Notebook 03, seções 6.2 a 6.4

| Código | Tema | Comportamento esperado |
| --- | --- | --- |
| F01 | Consumo e público | Informar 150,40 kWh/mês e que o perfil é hipotético |
| F02 | Eficiência do chuveiro | Explicar 30 → 24 minutos/dia e economia de 198 kWh/ano |
| F03 | Geração fotovoltaica | Apresentar cerca de 1,33 kWp e limitações do cálculo |
| F04 | Alternativas combinadas | Apresentar 1,328 → 1,182 kWp e redução de 10,97% |
| F05 | Custo da solar | Apresentar R$ 3,3 mil a R$ 3,8 mil como estimativa de mercado; retorno indicativo, não garantido |
| F06 | Fontes e transparência | Identificar ANEEL, EPE e INPE/LABREN, sem generalizar |
| F07 | Fatura e regulação | Não garantir que a conta de luz será zerada |
| F08 | Pergunta fora do escopo | Não personalizar a economia sem dados suficientes |
| F09 | Público de baixa renda | Priorizar Tarifa Social e parcelamento, depois banho; solar só com orçamento e sem dívida |

Na versão examinada, **`EXECUTAR_TESTES_API = False`**. Assim, a seção 6.3 **não executa** automaticamente as oito consultas online. Para executá-las deliberadamente, configure o Secret, confirme a cota e altere a variável para `True`, sabendo que isso pode gerar **uma requisição por cenário**.

A seção 6.4 registra **oito respostas de referência avaliadas pela equipe como adequadas aos critérios**. A origem da resposta deve ser preservada nos registros: Eventuais testes externos do Gemini realizados por integrantes devem ser documentados separadamente com perguntas, respostas, modelo e avaliação.

O arquivo `roteiro_testes_funcionais.csv` contém os **cenários e critérios**, não um relatório automático de aprovação das respostas online.

### Avaliação com usuários — Notebook 03, seção 7

O enunciado pede verificar a **compreensão e a utilidade** das orientações, registrar as situações testadas, os comentários e os ajustes, sem dados pessoais desnecessários.

**Situação em 13/11/2026:** os três registros que apareciam numa execução anterior (todos do mesmo participante, em 3 minutos) foram **descartados** por não serem uma avaliação válida. A seção 7 começa vazia e só recebe testes feitos de verdade.

**Como o teste é feito:** numa rodada única com o assistente de Python, que serve para as duas disciplinas. O roteiro, com as tarefas de energia (6, 7 e 8) e a folha de anotação, está em `roteiro_teste_usuarios.md` (cópia nesta pasta e no repositório de Python). Para cada tarefa de energia:

1. a pessoa usa a opção "Orientações de energia" do assistente;
2. dá notas de 1 a 5 para compreensão e utilidade e diz o que ficou confuso;
3. a equipe registra com `registrar_teste_usuario(...)` na seção 7.2, usando P1, P2, P3 no lugar do nome;
4. depois da rodada, roda as seções 7.3 e 8.4 e faz commit de `registro_testes_usuarios.csv`.

**Resultado do teste com usuários (16/11/2026):** 3 pessoas de fora da equipe (P1, P2 e P3), 5 avaliações de tarefas de energia, registradas na seção 7.2 do Notebook 03 e exportadas em `Dados/Processados/registro_testes_usuarios.csv`. Média de **compreensão 4,4** e de **utilidade 4,6**, numa escala de 1 a 5.

| Teste | Participante | Situação testada | Compreensão | Utilidade | Comentário | Ajuste |
|---|---|---|---|---|---|---|
| T01 | P1 | Gastar menos energia sem gastar dinheiro (EN01, Tarifa Social) | 3 | 5 | — | Não necessário |
| T02 | P1 | Calculadora da economia do banho (EN02) | 4 | 4 | "Ajudou a ter uma noção da conta que é feita" | Não necessário |
| T03 | P2 | Placa solar com a conta atrasada (pelo menu de energia; o histórico registra a EN02) | 5 | 5 | — | Não necessário |
| T04 | P3 | Gastar menos energia sem gastar dinheiro | 5 | 4 | "Válido" | Não necessário |
| T05 | P3 | Placa solar com a conta atrasada, pela pergunta livre | 5 | 5 | "Achei interessante a resposta" | **Realizado** (abaixo) |

**O que o teste mostrou e o ajuste feito:** no T05, a pergunta "vale a pena colocar placa solar para quem está com a conta atrasada?" foi feita na pergunta livre do assistente, que respondeu com a orientação de corte por conta atrasada (CO01), e não com a de energia solar (EN03). A orientação de energia só existia no menu de energia. Ajuste realizado no repositório `citizen-assist-ai`: a busca livre passou a procurar também nas orientações de energia, e a EN03 ganhou a palavra-chave "vale a pena". A mesma pergunta agora encontra a EN03 (9 pontos, contra 8 da CO01), e o `teste_energia.py` confere isso.

**Limitações da rodada:** são 3 pessoas, número pequeno; os participantes digitaram as perguntas a partir do enunciado das tarefas; e duas avaliações (T03 e T04) não batem exatamente com o histórico do assistente, como indicado na tabela. As notas foram dadas pelas próprias pessoas logo após cada tarefa.

## Arquivos gerados e preservação dos resultados

Os notebooks escrevem arquivos em **locais internos diferentes**. Após executá-los, use a célula a seguir no Colab para reunir os arquivos em `Dados/Processados` dentro da **cópia local clonada** do repositório:

```python
from pathlib import Path
from shutil import copy2

REPOSITORIO = Path('/content/Citizen-Assist-AI')
DESTINO = REPOSITORIO / 'Dados' / 'Processados'
DESTINO.mkdir(parents=True, exist_ok=True)

origens = [
    Path('/content/GS_SERS/data/processed'),  # Notebooks 01 e 03
    REPOSITORIO,                        # Notebook 02, se o diretório atual era a raiz
]

nomes_solar = {
    'resultado_geracao_fotovoltaica.csv',
    'sensibilidade_performance_ratio.csv',
    'resumo_potencial_solar.json',
}

for pasta in origens:
    if not pasta.is_dir():
        continue
    for arquivo in pasta.iterdir():
        if not arquivo.is_file():
            continue
        if pasta == REPOSITORIO and arquivo.name not in nomes_solar:
            continue
        if arquivo.suffix.lower() not in {'.csv', '.json'}:
            continue
        copy2(arquivo, DESTINO / arquivo.name)
        print('Copiado:', arquivo.name)

# Registro de fontes do Notebook 01, criado em references/.
fontes = Path('/content/GS_SERS/references/fontes.json')
if fontes.is_file():
    copy2(fontes, DESTINO / fontes.name)
    print('Copiado:', fontes.name)

print('Arquivos em Dados/Processados:')
for arquivo in sorted(DESTINO.iterdir()):
    if arquivo.is_file():
        print('-', arquivo.name)
```

**Atenção:** copiar arquivos para `/content/Citizen-Assist-AI/Dados/Processados` **não os publica automaticamente no GitHub**. Para preservá-los, faça download, copie-os para o Google Drive ou registre-os no repositório com um commit, conforme a rotina da equipe. O armazenamento em `/content` é temporário.

**Arquivos de saída por etapa:**

| Origem | Arquivo | Conteúdo |
| --- | --- | --- |
| Notebook 01 | `epe_sp_residencial_mensal.csv` | Série residencial estadual tratada |
| Notebook 01 | `perfil_residencial_simulado.csv` | Equipamentos e consumo por equipamento |
| Notebook 01 | `resumo_consumo.json` | Resumo do cenário e custo simplificado |
| Notebook 01 | `fontes.json` | Fontes registradas pelo código |
| Notebook 02 | `resultado_geracao_fotovoltaica.csv` | Estimativas e saldo energético por mês |
| Notebook 02 | `sensibilidade_performance_ratio.csv` | Resultados para PR de 0,75, 0,80 e 0,85 |
| Notebook 02 | `resumo_potencial_solar.json` | Parâmetros solares e indicadores finais |
| Notebook 03 | `base_conhecimento_energia.json` | Base estruturada usada para orientar o assistente |
| Notebook 03 | `validacao_resultados_energia.csv` | Resultado das 14 verificações matemáticas |
| Notebook 03 | `roteiro_testes_funcionais.csv` | Perguntas F01–F09 e critérios esperados |
| Notebook 03 | `registro_testes_usuarios.csv` | Registros reais, se inseridos; caso contrário, modelo vazio |

Se os arquivos públicos originais forem grandes demais para o repositório, mantenha instruções claras de download e suas fontes de origem, respeitando as regras de distribuição dos conjuntos de dados. Não substitua arquivos brutos por resultados processados.

## Problemas frequentes

| Sintoma / mensagem | Causa provável | O que conferir |
| --- | --- | --- |
| `FileNotFoundError` para ANEEL | Parquet ausente no caminho interno | Nome do arquivo, pasta `Dados/Sem Refinamento` e cópia para `/content/GS_SERS/data/raw/` |
| Erro de aba/coluna na EPE | Versão ou formato de Excel diferente | Aba `CONSUMO E NUMCONS SAM UF` e nomes das colunas |
| `FileNotFoundError: dados/tilted_latitude_means_SP.csv` | Diretório atual errado ou CSV não copiado | `Path.cwd()` e existência de `dados/tilted_latitude_means_SP.csv` |
| `ModuleNotFoundError` | Biblioteca ausente | Executar as células `%pip install` do notebook correspondente |
| Erro ao montar o Drive | Permissão, autenticação ou ambiente diferente | Autorizar `drive.mount()` apenas no Colab |
| `GEMINI_API_KEY não encontrada` | Secret ausente ou sem acesso | Nome exato `GEMINI_API_KEY` e permissão do notebook |
| Cliente criado, mas sem resposta | Credencial, rede, modelo ou serviço | Executar a seção 3.3 e examinar o código da falha |
| `404`/modelo indisponível | Modelo não acessível ou identificador incorreto | Conferir `MODELO_GEMINI` e listar modelos disponíveis |
| `429 RESOURCE_EXHAUSTED` | Cota ou limite de requisições atingido | Desativar testes online automáticos; revisar limites da conta |
| `503 UNAVAILABLE` | Falha temporária do serviço/modelo | Não interpretar como reprovação do cálculo local; registrar a falha e verificar o serviço |
| CSV de usuários vazio | Nenhum registro inserido na execução atual | Inserir avaliações reais, executar seções 7.2 → 7.3 → 8.4 |
| Resumos anteriores não encontrados no Notebook 03 | Arquivos de saída em outra sessão ou pasta | Copiar `resumo_consumo.json` e `resumo_potencial_solar.json` para os caminhos opcionais |
| Alterações desaparecem após encerrar Colab | Dados mantidos apenas em `/content` | Salvar em Drive, baixar ou versionar resultados no GitHub |

## Limitações e segurança

**Limitações energéticas e econômicas**

- O perfil de quatro moradores e o consumo de 150,40 kWh/mês são **simulados**, não medidos.
- O consumo da EPE é estatística agregada; não representa uma residência individual.
- A tarifa **TUSD + TE** não equivale necessariamente ao valor final da fatura.
- O Atlas do INPE/LABREN fornece médias históricas; não mede as condições específicas do telhado.
- O **Performance Ratio de 0,80** é uma hipótese do estudo.
- O dimensionamento de **1,328 kWp** é preliminar; não é projeto executivo, nem garante fatura zero.
- O custo fotovoltaico é estimado com preços médios de mercado, não com orçamentos; o *payback* é só indicativo.
- As emissões evitadas usam o fator médio do SIN; a análise de ciclo de vida dos painéis não foi feita.
- Elegibilidade à Tarifa Social não é confirmada pelo estudo nem pelo assistente.
- As economias simuladas dependem da adoção real dos hábitos e das condições tarifárias.

**Limitações da IA e dos testes**

- O protótipo conversacional funciona dentro do notebook, não como aplicação pública independente.
- O comportamento da IA pode variar entre chamadas, mesmo sob as instruções de segurança.
- A API depende de chave, disponibilidade do modelo, conexão e limites de uso.
- As **14 verificações matemáticas** avaliam coerência da base; não comprovam automaticamente a correção de toda resposta generativa.
- Os cenários F01–F09 e suas respostas de referência não devem ser confundidos com um histórico completo de execução online.
- A utilidade e a compreensão por usuários devem ser relatadas com base em **avaliações efetivamente registradas**.

**Segurança e privacidade**

- Não publicar chaves de API, tokens, senhas ou arquivos com credenciais.
- Não inserir CPF, endereço completo, conta de energia identificável ou dados pessoais desnecessários nos prompts.
- Rever outputs e metadados dos arquivos `.ipynb` antes de publicar.
- Evitar comandos, recomendações ou intervenções elétricas inseguras; serviços elétricos devem ser tratados por profissionais habilitados.
- Toda estimativa deve vir acompanhada de premissas, unidades, fonte e limitações.

## Referências

**Fontes dos dados energéticos**

1. **ANEEL — Agência Nacional de Energia Elétrica.** [Dados Abertos](https://dadosabertos.aneel.gov.br/). Base de tarifas de aplicação utilizada para identificar TUSD e TE de referência.
2. **EPE — Empresa de Pesquisa Energética.** [Dados do Consumo Mensal de Energia Elétrica](https://www.epe.gov.br/pt/publicacoes-dados-abertos/dados-abertos/dados-do-consumo-mensal-de-energia-eletrica).
3. **EPE.** [Dicionário de Dados — Consumo Mensal](https://www.epe.gov.br/sites-pt/publicacoes-dados-abertos/dados-abertos/Documents/Consumo-Mensal-Dicionario-de-Dados.pdf).
4. **PEREIRA, E. B. et al.** *Atlas Brasileiro de Energia Solar*. 2ª ed. INPE, 2017. [INPE/LABREN — Atlas](https://labren.ccst.inpe.br/atlas_2017.html).
5. **INPE/LABREN.** [Tabela de irradiação no plano inclinado — São Paulo](https://labren.ccst.inpe.br/atlas2_tables/SP_inc-en.html).

**Impactos ambientais**

6. **IRENA; IEA-PVPS.** *End-of-Life Management: Solar Photovoltaic Panels*. 2016. [Publicação](https://www.irena.org/publications/2016/Jun/End-of-life-management-Solar-Photovoltaic-Panels).
7. **IPCC.** *Climate Change 2022: Mitigation of Climate Change*. Working Group III, Sixth Assessment Report. [Relatório](https://www.ipcc.ch/report/ar6/wg3/).
8. **MCTI.** [Fatores de emissão de CO₂ do Sistema Interligado Nacional — SIRENE](https://www.gov.br/mcti/pt-br/acompanhe-o-mcti/sirene); [notícia dos fatores de 2025](https://www.gov.br/mcti/pt-br/acompanhe-o-mcti/noticias/2025/06/mcti-publica-fatores-de-emissao-de-co2-da-geracao-de-energia-eletrica-no-brasil-para-2025). Consultado em 13/11/2026.

**Custos e Tarifa Social**

9. **Cenário Energia.** [Preço do watt-pico residencial recua 7% em um ano (Radar Solfácil, 1º tri 2026)](https://cenarioenergia.com.br/2026/06/09/preco-do-watt-pico-residencial-recua-7-em-um-ano-e-abre-janela-de-oportunidade-na-mmgd/), 09/06/2026.
10. **Olhar Digital.** [Preços médios da Greener, Estudo Estratégico Geração Distribuída 2025](https://olhardigital.com.br/curiosidades/?p=10663).
11. **ANEEL.** [Tarifa Social de Energia Elétrica](https://www.gov.br/aneel/pt-br/assuntos/tarifas/tarifa-social).
12. **MME.** [Programa Luz do Povo — Tarifa Social e Desconto Social](https://www.gov.br/mme/pt-br/luzdopovo).

**Documentação de desenvolvimento**

13. **Google AI for Developers.** [Gemini API — Modelos](https://ai.google.dev/gemini-api/docs/models).
14. **Google.** [Google Gen AI SDK para Python](https://googleapis.github.io/python-genai/).
15. **Google AI Studio.** [Gerenciamento de chaves da API](https://aistudio.google.com/app/apikey).
16. **Google.** [Exemplo oficial de autenticação no Colab](https://colab.research.google.com/github/google-gemini/cookbook/blob/main/quickstarts/Authentication.ipynb).

As referências justificam a metodologia e a discussão qualitativa. Parâmetros como o consumo doméstico simulado, a redução de 20% e o PR adotado são **premissas do projeto**, não resultados diretamente medidos pelas instituições citadas.

---

**Citizen Assist AI — FIAP, Global Solution 2026.2.** Protótipo acadêmico de orientação energética baseado em dados públicos, simulação residencial e inteligência artificial generativa.
