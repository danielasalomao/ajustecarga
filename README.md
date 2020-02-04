# 9000075165 - Ajuste programa de carga de doc contabil

### DESCRIÇÃO DOS DADOS
---
#### TABELAS 

##### FPESQUISA
* **ID_Processador:** Identificador do processador.<br>
    - Tipo de dados: Número Decimal.
* **ID_Notificador:** Identificador do notificador.<br>
    - Tipo de dados: Número Decimal.
* **ID_SSI:** Identificador da SSI(chamado).<br>
    - Tipo de dados: Número Decimal.
* **Data_Criacao_SSI:** Armazena a data que a SSI foi aberta.<br>
    - Tipo de dados: Qualquer.
* **Data_Encerramento_SSI:** Armazena a data que a SSI foi encerrada.<br>
    - Tipo de dados: Qualquer.

#### CAMPOS

budat	data de lançamento
monat	periodo
bukrs	empresa
werks	centro
blart	tipo de documento
bldat	data de documento
waers	moeda
kursf(12)	taxa de conversao
bktxt	texto cabeçalho
xblnr	referencia
bschl	chave de lancamento
newko	conta contabil
newnum	indicacao do razao
wrbtr	montante
zfbdt	data base
zterm	Cond. de pagamento
matnr	material
kostl	centro de custo
prctr	centro de lucro
aufnr	ordem interna
zuonr	atribuicao
sgtxt	texto do item
bupla	local de negocio
ebeln	documento de material
xref3	chav referencia
anfbn	nsolicitacao
anfbu	empresa solicitação
anfbj	ano solicitacao
hbkid	banco empresa
zlsch	forma de pagamento


##### DNOTIFICADOR: Armazena informacoes relativas ao notificador (pessoa que abre a SSI).<br>
* **ID_Notificador:** Identificador do notificador.<br>
    - Tipo de dados: Número Decimal.
* **Nome:** Armazena nome do notificador.<br>
    - Tipo de dados: Texto.
* **Departamento:** Armazena o departamento que o usuário pertence.<br>
    - Tipo de dados: Texto.
* **Diretoria:** Armazena a diretoria que o funcionário trabalha.<br>
    - Tipo de dados: Texto.
* **Empresa:** Armazena nome da empresa.<br>
    - Tipo de dados: Texto.
* **Usuario_Chave:** É usuário chave?<br>
    - Tipo de dados: 
* **Gestor:** É gestor? .<br>
    - Tipo de dados:

## ZRFI_PARTIDAS_ABERTAS_CPY


### INCLUDE ZRFI_PARTIDAS_ABERTASTOP_CPY.

    

### INCLUDE ZRFI_PARTIDAS_ABERTASF01_CPY.

#### * TELA DE SELEÇÃO

#### * PERFORM:

##### FORM F_UPLOAD

##### FORM F_TVARV

##### FORM F_TRATA_ARQUIVO

##### FORM F_FIM_BAPI

##### FORM F_TRATA_ARQUIVO_VERIFICA_ERRO

##### FORM_F_CRIA_ALV

##### FORM F_SET_FIELDCAT

##### FORM F_SORT_TABLE

##### FORM F_LIMPA_VAR

##### FORM F_CONVERSION_EXIT_ALPHA_INPUT

##### FORM F_CONVERSION_EXIT_SDATE_INPUT

##### FORM F_FORMATAR_DADOS_ARQUIVO
    
    
