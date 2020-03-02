# 9000075165 - Ajuste programa de carga de doc contabil

## CÓDIGO

### 1 - ZRFI_PARTIDAS_ABERTAS_CPY

```abap

*----------------------------------------------------------------------*
* Includes
*----------------------------------------------------------------------*

INCLUDE ZRFI_PARTIDAS_ABERTASTOP_CPY.
*INCLUDE zrfi_partidas_abertastop.
*INCLUDE zrfi_partidas_abertastop.  " TOP Include
INCLUDE ZRFI_PARTIDAS_ABERTASF01_CPY.
*INCLUDE zrfi_partidas_abertasf01.
*INCLUDE zrfi_partidas_abertasf01.  " FORMs Include

*----------------------------------------------------------------------*
* START-OF-SELECTION
*----------------------------------------------------------------------*
* Processamento principal do programa
*----------------------------------------------------------------------*
START-OF-SELECTION.

  " Limpa variáveis e tabelas internas.
  PERFORM f_limpa_var.

  " Recebe valores da TVARV
  PERFORM f_tvarv.

  " Lê o arquivo.
  PERFORM f_upload.

  " Trata o arquivo e grava na tabela.
 " PERFORM f_trata_arquivo.

  "Trata arquivo
  "Verifica se algum documento contem erro.
  "Commit apenas se todos ok.
  PERFORM f_trata_arquivo_verifica_erro.

"  WAIT UNTIL vg_rcv_jobs EQ vg_snd_jobs.



  " Chama a tela do ALV com os documentos NÃO gravados.
  PERFORM f_cria_alv.

* Elementos de texto
*----------------------------------------------------------
* F01	Status
* F02	Data no documento
* F03	Empresa
* F04	Data de lançamento no documento
* F05	Mês do exercício
* F06	Tipo de documento
* F07	Moeda
* F08	Nº documento de referência
* F09	Texto de cabeçalho de documento
* F10	Chave de lançamento
* F11	Conta
* F12	Montante
* F13	Taxa de conversão
* F14	Indicação de Razão Especial
* F15	Data base
* F16	Centro de Custo
* F17	Centro de Lucro
* F18	Ordem Interna
* F19	Atribuição
* F20	Texto do Item
* F21	Mensagem de Log

* Textos de seleção
*----------------------------------------------------------
* P_PATH Caminho

*BCI:RRUBINO: Melhoria carga ZRFI_PARTIDAS_ABERTAS - 17.01.2019 - Inicio
AT SELECTION-SCREEN ON VALUE-REQUEST FOR p_path.
  DATA gd_subrc TYPE i.
  DATA it_file TYPE filetable.

  CALL METHOD cl_gui_frontend_services=>file_open_dialog
    EXPORTING
      window_title            = 'Selecione o arquivo de carga no formato csv'
      default_extension       = '*.csv'
      file_filter             = 'Separado por ; (*.csv)|*.csv|'
    CHANGING
      file_table              = it_file
      rc                      = gd_subrc
    EXCEPTIONS
      file_open_dialog_failed = 1
      cntl_error              = 2
      error_no_gui            = 3
      OTHERS                  = 4.
  IF sy-subrc = 0.
    READ TABLE it_file INDEX 1 INTO p_path.
  ENDIF.
```

#### 1.1 - INCLUDE ZRFI_PARTIDAS_ABERTASTOP_CPY.
 
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

