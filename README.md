# 9000075165 - Ajuste programa de carga de doc contabil

## CÓDIGO

### 1 - ZRFI_PARTIDAS_ABERTAS_CPY

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/ZRFI_PARTIDAS_ABERTAS_cpy.txt)

#### 1.1 - INCLUDE ZRFI_PARTIDAS_ABERTASTOP_CPY.

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/Include_ZRFI_PARTIDASTOP_CPY.txt)  
 
* Report
* Types
* Ty_arq - campos planilha excel
* Ty_alv
* Tabelas Internas
* Work Areas
* Estruturas
* Variáveis
* Constantes
* Objetos

#### 1.2 - INCLUDE ZRFI_PARTIDAS_ABERTASF01_CPY.

##### A - TELA DE SELEÇÃO

  [Código](https://github.com/danielasalomao/ajustecarga/blob/master/teladeselecao.txt)

##### B - FORM F_UPLOAD

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_upload.txt)
 
 * Lê o arquivo.

##### C - FORM F_TVARV

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_tvarv.txt)
 
 * Recebe valores da TVARV.

##### D - FORM F_TRATA_ARQUIVO

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_trata_arquivo.txt)
 
 * **Funções chamadas:**<br>
    - TH_ARFC_LOCAL_RESOURCES - 183, 394
    
    [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20TH_ARFC_LOCAL_RESOURCES.txt)
    
    - ZFFI_CARGA_PARTIDA_ABERTA - 191, 402  
    
    [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20zffi_carga_partida_aberta.txt)
    
    - CONVERSION_EXIT_ALPHA_INPUT - 259, 327   
    
    [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20CONVERSION_EXIT_ALPHA_INPUT.txt)
 

##### E - FORM F_FIM_BAPI

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_fim_bapi.txt)
 
  * **Funções:**<br>
      - Recebe resultados da ZFFI_CARGA_PARTIDA_ABERTA - 464

##### F - FORM F_TRATA_ARQUIVO_VERIFICA_ERRO

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_trata_arquivo_verfifica_erro.txt)
 
 * Trata arquivo
 * Verifica se algum documento contem erro.
 * Commit apenas se todos ok.

##### G - FORM_F_CRIA_ALV

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_cria_alv.txt)
 
   * **Funções chamadas:**<br>
   
       - REUSE_ALV_GRID_DISPLAY - 590
   
   [Código]( https://github.com/danielasalomao/ajustecarga/blob/master/function%20reuse_alv_grid_display.txt)

##### H - FORM F_SET_FIELDCAT

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_set_fieldcast.txt)
 
##### I - FORM F_SORT_TABLE

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_sort_table.txt)

##### J - FORM F_LIMPA_VAR

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/form_f_limpa_var.txt)
 
 * Limpa variáveis e tabelas internas.

##### K - FORM F_CONVERSION_EXIT_ALPHA_INPUT

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_conversion_exit_alpha_input.txt)
 
 * **Funções chamadas:**<br>
    - CONVERSION_EXIT_ALPHA_INPUT - 808
   

##### L - FORM F_CONVERSION_EXIT_SDATE_INPUT

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_conversion_exit_sdate_input.txt)
 
 * **Funções chamadas:**<br>
    - CONVERSION_EXIT_SDATE_INPUT - 825
    
      [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20CONVERSION_EXIT_SDATE_INPUT.txt)

##### M - FORM F_FORMATAR_DADOS_ARQUIVO

 [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_formatar_dados_arquivo.txt)
 
