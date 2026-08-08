# 🧾 Anotações de ABAP

> ⚠️ Códigos de empresa, usuários e nomes específicos do sistema real foram trocados por exemplos genéricos.

**F8** = executa o programa.

---

## 📄 Estrutura de um relatório (REPORT)

```abap
REPORT zpr_exemplo.

TABLES t001w.

SELECT-OPTIONS: s_werks FOR t001w-werks.

START-OF-SELECTION.

WRITE: / 'Centro', 10 'Nome'.

SELECT werks, name1
  INTO TABLE @DATA(lt_centros)
  FROM t001w
  WHERE werks IN @s_werks.

LOOP AT lt_centros INTO DATA(ls_centro).
  WRITE: / ls_centro-werks, 10 ls_centro-name1.
ENDLOOP.
```
- `TABLES` declara a tabela base pra usar em `SELECT-OPTIONS`
- `SELECT-OPTIONS ... FOR campo` cria uma tela de seleção (filtro `IN`) pro usuário
- `START-OF-SELECTION` é o evento onde o processamento principal começa
- `WRITE: / 'texto', posição 'texto2'` — a `/` pula linha, o número é a coluna onde o texto começa

---

## 📦 Variáveis (DATA)

```abap
DATA: lv_numero TYPE i VALUE 10,          " inteiro
      lv_texto  TYPE c LENGTH 11,          " texto de tamanho fixo
      lv_string TYPE string,               " texto de tamanho variável
      lv_valor  TYPE p LENGTH 8 DECIMALS 2. " número com casas decimais (packed)
```
| Prefixo (convenção) | Significa |
|---|---|
| `lv_` | local variable (variável simples) |
| `lt_` / `it_` | internal table (tabela interna) |
| `ls_` / `wa_` | line/work area (uma linha de uma tabela) |
| `gv_` / `gt_` / `gs_` | versão global das anteriores |

---

## ✍️ Saída de tela (WRITE)

```abap
WRITE: / 'RELATÓRIO', 10 'Coluna 2'.
ULINE.            " linha horizontal
SKIP 1.           " pula 1 linha (SKIP 2 pula 2)
MESSAGE 'Feito com sucesso' TYPE 'S' DISPLAY LIKE 'S'.  " mensagem no rodapé
```
- Tipos de `MESSAGE`: `'S'` sucesso, `'E'` erro, `'W'` aviso, `'I'` informação
- `FORMAT COLOR COL_HEADING` / `FORMAT COLOR OFF` — colore a saída (útil pra "zebrar" linhas de relatório)

---

## 🔀 Condições

```abap
IF condição.
  " ...
ELSEIF outra_condição.
  " ...
ELSE.
  " ...
ENDIF.

CASE variavel.
  WHEN 1.
    WRITE: / 'É um'.
  WHEN 2.
    WRITE: / 'É dois'.
  WHEN OTHERS.
ENDCASE.
```

---

## 🔁 Loops

```abap
" Enquanto
WHILE cont <= 5.
  cont = cont + 1.
ENDWHILE.

" Repetir N vezes
DO 5 TIMES.
  WRITE: / 'Número:', sy-index.  " sy-index = número da repetição atual
ENDDO.

" Dentro de LOOP AT / DO:
CONTINUE.        " pula pra próxima repetição
CHECK condição.  " se falso, pula direto pra próxima repetição
EXIT.            " sai do loop completamente
```

---

## 📚 Tabelas internas (Internal Tables)

```abap
DATA: lt_clientes TYPE TABLE OF zcliente,
      ls_cliente  TYPE zcliente.

SELECT * FROM zcliente INTO TABLE lt_clientes.

LOOP AT lt_clientes INTO ls_cliente.
  WRITE: / ls_cliente-nome.
ENDLOOP.

APPEND ls_cliente TO lt_clientes.  " adiciona uma linha
```

### FIELD-SYMBOLS (ler/alterar direto na tabela, sem copiar)
```abap
FIELD-SYMBOLS: <fs_cliente> TYPE zcliente.

LOOP AT lt_clientes ASSIGNING <fs_cliente>.
  IF <fs_cliente>-nome = 'ANA'.
    <fs_cliente>-email = 'ana@email.com'.   " altera direto na tabela original
  ENDIF.
ENDLOOP.

MODIFY zcliente FROM TABLE lt_clientes.
IF sy-subrc IS INITIAL.
  COMMIT WORK.   " salva as alterações no banco
ENDIF.
```
> 💡 `sy-subrc = 0` (ou `IS INITIAL`) quase sempre significa "deu certo". `sy-index` é a repetição atual do loop, `sy-tabix` é a linha atual da tabela interna.