```abap
REPORT zrfi_partidas_abertas.

*----------------------------------------------------------------------*
* Types
*----------------------------------------------------------------------*
TYPES:


***campos da planilha do excel
  BEGIN OF ty_arq,


***data de lançamento: 31122019
    budat     TYPE bkpf-budat,

***periodo:   12
    monat     TYPE bkpf-monat,

***empresa: COMP
    bukrs     TYPE bkpf-bukrs,

* SSI - 1000017494
***centro:CM01
***Centro : Campo WERKS da tabela BSEG. Tem o objetivo de informar qual o centro se refere nos lançamentos contábeis em contas de estoque;
    werks     TYPE bseg-werks,
*

***tipo de documento: AB
    blart     TYPE bkpf-blart,

***data de documento: 31122019
    bldat     TYPE bkpf-bldat,

***moeda: BRL
    waers     TYPE bkpf-waers,

***taxa de conversao: null
    kursf(12) TYPE c, "bkpf-KURSF,

***texto cabeçalho: null
    bktxt     TYPE bkpf-bktxt,

***referencia: 221331
    xblnr     TYPE bkpf-xblnr,

***chave de lancamento: 40
    bschl     TYPE bseg-bschl,

***conta contabil: 113010001
    newko     TYPE bbseg-newko,

***indicacao do razao: null
    newnum    TYPE bbseg-newum,

***montante: 261.04
    wrbtr     TYPE bseg-wrbtr,

***data base: 31122019
    zfbdt     TYPE bseg-zfbdt,

* SSI - 1000017494
***cond pag: null
***•  Cond. De Pagamento: Campo ZTERM da tabela BSEG. O objetivo da inclusão desse campo ao layout é que os vencimentos das partidas
***   sejam calculados pelo SAP automaticamente já que a Data Base já está disponível no layout atual.
    zterm     TYPE bseg-zterm,

*SSI - 1000017494
*** •	Material: Campo MATNR da tabela BSEG. O objetivo da inclusão desse campo no layout é identificar o material referente
***   ao lançamento efetuado nas contas de estoques/resultado.
    matnr     TYPE bseg-matnr,


***centro de custo: null
    kostl     TYPE bseg-kostl,

***centro de lucro: null
    prctr     TYPE bseg-prctr,

***ordem interna: null
    aufnr     TYPE bseg-aufnr,

***atribuicao: 221331
    zuonr     TYPE bseg-zuonr,

***texto do item: VLR TRANSF ICMS NF 221331
    sgtxt     TYPE bseg-sgtxt,

***local de negocio: 0001
    bupla     TYPE bseg-bupla, "novos campos ->.

* SSI - 1000017494
***•  Documento de Material: Campo EBELN da tabela BSEG. Tem o objetivo de informar o documento de material para lançamentos
***   que se referem a entradas de mercadoria;  null
    ebeln     TYPE bseg-ebeln,
*

***chav referencia: null
    xref3     TYPE bseg-xref3,

***nsolicitacao: null
    anfbn     TYPE bseg-anfbn,

***empresa solicitação: null
    anfbu     TYPE bseg-anfbu,

***ano solicitacao: null
    anfbj     TYPE bseg-anfbj,

***banco empresa: null
    hbkid     TYPE bseg-hbkid,

***forma de pagamento: null
    zlsch     TYPE bseg-zlsch, "novos campos <-.

  END OF ty_arq,



***final campos da planilha excel



  BEGIN OF ty_alv,
    stats   TYPE icon-id,          " Status
    bukrs   TYPE bkpf-bukrs,       " Empresa
    bldat   TYPE bkpf-bldat,       " Data no documento
* SSI - 1000017494
    werks   TYPE bseg-werks,       "Centro
    budat   TYPE bkpf-budat,       " Data de lançamento no documento
    monat   TYPE bkpf-monat,       " Mês do exercício
    blart   TYPE bkpf-blart,       " Tipo de documento
    waers   TYPE bkpf-waers,       " Moeda
    xblnr   TYPE bkpf-xblnr,       " Nº documento de referência
    bktxt   TYPE bkpf-bktxt,       " Texto de cabeçalho de documento
    bschl   TYPE bseg-bschl,       " Chave de lançamento
    newko   TYPE rf05a-newko,      " Conta ou matchcode
    wrbtr   TYPE bseg-wrbtr,       " Montante
    kursf   TYPE bkpf-kursf,       " Taxa de conversão
    newnum  TYPE bbseg-newum,      " Indicação de Razão Especial
    zfbdt   TYPE bseg-zfbdt,       " Data base
* SSI - 1000017494
    zterm   TYPE bseg-zterm,     "Cond. de Pagamento
    matnr   TYPE bseg-matnr,     " Material
    kostl   TYPE bseg-kostl,       " Centro de Custo
    prctr   TYPE bseg-prctr,       " Centro de Lucro
    aufnr   TYPE bseg-aufnr,       " Ordem Interna
    zuonr   TYPE bseg-zuonr,       " Atribuição
    sgtxt   TYPE bseg-sgtxt,       " Texto do Item
    bupla   TYPE bseg-bupla,       " Local de negócios
* SSI - 1000017494
    ebeln   TYPE bseg-ebeln,     " Documento de material
    xref3   TYPE bseg-xref3,       " Chave de referência para item de doc.
    anfbn   TYPE bseg-anfbn,       " Nº Solicitação
    anfbu   TYPE bseg-anfbu,       " Empresa da Solicitação de L/C
    anfbj   TYPE bseg-anfbj,       " Ano da Solicitação de L/C
    hbkid   TYPE bseg-hbkid,       " Banco empresa
    zlsch   TYPE bseg-zlsch,       " Forma de pagamento
    message TYPE c LENGTH 300,     " Log Message
    belnr   TYPE bkpf-belnr,       "Nº documento
    gjahr(4)   type c,              "Exercicio
    message_resumo TYPE bapiret2-message, " Log Message

  END OF ty_alv.


*----------------------------------------------------------------------*
* Tabelas Internas
*----------------------------------------------------------------------*

DATA:
  it_alv               TYPE TABLE OF ty_alv,
  wa_alv               TYPE ty_alv, "BCI:RRUBINO: Melhoria carga ZRFI_PARTIDAS_ABERTAS
  it_arq               TYPE TABLE OF ty_arq,
  it_fieldcat          TYPE slis_t_fieldcat_alv,
  it_accountgl         TYPE TABLE OF bapiacgl09,
  it_curr_amount       TYPE TABLE OF bapiaccr09,
  it_accountpayable    TYPE TABLE OF bapiacap09,
  it_accountreceivable TYPE TABLE OF bapiacar09,
  it_extension2        TYPE TABLE OF bapiparex,
  it_sort              TYPE slis_t_sortinfo_alv.


FIELD-SYMBOLS <fs_alv> TYPE ty_alv. "BCI:RRUBINO: Melhoria carga ZRFI_PARTIDAS_ABERTAS
*----------------------------------------------------------------------*
* Work Areas
*----------------------------------------------------------------------*
DATA:
  wa_doc_header        TYPE bapiache09,
  wa_curr_amount       LIKE LINE OF it_curr_amount,
  wa_accountgl         LIKE LINE OF it_accountgl,
  wa_accountpayable    LIKE LINE OF it_accountpayable,
  wa_accountreceivable LIKE LINE OF it_accountreceivable,
  wa_extension2        LIKE LINE OF it_extension2,
  wa_sort              TYPE slis_sortinfo_alv.

*----------------------------------------------------------------------*
* Estruturas
*----------------------------------------------------------------------*
DATA:
  st_layout TYPE slis_layout_alv.

*----------------------------------------------------------------------*
* Variáveis
*----------------------------------------------------------------------*
DATA:
  gv_cont(4)  TYPE n,
  gv_sessao   TYPE i,

  vg_snd_jobs TYPE i VALUE 0, "Jobs enviado
  vg_rcv_jobs TYPE i VALUE 0, "Jobs executados
  vg_act_jobs TYPE i VALUE 0, "Jobs Ativos
  vg_free     TYPE i,
  vg_th_cont  TYPE char10,
  vg_task_id  TYPE string.

*----------------------------------------------------------------------*
* Constantes
*----------------------------------------------------------------------*

CONSTANTS:

  " Fornecedor
  c_ch_cred_fornec(2)  TYPE c VALUE '21',
  c_ch_razao_fornec(2) TYPE c VALUE '29',
  c_ch_fat_fornec(2)   TYPE c VALUE '31',

  " Saldo GL
  c_ch_lanc_cred(2)    TYPE c VALUE '50',
  c_ch_lanc_deb(2)     TYPE c VALUE '40',

  " Clientes
  c_ch_fat_cli(2)      TYPE c VALUE '01',
  c_ch_cred_cli(2)     TYPE c VALUE '11',
  c_ch_razao_cli(2)    TYPE c VALUE '19',

  " TVARV
  c_prefixo            TYPE string VALUE 'Z',
  c_separador          TYPE char01 VALUE '_',
  c_sessao             TYPE string VALUE 'FI_NUM_SESSAO',
  c_task               TYPE char10 VALUE 'TASK'.

*----------------------------------------------------------------------*
* Objetos
*----------------------------------------------------------------------*
DATA:
  o_tvarv TYPE REF TO zcl_tvarv.

DATA gv_iserror TYPE c.
```

