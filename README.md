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

#### TABELAS DE DIMENSÃO

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
    

##### DSSI: Armazena informacoes relativas a SSI (chamado).<br>
* **ID_SSI:** Identificador da SSI.<br>
    - Tipo de dados: Número Decimal.
* **SSI:** Armazena o número da SSI aberta. <br>
    - Tipo de dados: Número Inteiro.
* **Descricao:** Descreve a SSI.<br>
    - Tipo de dados: Texto.

##### DPROCESSADOR: Armazena informacoes relativas ao processador (pessoa que atende a SSI).<br>
* **ID_Processador:** Identificador do processador.<br>
    - Tipo de dados: Número Decimal.
* **Nome:** Armazena nome do funcionário.<br>
    - Tipo de dados: Texto.
* **Equipe_suporte:** Armazena a equipe que o processador faz parte. Infra, Sistemas ou Suporte N1.<br>
    - Tipo de dados: Texto.

##### DRESPOSTA: Armazena informacoes referentes ao preenchimento da pesquisa de satisfacao.<br>
* **ID_SSI:** Identificador da SSI.<br>
    - Tipo de dados: Número Decimal.
* **SSI:** (Necessário??)<br>
    - Tipo de dados: Número Inteiro.
* **Data_Resposta_Pesquisa:** Armazena a data em que a pesquisa foi respondida.<br>
    - Tipo de dados: Qualquer.
* **Resolucao_Problema:** Seu problema foi resolvido neste atendimento? <br>
    - Tipo de dados: Texto.
* **Agilidade_Atendimento:** Quão ágil foi a resolução do seu atendimento?<br>
    - Tipo de dados: Texto.
* **Qualidade_Atendimento:** Quão bem atendido você foi?<br>
    - Tipo de dados: Texto.
* **Satisfacao_Atendimento:** De 1 a 5, quão satisfeito você está com o serviço prestado neste atendimento?<br>
    - Tipo de dados: Número Inteiro.
* **Comentários:** Compartilhe conosco o que achar importante.<br>
    - Tipo de dados: Texto.


#### DCALENDARIO:
* **Data:** DD/MM/AAAA<br>
    - Tipo de dados: Qualquer.
* **Ano:** AAAA<br>
    - Tipo de dados: Número Inteiro.
* **Dia:** DD<br>
    - Tipo de dados: Número Inteiro.
* **Mes:** MM<br>
    - Tipo de dados: Número Inteiro.
* **Mes/Ano:** MM/AAAA<br>
    - Tipo de dados: Texto.
    

### ZRFI_PARTIDAS_ABERTASF01_CPY

#### TELA DE SELEÇÃO

#### PERFORM:

FORM F_UPLOAD

FORM F_TVARV

FORM F_TRATA_ARQUIVO

FORM F_FIM_BAPI

FORM F_TRATA_ARQUIVO_VERIFICA_ERRO

FORM_F_CRIA_ALV

FORM F_SET_FIELDCAT

FORM F_SORT_TABLE

FORM F_LIMPA_VAR

FORM F_CONVERSION_EXIT_ALPHA_INPUT

FORM F_CONVERSION_EXIT_SDATE_INPUT

FORM F_FORMATAR_DADOS_ARQUIVO
    
    
