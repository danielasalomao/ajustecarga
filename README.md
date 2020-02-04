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

Nome Tecnico	Tipo	Descrição 	          Exemplo
:----------- | ----------|-------------------|-----------:
anfbj	    |bseg-anfbj	|ano solicitacao	|null 



.<BR>
anfbn	bseg-anfbn	nsolicitacao	null <br>
anfbu	bseg-anfbu	empresa solicitação	null <br>
aufnr	bseg-aufnr	ordem interna	null <br>
bktxt	bkpf-bktxt	texto cabeçalho	null <br>
blart	bkpf-blart	tipo de documento	AB <br>
bldat	bkpf-bldat	data de documento	31122019 <br>
bschl	bseg-bschl	chave de lancamento	40 <br>
budat	bkpf-budat	data de lançamento	31122019 <br>
bukrs	bkpf-bukrs	empresa	COMP <br>
bupla	bseg-bupla	local de negocio	0001 <br>
ebeln	bseg-ebeln	documento de material	null <br>
end of ty_arq			 <br>
hbkid	bseg-hbkid	banco empresa	null <br>
kostl	bseg-kostl	centro de custo	null <br>
kursf(12)	c	taxa de conversao	null <br>
matnr	bseg-matnr	material	 <br>
monat	bkpf-monat	periodo	12 <br>
newko	bbseg-newko	conta contabil	113010001 <br>
newnum	bbseg-newum	indicacao do razao	null <br>
prctr	bseg-prctr	centro de lucro	null <br>
sgtxt	bseg-sgtxt	texto do item	VLR TRANSF ICMS NF 221331 <br>
waers	bkpf-waers	moeda	BRL <br>
werks	bseg-werks	centro	CM01 <br>
wrbtr	bseg-wrbtr	montante	261.04 <br>
xblnr	bkpf-xblnr	referencia	221331 <br>
xref3	bseg-xref3	chav referencia	null <br>
zfbdt	bseg-zfbdt	data base	31122019 <br>
zlsch	bseg-zlsch	forma de pagamento	null <br>
zterm	bseg-zterm	Cond. de pagamento	null <br>
zuonr	bseg-zuonr	atribuicao	221331 <br>


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
    
    