#### 1.2 - INCLUDE ZRFI_PARTIDAS_ABERTASF01_CPY.

##### A - TELA DE SELEÇÃO

  ```abap
  SELECTION-SCREEN BEGIN OF BLOCK b1 WITH FRAME TITLE text-001.     ""Selecione o arquivo.
PARAMETERS: p_path  TYPE string OBLIGATORY,   " Caminho
            p_teste AS CHECKBOX TYPE c DEFAULT ' '.
SELECTION-SCREEN END OF BLOCK b1.
```

##### B - FORM F_UPLOAD
 
 * Lê o arquivo.
 
 ```abap
 FORM f_upload.

*  DATA: l_string  TYPE string,
*        l_caminho TYPE string.
*
*  CLEAR: l_string, l_caminho.
*
*  l_caminho = p_path.
*
*  OPEN DATASET l_caminho FOR INPUT IN
*                         TEXT MODE ENCODING
*                         DEFAULT WITH SMART LINEFEED.
*
*  WHILE sy-subrc IS INITIAL.
*    READ DATASET l_caminho INTO l_string.
*    CHECK sy-subrc IS INITIAL.
*    CHECK sy-index GT 1. " Desprezar header
*    PERFORM f_formatar_dados_arquivo USING l_string.
*    CLEAR: l_string.
*  ENDWHILE.
*
*  CLOSE DATASET l_caminho.
*
*  IF it_arq IS INITIAL.
*    MESSAGE text-002 TYPE 'S' DISPLAY LIKE 'E'.
*    LEAVE LIST-PROCESSING.
*  ENDIF.

*BCI:RRUBINO: Melhoria carga ZRFI_PARTIDAS_ABERTAS - 17.01.2019 - Inicio

  DATA: it_datatab TYPE table_of_strings,
        wa_datatab LIKE LINE OF it_datatab,
        ld_lines   TYPE i,
        ld_file    TYPE string,
        l_string   TYPE string.

  ld_file = p_path.

  CALL METHOD cl_gui_frontend_services=>gui_upload
    EXPORTING
      filename = ld_file
      filetype = 'ASC'
    CHANGING
      data_tab = it_datatab
    EXCEPTIONS
      OTHERS   = 1.

  IF sy-subrc = 0.
    IF it_datatab IS INITIAL.
      MESSAGE text-002 TYPE 'S' DISPLAY LIKE 'E'.
***Arquivo não contem registros / Tipo S??? Display like???
      LEAVE LIST-PROCESSING.
    ELSE.
      LOOP AT it_datatab INTO wa_datatab.
        CHECK sy-tabix GT 1. " Desprezar header
        l_string = wa_datatab.
        PERFORM f_formatar_dados_arquivo USING l_string.
      ENDLOOP.
    ENDIF.
  ELSE.
    MESSAGE text-007 TYPE 'S' DISPLAY LIKE 'E'.
***Erro ao abrir arquivo  / Tipo S??? Display like???
    LEAVE LIST-PROCESSING.
  ENDIF.
*BCI:RRUBINO: Melhoria carga ZRFI_PARTIDAS_ABERTAS - 17.01.2019 - Fim
ENDFORM.
 
 ```