##### N - FORM exibir_popup_erros
 
  [Código](https://github.com/danielasalomao/ajustecarga/blob/master/f_exibir_popup_erros.txt)
```abap
  
  
  
  
  FORM exibir_popup_erros USING lv_rownumber.
  DATA: idd07v TYPE TABLE OF  dd07v WITH HEADER LINE.
  DATA lv_choice TYPE string.
  DATA it_textoerros TYPE TABLE OF dd07v-ddtext.
  FIELD-SYMBOLS <fs_textoerros> TYPE dd07v-ddtext.
  DATA lv_nlinhas TYPE i.
  DATA lv_cont TYPE i.
  DATA lv_saknr TYPE skat-saknr.
  DATA lv_txt20 TYPE skat-txt20.

  READ TABLE it_alv INTO wa_alv INDEX lv_rownumber.
  IF sy-subrc IS INITIAL.

    SPLIT wa_alv-message AT ';' INTO TABLE it_textoerros.

    DELETE it_textoerros INDEX 1.
    LOOP AT it_textoerros ASSIGNING <fs_textoerros>.

      AT LAST.
        IF <fs_textoerros>(9) = 'Descrição'.
          IF p_teste IS INITIAL.

            CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
              EXPORTING
                input  = wa_alv-newko
              IMPORTING
                output = lv_saknr.

            SELECT SINGLE txt20 FROM skat
             INTO lv_txt20
             WHERE saknr = lv_saknr.
            <fs_textoerros> = |Descrição: { lv_txt20 }|.
          ENDIF.

        ENDIF.
      ENDAT.
      <fs_textoerros> = |{ sy-tabix }. { <fs_textoerros> }|.
    ENDLOOP.

    lv_nlinhas = lines( it_textoerros ).
    CALL FUNCTION 'POPUP_WITH_TABLE_DISPLAY_OK'
      EXPORTING
        endpos_col   = 70
        endpos_row   = lv_nlinhas
        startpos_col = 1
        startpos_row = 1
        titletext    = 'Exibição dos erros para linha selecionada'
      TABLES
        valuetab     = it_textoerros
      EXCEPTIONS
        break_off    = 1
        OTHERS       = 2.
    IF sy-subrc <> 0.
      MESSAGE 'Erro ao exibir popup' TYPE 'E'.
    ENDIF.
  ENDIF.

ENDFORM.
  
  
```
  
  * **Funções chamadas:**<br>
     - CONVERSION_EXIT_ALPHA_INPUT - 950
     - POPUP_WITH_TABLE_DISPLAY_OK - 968   
       [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20POPUP_WITH_TABLE_DISPLAY_OK.txt)

```abap
FUNCTION POPUP_WITH_TABLE_DISPLAY_OK.
*"----------------------------------------------------------------------
*"*"Local interface:
*"       IMPORTING
*"             VALUE(ENDPOS_COL)
*"             VALUE(ENDPOS_ROW)
*"             VALUE(STARTPOS_COL)
*"             VALUE(STARTPOS_ROW)
*"             VALUE(TITLETEXT)
*"       EXPORTING
*"             VALUE(CHOISE) LIKE  SY-TABIX
*"       TABLES
*"              VALUETAB
*"       EXCEPTIONS
*"              BREAK_OFF
*"----------------------------------------------------------------------
  REFRESH LISTTAB. CLEAR LISTTAB.

  LOOP AT VALUETAB.
    LISTTAB = VALUETAB.
    APPEND LISTTAB.
  ENDLOOP.

  TITLE_TEXT = TITLETEXT.
  CALL SCREEN 0200 STARTING AT STARTPOS_COL STARTPOS_ROW
                   ENDING   AT ENDPOS_COL   ENDPOS_ROW.
  CHOISE = HLINE.
ENDFUNCTION.


AT USER-COMMAND.
  CASE SY-UCOMM.
    WHEN 'SLCT'.
      GET CURSOR FIELD HFIELD LINE HLINE.
      IF SY-SUBRC = 0.
        SET SCREEN 0.
        LEAVE SCREEN.
      ENDIF.
    WHEN 'ABR'.
      SET SCREEN 0.
      RAISE BREAK_OFF.
      LEAVE SCREEN.
  ENDCASE.

*---------------------------------------------------------------------*
*       MODULE SET_TITLE                                              *
*---------------------------------------------------------------------*
*       Set the title given by calling program                        *
*---------------------------------------------------------------------*

MODULE SET_TITLE OUTPUT.

  SET TITLEBAR '002' WITH TITLE_TEXT.

ENDMODULE.


*---------------------------------------------------------------------*
*       MODULE LISTPROCESSING OUTPUT                                  *
*---------------------------------------------------------------------*
*       Switchs to list-processing and displays internal table data   *
*---------------------------------------------------------------------*
MODULE LISTPROCESSING OUTPUT.
  LEAVE TO LIST-PROCESSING AND RETURN TO SCREEN 0000.
  SET PF-STATUS 'EXTR'.
  NEW-PAGE NO-TITLE .
  LOOP AT LISTTAB.
    IF LISTTAB+15(1) = 'S' OR LISTTAB+15(1) = 'F'.
      IF LISTTAB+15(1) = 'S'.
        WRITE: / LISTTAB+0(15), ICON_OKAY AS ICON.
      ELSEIF LISTTAB+15(1) = 'F'.
        WRITE: / LISTTAB+0(15), ICON_CANCEL AS ICON.
      ENDIF.
    ELSE.
      WRITE: / LISTTAB.
    ENDIF.
  ENDLOOP.
  LEAVE SCREEN.

ENDMODULE.
*&---------------------------------------------------------------------*
*&      Module  SUPPRESS_DIALOG  OUTPUT
*&---------------------------------------------------------------------*
*       text                                                           *
*----------------------------------------------------------------------*
MODULE SUPPRESS_DIALOG OUTPUT.
  SUPPRESS DIALOG.
ENDMODULE.                             " SUPPRESS_DIALOG  OUTPUT
```


 ##### O - FORM user_command
 
 
```abap
FORM user_command USING ucomm    LIKE sy-ucomm
                        selfield TYPE slis_selfield.

  selfield-row_stable = 'X'.

  CASE ucomm.
    WHEN '&IC1'.
*      if selfield-fieldname = 'ERRO'.
      PERFORM exibir_popup_erros USING selfield-tabindex.
*      endif.
  ENDCASE.
  CLEAR ucomm.

ENDFORM.                    "user_command
```
    
---    

## CAMPOS

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
