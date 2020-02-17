# 9000075165 - Ajuste programa de carga de doc contabil

#### CAMPOS

Nome Tecnico  |Tipo	        |Descrição 	          |Exemplo
:-----------: | :----------:|:-------------------:|:-----------:
anfbj	    |bseg-anfbj	|ano solicitacao	|null 
anfbn	    |bseg-anfbn	|nsolicitacao	|null 
anfbu	    |bseg-anfbu	|empresa solicitação	|null 
aufnr	    |bseg-aufnr	|ordem interna	|null 
bktxt	    |bkpf-bktxt	|texto cabeçalho	|null 
blart	    |bkpf-blart	|tipo de documento	|AB 
bldat	    |bkpf-bldat	|data de documento	|31122019 
bschl	    |bseg-bschl	|chave de lancamento	|40 
budat	    |bkpf-budat	|data de lançamento	|31122019 
bukrs	    |bkpf-bukrs	|empresa	|COMP 
bupla	    |bseg-bupla	|local de negocio	|0001 
ebeln	    |bseg-ebeln	|documento de material	|null	 
hbkid	    |bseg-hbkid	|banco empresa	|null 
kostl	    |bseg-kostl	|centro de custo	|null 
kursf(12)	|c	|taxa de conversao	|null 
matnr	    |bseg-matnr	|material|	
monat	    |bkpf-monat	|periodo	|12 
newko	    |bbseg-newko	|conta contabil	|113010001 
newnum	    |bbseg-newum	|indicacao do razao	|null 
prctr	    |bseg-prctr	|centro de lucro	|null 
sgtxt	    |bseg-sgtxt	|texto do item	|VLR TRANSF ICMS NF 221331
waers	    |bkpf-waers	|moeda|	BRL 
werks	    |bseg-werks	|centro	|CM01 
wrbtr	    |bseg-wrbtr	|montante|	261.04 
xblnr	    |bkpf-xblnr|	referencia|	221331 
xref3	    |bseg-xref3	|chav referencia	|null 
zfbdt	    |bseg-zfbdt|	data base	|31122019 
zlsch	    |bseg-zlsch|	forma de pagamento|	null 
zterm	    |bseg-zterm	|Cond. de pagamento	|null 
zuonr	    |bseg-zuonr|	atribuicao|	221331 


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
    
    