### TYPES personalizados (criar sua própria estrutura)
```abap
TYPES: BEGIN OF ty_cliente,
         nome  TYPE string,
         idade TYPE i,
       END OF ty_cliente.

DATA: lv_cliente TYPE ty_cliente.
lv_cliente = VALUE #( nome = 'Lucas' idade = 30 ).
```

---

## 🔎 SELECT (consultas no banco)

```abap
" Básico
SELECT * FROM t001 INTO TABLE @DATA(lt_t001) WHERE bukrs = '0001'.

" Só um campo específico / uma linha
SELECT SINGLE name1 ort01 FROM t001w INTO (lv_nome, lv_cidade) WHERE werks = '0101'.

" LIKE (contém / começa com)
SELECT * INTO TABLE lt FROM t001w WHERE name1 LIKE '%ARMAZEM%'.
SELECT * INTO TABLE lt FROM t001w WHERE name1 LIKE 'MATRIZ%'.

" BETWEEN (intervalo)
SELECT * FROM t001 INTO TABLE @lt WHERE bukrs BETWEEN '0001' AND '0010'.

" NOT IN (exclui valores)
SELECT * FROM t001 INTO TABLE @lt WHERE bukrs NOT IN ( '0001', '0002' ).

" IS NOT NULL
SELECT * FROM t001 INTO TABLE @lt WHERE adrnr IS NOT NULL.

" Agregações
SELECT COUNT(*) FROM t001w INTO lv_qtd WHERE name1 LIKE '%ARMAZEM%'.
SELECT SUM( campo ) FROM t001w INTO lv_soma.

" Ordenar
SELECT * INTO TABLE lt FROM t001w ORDER BY name1 DESCENDING.

" JOIN entre tabelas
SELECT t~bukrs, t~butxt, a~street
  INTO TABLE @DATA(lt_join)
  FROM t001 AS t
  LEFT JOIN adrc AS a ON a~addrnumber = t~adrnr
  WHERE t~bukrs = '0001'.

" CASE dentro do SELECT
SELECT bukrs, land1,
       CASE WHEN land1 = 'BR' THEN 'Brasil' ELSE 'Outro' END AS tipo_pais
  FROM t001
  INTO TABLE @lt.
```

### RANGES (usado com `IN`)
```abap
DATA: r_bukrs TYPE RANGE OF t001-bukrs,
      ls_bukrs LIKE LINE OF r_bukrs.

ls_bukrs-sign   = 'I'.   " I = inclui / E = exclui
ls_bukrs-option = 'EQ'.  " EQ, NE, LT, LE, GT, GE, BT (between), NP (not pattern)
ls_bukrs-low    = '0001'.
APPEND ls_bukrs TO r_bukrs.

SELECT * FROM t001 INTO TABLE @lt WHERE bukrs IN @r_bukrs.
```

---

## 🧵 Strings

```abap
DATA: texto TYPE string VALUE 'João;Maria;Carlos'.
SPLIT texto AT ';' INTO DATA(nome1) DATA(nome2) DATA(nome3). " quebra por caractere

DATA: frase TYPE string VALUE ' texto '.
SHIFT frase RIGHT DELETING TRAILING space.   " tira espaços à direita
CONDENSE frase.                              " remove espaços extras (inclusive no meio)

SEARCH texto FOR 'termo'.                    " procura um trecho
IF sy-subrc = 0.
  WRITE: / 'Encontrado na posição:', sy-fdpos + 1.
ENDIF.

REPLACE 'antigo' WITH 'novo' INTO texto.     " substitui um trecho

CONCATENATE nome sobrenome INTO nome_completo SEPARATED BY space. " junta strings

DATA(tamanho) = strlen( texto ).             " conta caracteres

MOVE nome1 TO nome2.                         " copia valor (equivale a nome2 = nome1)
```

---

## ⚠️ Tratamento de erro (TRY/CATCH)

```abap
TRY.
    lv_resultado = lv_num1 / lv_num2.
  CATCH cx_sy_arithmetic_error INTO DATA(erro).
    WRITE: / 'Erro: divisão por zero.'.
ENDTRY.
```