##### C - FORM F_TVARV
 
 * Recebe valores da TVARV.
 
 ```abap
 FORM f_tvarv .

  " Se o objeto ainda não foi instanciado, cria-lo.
  IF o_tvarv IS NOT BOUND.
    CREATE OBJECT o_tvarv
      EXPORTING
        prefix    = c_prefixo
        separator = c_separador.
  ENDIF.

  CALL METHOD o_tvarv->get_parameter
    EXPORTING
      suffix = c_sessao
    IMPORTING
      value  = gv_sessao.

ENDFORM.
 ```

##### D - FORM F_TRATA_ARQUIVO
 
 ```abap
 FORM f_trata_arquivo.

  DATA: lw_arq      LIKE LINE OF it_arq,
        l_newko(10) TYPE c,
        l_zuonr     TYPE bseg-zuonr,
        lwa_arq     TYPE ty_arq.

  DATA: it_arq_sorted    TYPE TABLE OF ty_arq,
        lv_ischangezuonr TYPE c.


  CLEAR: lw_arq, l_newko, l_zuonr.

  APPEND LINES OF it_arq TO it_arq_sorted.
  SORT it_arq_sorted BY zuonr.

*BCI:RRUBINO: Melhoria carga ZRFI_PARTIDAS_ABERTAS - 11.02.2019
*  LOOP AT it_arq ASSIGNING FIELD-SYMBOL(<fs_arq>).
  CLEAR it_alv.

  LOOP AT it_arq_sorted ASSIGNING FIELD-SYMBOL(<fs_arq>).
    AT FIRST.
      l_zuonr    = <fs_arq>-zuonr.          "Atribuiçao  = xblnr referencia
      vg_th_cont = 1.
    ENDAT.

* SSI - 1000017494
*    IF  l_zuonr IS NOT INITIAL
*    AND l_zuonr <> <fs_arq>-zuonr.

    IF  l_zuonr <> <fs_arq>-zuonr.
*
      " Executar BAPI / COMMIT.
      vg_act_jobs = vg_snd_jobs - vg_rcv_jobs.
      WAIT UNTIL vg_act_jobs LT gv_sessao.

      IF lwa_arq IS INITIAL.
        lwa_arq = <fs_arq>.
      ENDIF.

      DO.

        CALL FUNCTION 'TH_ARFC_LOCAL_RESOURCES'
          IMPORTING
            nores = vg_free.

        CHECK vg_free GE 3.

        CONCATENATE c_task vg_th_cont INTO vg_task_id.

        CALL FUNCTION 'ZFFI_CARGA_PARTIDA_ABERTA'
          STARTING NEW TASK vg_task_id
          DESTINATION IN GROUP DEFAULT
          PERFORMING f_fim_bapi ON END OF TASK
          EXPORTING
            p_arq                 = lwa_arq
            doc_header            = wa_doc_header
            p_teste               = p_teste
          TABLES
            it_accountgl          = it_accountgl
            it_curr_amount        = it_curr_amount
            it_accountpayable     = it_accountpayable
            it_accountreceivable  = it_accountreceivable
            it_extension2         = it_extension2
          EXCEPTIONS
            communication_failure = 1
            system_failure        = 2
            resource_failure      = 3.

        IF sy-subrc EQ 3.
          WAIT UP TO 1 SECONDS.
        ELSE.
          EXIT.
        ENDIF.

      ENDDO.

      CLEAR: lwa_arq.

      vg_snd_jobs = vg_snd_jobs + 1.
      vg_th_cont  = vg_th_cont + 1.

      CLEAR:
        wa_doc_header,
        wa_accountgl,
        wa_accountpayable,
        wa_accountreceivable,
        wa_curr_amount,
        gv_cont,
        wa_extension2.

      REFRESH:
        it_accountgl,
        it_accountpayable,
        it_accountreceivable,
        it_curr_amount,
        it_extension2.

    ENDIF.

    lw_arq  = <fs_arq>.
    l_zuonr = <fs_arq>-zuonr.
    gv_cont = gv_cont + 1.

    " Format 'DOCUMENTHEADER'
    wa_doc_header-username   = sy-uname.     " User name
    wa_doc_header-header_txt = <fs_arq>-bktxt. " Document Header Text
    wa_doc_header-comp_code  = <fs_arq>-bukrs. " Company Code
    wa_doc_header-doc_date   = <fs_arq>-bldat. " Document Date in Document
    wa_doc_header-pstng_date = <fs_arq>-budat. " Posting Date in the Document
    wa_doc_header-doc_type   = <fs_arq>-blart. " Document Type
    wa_doc_header-ref_doc_no = <fs_arq>-xblnr. " Reference Document Number

    IF lwa_arq IS INITIAL.
      lwa_arq = <fs_arq>.
    ENDIF.

    " Exit de conversão para utilização da conta correta com apenas 10 caracteres.
    CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
      EXPORTING
        input  = <fs_arq>-newko
      IMPORTING
        output = l_newko.

    <fs_arq>-newko = l_newko.


***Se chave de lançamento(bschl).
    CASE <fs_arq>-bschl.

***Fornecedor
      WHEN c_ch_cred_fornec   "21
        OR c_ch_razao_fornec  "29
        OR c_ch_fat_fornec.   "31

        " Format 'ACCOUNTPAYABLE'
        wa_accountpayable-itemno_acc    = gv_cont.        " Invoice Number
        wa_accountpayable-vendor_no     = <fs_arq>-newko. " Vendor Number
        wa_accountpayable-comp_code     = <fs_arq>-bukrs. " Company Code
        wa_accountpayable-item_text     = <fs_arq>-sgtxt.
        wa_accountpayable-profit_ctr    = <fs_arq>-prctr.
        wa_accountpayable-alloc_nmbr    = <fs_arq>-zuonr.
        wa_accountpayable-bline_date    = <fs_arq>-zfbdt.
        wa_accountpayable-businessplace = <fs_arq>-bupla.
        wa_accountpayable-pymt_meth     = <fs_arq>-zlsch.
        wa_accountpayable-bank_id       = <fs_arq>-hbkid.
        wa_accountpayable-ref_key_3     = <fs_arq>-xref3.



* SSI - 1000017494
        wa_accountpayable-pmnttrms      = <fs_arq>-zterm. " Chave de condições de pagamento
*
        APPEND wa_accountpayable TO it_accountpayable.

        " Format 'CURR_AMOUNT'
        wa_curr_amount-itemno_acc   = gv_cont.        " Accounting Document Line Item Number
        wa_curr_amount-currency_iso = <fs_arq>-waers. " Currency
        wa_curr_amount-amt_doccur   = <fs_arq>-wrbtr. " Amount
        APPEND wa_curr_amount TO it_curr_amount.


***Saldo GL

      WHEN c_ch_lanc_cred   "50
        OR c_ch_lanc_deb.   "40

        "Adicionando Ordem e Centro de Lucro
        IF <fs_arq>-bschl = c_ch_lanc_deb.        " Se for igual a 40
          wa_accountgl-orderid = <fs_arq>-aufnr.  "??? aufnr???
        ENDIF.

        " Format 'ACCOUNTGL'
        wa_accountgl-itemno_acc   = gv_cont.        " Accounting Document Line Item Number
        wa_accountgl-gl_account   = <fs_arq>-newko. " Revenue GL Account                 "Conta contabil
        wa_accountgl-comp_code    = <fs_arq>-bukrs. " Company Code                       "Empresa
        wa_accountgl-pstng_date   = <fs_arq>-budat. " Posting Date in the Document       "Data de Lançamento
        wa_accountgl-item_text    = <fs_arq>-sgtxt.                   "Texto do item
        wa_accountgl-alloc_nmbr   = <fs_arq>-zuonr.                   "Atribuição = xblnr referencia
        wa_accountgl-costcenter   = <fs_arq>-kostl.                   "Centro de custo
        wa_accountgl-profit_ctr   = <fs_arq>-prctr.                   "Centro de lucro
        wa_accountgl-ref_key_3    = <fs_arq>-xref3.                   "Chav Referencia

* SSI - 1000017494
        wa_accountgl-plant        = <fs_arq>-werks.                   "Centro (CM01)

        CALL FUNCTION 'CONVERSION_EXIT_ALPHA_INPUT'
          EXPORTING
            input  = <fs_arq>-matnr
          IMPORTING
            output = <fs_arq>-matnr.

        wa_accountgl-material     = <fs_arq>-matnr.
        wa_accountgl-po_number    = <fs_arq>-ebeln.                 "Doc Material
*
        APPEND wa_accountgl TO it_accountgl.

        wa_extension2-structure  = 'EXTENSION2'.
        wa_extension2-valuepart1 = <fs_arq>-bupla.
        wa_extension2-valuepart2 = wa_accountgl-itemno_acc.
        APPEND wa_extension2 TO it_extension2.

        " Format 'CURR_AMOUNT'
        wa_curr_amount-itemno_acc   = gv_cont.        " Accounting Document Line Item Number
        wa_curr_amount-currency_iso = <fs_arq>-waers. " Currency
        wa_curr_amount-amt_doccur   = <fs_arq>-wrbtr. " Amount
        APPEND wa_curr_amount TO it_curr_amount.


***Clientes

      WHEN c_ch_fat_cli    "01
        OR c_ch_cred_cli   "11
        OR c_ch_razao_cli. "19

        " fORMAT 'accountreceivable'
        wa_accountreceivable-itemno_acc    = gv_cont.        " Accounting Document Line Item Number
        wa_accountreceivable-customer      = <fs_arq>-newko. " Customer Account
        wa_accountreceivable-comp_code     = <fs_arq>-bukrs. " Company Code
        wa_accountreceivable-item_text     = <fs_arq>-sgtxt.
        wa_accountreceivable-profit_ctr    = <fs_arq>-prctr.
        wa_accountreceivable-alloc_nmbr    = <fs_arq>-zuonr.
        wa_accountreceivable-bline_date    = <fs_arq>-zfbdt.
        wa_accountreceivable-businessplace = <fs_arq>-bupla.
        wa_accountreceivable-pymt_meth     = <fs_arq>-zlsch.
        wa_accountreceivable-bank_id       = <fs_arq>-hbkid.
        wa_accountreceivable-ref_key_3     = <fs_arq>-xref3.

* SSI - 1000017494
        wa_accountreceivable-pmnttrms      = <fs_arq>-zterm. " Chave de condições de pagamento
*

        APPEND wa_accountreceivable TO it_accountreceivable.

        " Format 'CURR_AMOUNT'
        wa_curr_amount-itemno_acc   = gv_cont.        " Accounting Document Line Item Number
        wa_curr_amount-currency_iso = <fs_arq>-waers. " Currency
        wa_curr_amount-amt_doccur   = <fs_arq>-wrbtr. " Amount
        APPEND wa_curr_amount TO it_curr_amount.

    ENDCASE.
*** Fim SE chave de lançamento(bschl).


    " Processar ultimo registro
    AT LAST.

      " Executar BAPI / COMMIT.
      vg_act_jobs = vg_snd_jobs - vg_rcv_jobs.
      WAIT UNTIL vg_act_jobs LT gv_sessao.

      DO.

        CALL FUNCTION 'TH_ARFC_LOCAL_RESOURCES'
          IMPORTING
            nores = vg_free.

        CHECK vg_free GE 3.

        CONCATENATE c_task vg_th_cont INTO vg_task_id.

        CALL FUNCTION 'ZFFI_CARGA_PARTIDA_ABERTA'
          STARTING NEW TASK vg_task_id
          DESTINATION IN GROUP DEFAULT
          PERFORMING f_fim_bapi ON END OF TASK
          EXPORTING
            p_arq                 = lwa_arq
            doc_header            = wa_doc_header
            p_teste               = p_teste
          TABLES
            it_accountgl          = it_accountgl
            it_curr_amount        = it_curr_amount
            it_accountpayable     = it_accountpayable
            it_accountreceivable  = it_accountreceivable
            it_extension2         = it_extension2
          EXCEPTIONS
            communication_failure = 1
            system_failure        = 2
            resource_failure      = 3.

        IF sy-subrc EQ 3.
          WAIT UP TO 1 SECONDS.
        ELSE.
          EXIT.
        ENDIF.
      ENDDO.

      CLEAR: lwa_arq.

      vg_snd_jobs = vg_snd_jobs + 1.
      vg_th_cont  = vg_th_cont + 1.

      CLEAR:
        wa_doc_header,
        wa_accountgl,
        wa_accountpayable,
        wa_accountreceivable,
        wa_curr_amount,
        gv_cont,
        wa_extension2.

      REFRESH:
        it_accountgl,
        it_accountpayable,
        it_accountreceivable,
        it_curr_amount,
        it_extension2.

    ENDAT.

  ENDLOOP.

ENDFORM.
 ```
 
 * **Funções chamadas:**<br>
    - TH_ARFC_LOCAL_RESOURCES - 183, 394
    
    [Código](https://github.com/danielasalomao/ajustecarga/blob/master/FUNCTION%20TH_ARFC_LOCAL_RESOURCES.txt)
    
 ```abap
 
 ```
    
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

```abap 
FORM f_formatar_dados_arquivo USING ip_string TYPE string.

  DATA: lw_arq      LIKE LINE OF it_arq,
        l_wrbtr(16) TYPE c,
        l_budat(10) TYPE c,
        l_bldat(10) TYPE c,
        l_zfbdt(10) TYPE c,
        lv_belnr(10) TYPE c,
        l_gjahr(4) TYPE c.


  CLEAR: lw_arq, l_wrbtr, l_budat, l_bldat, l_zfbdt, lv_belnr, l_gjahr.

  " Executar Loop apenas se a tabela de conteúdo estiver preenchida.
  SPLIT ip_string AT ';' INTO: l_budat       " Data de Lançamento
                               lw_arq-monat  " Período
                               lw_arq-bukrs  " Empresa
*
                               lw_arq-werks  " Centro
*
                               lw_arq-blart  " Tipo de Documento
                               l_bldat       " Data do documento
                               lw_arq-waers  " Moeda
                               lw_arq-kursf  " Taxa de conversão
                               lw_arq-bktxt  " Texto de cabeçalho
                               lw_arq-xblnr  " Referência
                               lw_arq-bschl  " Chave de Lançamento
                               lw_arq-newko  " Conta / Conta Contábil
                               lw_arq-newnum " Indicação de Razão Especial
                               l_wrbtr       " Montante na moeda do documento
                               l_zfbdt       " Data base
*
                               lw_arq-zterm  " Cond.Pafgamento
                               lw_arq-matnr  " Material
*
                               lw_arq-kostl  " Centro de Custo
                               lw_arq-prctr  " Centro de Lucro
                               lw_arq-aufnr  " Ordem Interna
                               lw_arq-zuonr  " Atribuição
                               lw_arq-sgtxt  " Texto do Item.
                               lw_arq-bupla  " Local de negócios
*
                               lw_arq-ebeln  " Doc.Material
*
                               lw_arq-xref3  " Chave de referência para item de doc.
                               lw_arq-anfbn  " Nº Solicitação
                               lw_arq-anfbu  " Empresa da Solicitação de L/C
                               lw_arq-anfbj  " Ano da Solicitação de L/C
                               lw_arq-hbkid  " Banco empresa
                               lw_arq-zlsch " Forma de pagamento.
                               lv_belnr " Nº documento.
                               l_gjahr. " Exercicio.

  " trata centro de lucro do arquivo
  PERFORM f_conversion_exit_alpha_input CHANGING lw_arq-prctr.

  " Data de Lançamento
  PERFORM f_conversion_exit_sdate_input USING    l_budat
                                        CHANGING lw_arq-budat.
  " Data do documento
  PERFORM f_conversion_exit_sdate_input USING    l_bldat
                                        CHANGING lw_arq-bldat.
  " Data base
  PERFORM f_conversion_exit_sdate_input USING    l_zfbdt
                                        CHANGING lw_arq-zfbdt.


*  PERFORM f_conversion_exit_sdate_input USING    l_gjahr
*                                       CHANGING lw_arq-budat.




  " Tratando variável de valor
  TRANSLATE l_wrbtr USING ',.'.

  IF lw_arq-bschl EQ c_ch_lanc_cred
  OR lw_arq-bschl EQ c_ch_fat_fornec
  OR lw_arq-bschl EQ c_ch_razao_cli
  OR lw_arq-bschl EQ c_ch_cred_cli.
    lw_arq-wrbtr = l_wrbtr * -1.
  ELSE.
    lw_arq-wrbtr = l_wrbtr.
  ENDIF.

  APPEND lw_arq TO it_arq.

ENDFORM.

```
 
##### N - FORM exibir_popup_erros
 
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
