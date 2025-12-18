# prog-cadastro-de-produto-em-ABAP
programa de cadastro de produto em ABAP
REPORT ZWSILVA4_produto_cadastro.

INCLUDE ZWSILVA4_produto_top.
INCLUDE ZWSILVA4_produto_forms.
INCLUDE ZWSILVA4_produto_main.

📄 INCLUDE – Declarações (Z_PRODUTO_TOP)

"Parâmetros simulando tela de cadastro
PARAMETERS:
  p_cod   TYPE char10 OBLIGATORY,
  p_desc  TYPE char40 OBLIGATORY,
  p_preco TYPE p DECIMALS 2,
  p_ativo TYPE c AS CHECKBOX DEFAULT 'X'.

DATA: gv_status TYPE c LENGTH 1.

CONSTANTS:
  gc_status_success TYPE c VALUE 'S', "Sucesso
  gc_status_error   TYPE c VALUE 'E'. "Erro


INCLUDE – Regras e Validações (Z_PRODUTO_FORMS)

FORM validar_dados.

  IF p_preco <= 0.
    gv_status = 'E'.
    gv_msg = 'Preço inválido. Deve ser maior que zero.'.
    EXIT.
  ENDIF.

  IF p_desc IS INITIAL.
    gv_status = 'E'.
    gv_msg = 'Descrição do produto é obrigatória.'.
    EXIT.
  ENDIF.

  gv_status = 'S'.

ENDFORM.


FORM cadastrar_produto.

  PERFORM validar_dados.

  IF gv_status = 'E'.
    EXIT.
  ENDIF.

  "Simulação de gravação em tabela Z
  gv_msg = |Produto { p_cod } cadastrado com sucesso!|.

ENDFORM.

INCLUDE – Processamento Principal (Z_PRODUTO_MAIN)

START-OF-SELECTION.

  PERFORM cadastrar_produto.

  IF gv_status = 'E'.
    MESSAGE gv_msg TYPE 'E'.
  ELSE.
    WRITE: / 'Código....:', p_cod,
           / 'Descrição:', p_desc,
           / 'Preço....:', p_preco,
           / 'Ativo....:', p_ativo,
           / gv_msg.
  ENDIF.
