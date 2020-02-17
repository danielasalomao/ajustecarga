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




## ZRFI_PARTIDAS_ABERTAS_CPY


### INCLUDE ZRFI_PARTIDAS_ABERTASTOP_CPY.

    

### INCLUDE ZRFI_PARTIDAS_ABERTASF01_CPY.

####  TELA DE SELEÇÃO

  [Código](https://github.com/danielasalomao/ajustecarga/blob/master/teladeselecao.txt)

####  PERFORM:

##### FORM F_UPLOAD

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_upload.txt)

##### FORM F_TVARV

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_tvarv.txt)

##### FORM F_TRATA_ARQUIVO

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_trata_arquivo.txt)
 
 * **Funções chamadas:**<br>
    - TH_ARFC_LOCAL_RESOURCES - 183, 394
    
    [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20TH_ARFC_LOCAL_RESOURCES.txt)
    
    - ZFFI_CARGA_PARTIDA_ABERTA - 191, 402  
    
    [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20zffi_carga_partida_aberta.txt)
    
    - CONVERSION_EXIT_ALPHA_INPUT - 259, 327   
    
    [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20CONVERSION_EXIT_ALPHA_INPUT.txt)
 

##### FORM F_FIM_BAPI

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_fim_bapi.txt)
 
  * **Funções:**<br>
   - Recebe resultados da ZFFI_CARGA_PARTIDA_ABERTA - 464

##### FORM F_TRATA_ARQUIVO_VERIFICA_ERRO

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_trata_arquivo_verfifica_erro.txt)

#### FORM_F_CRIA_ALV

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_cria_alv.txt)
 
   * **Funções chamadas:**<br>
   - REUSE_ALV_GRID_DISPLAY - 590
   
   [Código]( https://github.com/danielasalomao/ajustecarga/blob/master/function%20reuse_alv_grid_display.txt)

##### FORM F_SET_FIELDCAT

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_set_fieldcast.txt)
 
##### FORM F_SORT_TABLE

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_sort_table.txt)

##### FORM F_LIMPA_VAR

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_limpa_var.txt)

##### FORM F_CONVERSION_EXIT_ALPHA_INPUT

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_conversion_exit_alpha_input.txt)

##### FORM F_CONVERSION_EXIT_SDATE_INPUT

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_conversion_exit_sdate_input.txt)

##### FORM F_FORMATAR_DADOS_ARQUIVO

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_formatar_dados_arquivo.txt)
 
 ##### FORM exibir_popup_erros
 
  [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_exibir_popup_erros.txt)
  
  * **Funções chamadas:**<br>
   - CONVERSION_EXIT_ALPHA_INPUT - 950
   - POPUP_WITH_TABLE_DISPLAY_OK - 968
   
       [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20POPUP_WITH_TABLE_DISPLAY_OK.txt)
 
 ##### FORM user_command
 
  [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_user_command.txt)
    