---

## 🔧 Sub-rotinas (PERFORM / FORM)

```abap
DATA: lv_numero TYPE i VALUE 10.

START-OF-SELECTION.
  WRITE: / 'Antes:', lv_numero.
  PERFORM incrementar CHANGING lv_numero.
  WRITE: / 'Depois:', lv_numero.

FORM incrementar CHANGING pv_valor TYPE i.
  pv_valor = pv_valor + 1.
ENDFORM.
```
- `USING` passa valor só de entrada; `CHANGING` conecta a variável de fora com a de dentro (o que muda lá dentro reflete fora)

---

## 📞 Chamando função (FUNCTION MODULE)

```abap
CALL FUNCTION 'NOME_DA_FUNCAO'
  EXPORTING
    p_entrada = lv_valor
  IMPORTING
    p_saida   = lv_resultado
  EXCEPTIONS
    OTHERS    = 1.

IF sy-subrc = 0.
  WRITE: / 'Resultado:', lv_resultado.
ENDIF.
```

## 🧩 Macro
```abap
DEFINE saudacao.
  WRITE: / 'Olá'.
END-OF-DEFINITION.

START-OF-SELECTION.
  saudacao.
```

---

## 🏛️ Classes e OOP

### Estrutura básica + construtor
```abap
CLASS lcl_pessoa DEFINITION.
  PUBLIC SECTION.
    METHODS: constructor,
             mostrar_dados.
  PRIVATE SECTION.
    DATA: nome TYPE string.
ENDCLASS.

CLASS lcl_pessoa IMPLEMENTATION.
  METHOD constructor.
    nome = 'Exemplo'.
  ENDMETHOD.

  METHOD mostrar_dados.
    WRITE: / 'Nome:', nome.
  ENDMETHOD.
ENDCLASS.

START-OF-SELECTION.
  DATA lo_pessoa TYPE REF TO lcl_pessoa.
  CREATE OBJECT lo_pessoa.   " roda o constructor automaticamente
  lo_pessoa->mostrar_dados( ).
```
- `PUBLIC SECTION`: acessível de fora | `PRIVATE SECTION`: só dentro da própria classe
- `constructor` roda automaticamente ao criar o objeto com `CREATE OBJECT`

### Encapsulamento (métodos públicos controlando dados privados)
```abap
CLASS lcl_conta DEFINITION.
  PUBLIC SECTION.
    METHODS: depositar_public IMPORTING valor TYPE i,
             ver_saldo_public.
  PRIVATE SECTION.
    DATA saldo TYPE i.
    METHODS: depositar IMPORTING valor TYPE i,
             ver_saldo.
ENDCLASS.

CLASS lcl_conta IMPLEMENTATION.
  METHOD depositar.
    saldo = saldo + valor.
  ENDMETHOD.
  METHOD ver_saldo.
    WRITE: / 'Saldo:', saldo.
  ENDMETHOD.
  METHOD depositar_public.
    depositar( valor ).
  ENDMETHOD.
  METHOD ver_saldo_public.
    ver_saldo( ).
  ENDMETHOD.
ENDCLASS.
```

### Herança e Polimorfismo
```abap
CLASS lcl_animal DEFINITION.
  PUBLIC SECTION.
    METHODS falar.
ENDCLASS.
CLASS lcl_animal IMPLEMENTATION.
  METHOD falar.
    WRITE: / 'Animal fazendo som...'.
  ENDMETHOD.
ENDCLASS.

CLASS lcl_gato DEFINITION INHERITING FROM lcl_animal.
  PUBLIC SECTION.
    METHODS falar REDEFINITION.  " sobrescreve o método do pai
ENDCLASS.
CLASS lcl_gato IMPLEMENTATION.
  METHOD falar.
    WRITE: / 'Miau!'.
  ENDMETHOD.
ENDCLASS.

START-OF-SELECTION.
  DATA: lo_animal TYPE REF TO lcl_animal,
        lo_gato   TYPE REF TO lcl_gato.
  CREATE OBJECT lo_gato.
  lo_animal = lo_gato.       " polimorfismo: variável do tipo pai aponta pro objeto filho
  lo_animal->falar( ).       " executa o método do GATO (Miau!), não do animal genérico
```

### Interface
```abap
INTERFACE lif_acao.
  METHODS executar.
ENDINTERFACE.

CLASS lcl_comando DEFINITION.
  PUBLIC SECTION.
    INTERFACES lif_acao.
ENDCLASS.
CLASS lcl_comando IMPLEMENTATION.
  METHOD lif_acao~executar.
    WRITE: / 'Comando executado!'.
  ENDMETHOD.
ENDCLASS.

START-OF-SELECTION.
  DATA lo_comando TYPE REF TO lcl_comando.
  CREATE OBJECT lo_comando.
  lo_comando->lif_acao~executar( ).
```

### Eventos
```abap
CLASS lcl_botao DEFINITION.
  PUBLIC SECTION.
    EVENTS clicked.
    METHODS pressionar.
ENDCLASS.
CLASS lcl_botao IMPLEMENTATION.
  METHOD pressionar.
    RAISE EVENT clicked.
  ENDMETHOD.
ENDCLASS.

CLASS lcl_handler DEFINITION.
  PUBLIC SECTION.
    METHODS on_click FOR EVENT clicked OF lcl_botao.
ENDCLASS.
CLASS lcl_handler IMPLEMENTATION.
  METHOD on_click.
    WRITE 'Botão clicado!'.
  ENDMETHOD.
ENDCLASS.

START-OF-SELECTION.
  DATA: lo_botao TYPE REF TO lcl_botao,
        lo_handler TYPE REF TO lcl_handler.
  CREATE OBJECT lo_botao.
  CREATE OBJECT lo_handler.
  SET HANDLER lo_handler->on_click FOR lo_botao.
  lo_botao->pressionar( ).   " dispara o evento, que chama on_click automaticamente
```

### Atributos e métodos estáticos (CLASS-DATA / CLASS-METHODS)
```abap
CLASS lcl_contador DEFINITION.
  PUBLIC SECTION.
    CLASS-DATA: total TYPE i VALUE 0.
    CLASS-METHODS: incrementar.
ENDCLASS.
CLASS lcl_contador IMPLEMENTATION.
  METHOD incrementar.
    total = total + 1.
    WRITE: / 'Total:', total.
  ENDMETHOD.
ENDCLASS.

START-OF-SELECTION.
  lcl_contador=>incrementar( ).  " não precisa criar objeto — chama direto pela classe
  lcl_contador=>incrementar( ).
```
> 💡 Membros estáticos (`CLASS-DATA`/`CLASS-METHODS`) são compartilhados entre todas as instâncias — não pertencem a um objeto específico, e sim à classe.

---

## 📊 ALV (relatório em grid)

### Forma simples (moderna) — `cl_salv_table`
```abap
DATA: it_dados TYPE STANDARD TABLE OF ty_linha,
      o_alv    TYPE REF TO cl_salv_table.

TRY.
    cl_salv_table=>factory(
      IMPORTING r_salv_table = o_alv
      CHANGING  t_table      = it_dados ).

    o_alv->get_functions( )->set_all( abap_true ).
    o_alv->get_display_settings( )->set_striped_pattern( abap_true ). " zebrado
    o_alv->get_columns( )->set_optimize( abap_true ).  " ajusta largura automática
    o_alv->display( ).

  CATCH cx_salv_msg INTO DATA(lx_msg).
    MESSAGE lx_msg->get_text( ) TYPE 'E'.
ENDTRY.
```

### Forma clássica — `REUSE_ALV_GRID_DISPLAY_LVC` / `REUSE_ALV_GRID_DISPLAY`
Mais verbosa: monta manualmente o **fieldcat** (catálogo de campos/colunas) e passa a tabela pra função exibir. Útil quando se precisa de mais controle (hotspot em coluna, cores condicionais, eventos de clique como `&IC1`, PF-STATUS customizado etc). Estrutura básica:
```abap
CALL FUNCTION 'REUSE_ALV_GRID_DISPLAY_LVC'
  EXPORTING
    i_callback_program = sy-repid
    it_fieldcat_lvc     = gt_fieldcat
  TABLES
    t_outtab             = it_dados
  EXCEPTIONS
    program_error         = 1
    OTHERS                = 2.
```

---

## 📌 Atalhos úteis
- `Ctrl + ,` / `Ctrl + .` — comentar/descomentar linha
- `Ctrl + N` — nova linha
- `Ctrl + Espaço` — autocomplete
