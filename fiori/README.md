📘 SAP Fiori — Anotações CDS & Metadata Extension
Documentação pessoal sobre as principais anotações usadas em CDS Views e Metadata Extensions no desenvolvimento de aplicações SAP Fiori (List Report / Analytical).
Todos os exemplos fazem parte de uma mesma view fictícia de Movimentações de Estoque (ZI_MovimentacaoEstoque).

📘 Cabeçalho da View
1. @AccessControl.authorizationCheck
Controla a autorização da CDS View.
Valor
Descrição
#NOT_REQUIRED
Ignora autorização — usar apenas em DEV/testes
#CHECK
Aplica roles do PFCG
#PRIVILEGED_ONLY
Só funciona com acesso privilegiado

Quando usar:
App normal → #CHECK
Ambiente de teste → #NOT_REQUIRED

2. @AbapCatalog.viewEnhancementCategory
Define se a view pode ser estendida via enhancement clássico.
Valor
Descrição
#NONE
Não pode ser estendida
#PROJECTION_LIST
Permite adicionar campos
#GROUP_BY
Permite alterações com agregação
#UNION
Permite union extend

Quando usar: Na maioria dos casos → #NONE
@AbapCatalog.viewEnhancementCategory: [#NONE]


3. @Metadata.allowExtensions
Permite ou bloqueia a extensão via Metadata Extension.
Valor
Descrição
true
Permite Metadata Extension
false
Bloqueia extensão

Quando usar: Quase sempre true
@Metadata.allowExtensions: true


4. @Metadata.ignorePropagatedAnnotations
Controla se a view herda anotações de metadata de views associadas.
Valor
Descrição
true
Ignora metadata herdada
false
Herda tudo das views subjacentes

Quando usar: Para views analíticas (#CUBE) → sempre true (evita bug de conflito)
@Metadata.ignorePropagatedAnnotations: true


5. @OData.publish
Expõe a view como serviço OData automaticamente.
Valor
Descrição
true
Cria e expõe o serviço OData automaticamente
false
Não expõe

@OData.publish: true

Obs: Para apps mais complexos, é recomendado criar manualmente a Service Definition e o Service Binding.

6. @Analytics.dataCategory
Define o tipo analítico da view.
Valor
Descrição
#CUBE
Modelo analítico (gráficos, somas)
#DIMENSION
Dados mestre — sem soma
#FACT
Tabela de fatos — base do Cube
#AGGREGATIONLEVEL
Nível agregado

@Analytics.dataCategory: #CUBE


7. @ObjectModel.usageType
Define qualidade de serviço, tamanho e classe de dados para otimização de performance.
@ObjectModel.usageType: {
    serviceQuality: #X,
    sizeCategory:   #S,
    dataClass:      #MIXED
}

serviceQuality
Valor
Descrição
#A
Alta qualidade, sem filtro obrigatório
#B
Boa qualidade
#C
Qualidade média
#D
Requer filtro obrigatório
#X
Sem restrição definida

sizeCategory
Valor
Descrição
#S
Pequeno
#M
Médio
#L
Grande
#XL
Muito grande

dataClass
Valor
Descrição
Exemplos
#MASTER
Dados estáveis, pouca mudança
Material, Empresa, Centro
#TRANSACTIONAL
Dados que mudam frequentemente
Movimentação de estoque, Pedido, Nota fiscal
#MIXED
Combinação de dados mestre + transacional, cálculos, joins e agregações
CUBE de estoque


8. @EndUserText.label — Cabeçalho
Define a descrição da view exibida na interface.
@EndUserText.label: 'Movimentações de Estoque'


📘 Corpo da View
9. @Aggregation.default
Define como o campo será agregado em contextos analíticos.
Valor
Descrição
Quando usar
#SUM
Soma
Valores e quantidades numéricas
#MIN
Menor valor
Mínimos
#MAX
Maior valor
Máximos
#AVG
Média
Médias
#COUNT
Contagem de linhas
Contar registros
#COUNT_DISTINCT
Contagem única
Contar sem repetição
#NONE
Sem agregação
Campos de texto

-- Campo numérico (valor monetário)
@Aggregation.default: #SUM
cast( _Itens.dmbtr as abap.dec(23,2) ) as ValorTotal,

-- Campo de texto (não agrega)
@Aggregation.default: #NONE
_Itens.matnr as Material,


10. @Semantics
Define o significado semântico do campo para o framework Fiori.
🔹 Quantidade
Vincula um campo numérico à sua unidade de medida. A unidade precisa ser um campo separado do tipo abap.unit.
-- Campo de unidade de medida (criado para evitar conflito ao somar unidades diferentes)
@Aggregation.default: #NONE
cast( '' as abap.unit(3) ) as UnidadePadrao,

-- Campo de quantidade vinculado à unidade
@Semantics.quantity.unitOfMeasure: 'UnidadePadrao'
@Aggregation.default: #SUM
cast( _Itens.menge as abap.dec(23,3) ) as Quantidade,

Por que criar um campo de unidade separado? Ao somar quantidades com unidades diferentes (ex: KG e UN), o SAP geraria conflito. Criar um campo UnidadePadrao com valor fixo ('') contorna esse problema.
🔹 Moeda
@Semantics.amount.currencyCode: 'Moeda'
@Aggregation.default: #SUM
cast( _Itens.dmbtr as abap.dec(23,2) ) as ValorTotal,

@Semantics.currencyCode: true
@Aggregation.default: #NONE
_Itens.waers as Moeda,

🔹 Texto
@Semantics.text: true
_Material.maktx as DescricaoMaterial,

Tipos semânticos mais comuns:
Anotação
Uso
@Semantics.quantity.unitOfMeasure
Vincula quantidade à unidade
@Semantics.amount.currencyCode
Vincula valor à moeda
@Semantics.currencyCode: true
Marca campo como moeda
@Semantics.unitOfMeasure: true
Marca campo como unidade
@Semantics.text: true
Marca campo como descrição/texto


11. CAST
Converte o tipo de um campo para o tipo correto esperado pela view.
Quando usar:
Forçar o tipo correto quando o original não serve
Padronizar tipos para joins ou cálculos
Tipos mais usados:
Tipo
Descrição
abap.dec(x,y)
Decimal — mais usado para valores e quantidades
abap.int1/2/4/8
Inteiros de diferentes tamanhos
abap.char(n)
Texto de tamanho fixo
abap.string
Texto de tamanho variável
abap.numc(n)
Número formatado com zeros à esquerda
abap.cuky
Código de moeda (ex: BRL, USD)
abap.unit(n)
Unidade de medida (ex: KG, UN)

-- Extrair apenas o ano de uma data (budat é do tipo dats, 8 caracteres: YYYYMMDD)
cast( substring( cast( _Header.budat as abap.char(8) ), 1, 4 )
    as abap.char(4) ) as AnoCadastro,

-- Converter valor para decimal com 2 casas
@Aggregation.default: #SUM
cast( _Itens.dmbtr as abap.dec(23,2) ) as ValorTotal,


12. CASE
Equivalente ao IF/ELSE dentro do SELECT da CDS. Usado para criar lógica condicional diretamente na view.
Quando usar:
Converter códigos em textos legíveis
Criar campos calculados
Separar valores por condição (muito comum em #CUBE)
-- CASE simples: converter código em descrição
case _Itens.shkzg
    when 'S' then 'Entrada'
    else          'Saída'
end as DirecaoMovimentacao,

-- CASE com múltiplas condições
case _Itens.bwart
    when '101' then 'Entrada por GR'
    when '102' then 'Estorno de Entrada'
    when '201' then 'Saída para Ordem'
    when '261' then 'Saída por Consumo'
    else            'Outro Movimento'
end as TipoMovimento,


13. define view entity
Define o tipo da view CDS.
Tipo
Descrição
define view
Sintaxe antiga — legado
define view entity
Sintaxe atual — recomendado
define root view entity
Usado em apps transacionais (CRUD)



14. Associações
Criam relacionamentos entre views CDS, similares a JOINs sob demanda (lazy loading).
association to CDS as _Alias on <condição>

Cardinalidades:
Tipo
Descrição
[0..1]
Zero ou um registro correspondente (opcional)
[1..1]
Exatamente um registro correspondente (obrigatório)
[0..*]
Zero ou muitos registros correspondentes
[1..*]
Um ou muitos registros correspondentes (obrigatório)


association [0..1] to …  //Cada registro na visão de origem corresponde a, no máximo, um registro na visão de destino, podendo não haver correspondência.
association [1..1] to … //Cada registro na origem corresponde exatamente a um registro no destino.
association [0..*] to … //Cada registro na origem pode ter zero ou vários registros correspondentes no destino. O asterisco (*) representa "muitos" (n).
association [1..*] to … //Cada registro na origem deve ter obrigatoriamente pelo menos um, ou múltiplos registros no destino.


    -- Associação opcional com dados mestre de material
    association [0..1] to ZI_Material  as _Material  on $projection.Material  = _Material.Matnr
    -- Associação opcional com dados mestre de cliente
    association [0..1] to ZI_Cliente   as _Cliente   on $projection.ClienteId = _Cliente.IdCliente
    -- Associação para tabela SAP padrão de materiais
    association [0..1] to mara         as _Mara      on $projection.Material  = _Mara.matnr


📘 Metadata Extension — Cabeçalho e Gráficos
15. @Metadata.layer
Define em qual camada do sistema a Metadata Extension pertence.
Valor
Descrição
#CORE
Camada principal/base do sistema
#CUSTOMER
Customizações do cliente
#PARTNER
Desenvolvimentos de parceiros
#INDUSTRY
Soluções específicas de indústria
#LOCALIZATION
Localizações por país

@Metadata.layer: #CORE


16. @UI.headerInfo
Define as informações exibidas no cabeçalho da aplicação Fiori (tela de detalhes).
@UI.headerInfo: {
    typeName:       'Movimentação',
    typeNamePlural: 'Movimentações',
    title: { value: 'DocumentoMaterial' }  // Campo exibido no topo da tela de detalhes
}


17. @UI.chart
Cria gráficos automáticos na aplicação Fiori.
@UI.chart: [
  {
    qualifier: 'GraficoMovimentacoesMensais',
    chartType: #COLUMN,
    title: 'Movimentações por Mês',
    dimensions: [
        'AnoCadastro',
        'MesCadastro'
    ],
    measures: [
        'Quantidade'
    ],
    dimensionAttributes: [
      {
        dimension: 'AnoCadastro',
        role: #CATEGORY   // Eixo X — categoria principal
      },
      {
        dimension: 'MesCadastro',
        role: #SERIES     // Separa os dados em séries/grupos
      }
    ],
    measureAttributes: [
      {
        measure: 'Quantidade',
        role: #AXIS_1     // Eixo Y — valor numérico
      }
    ]
  }
]

Tipos de chartType:
Valor
Descrição
#COLUMN
Colunas verticais
#BAR
Barras horizontais
#LINE
Linha
#PIE
Pizza
#DONUT
Rosca
#AREA
Área preenchida
#SCATTER
Dispersão (pontos)
#BULLET
KPI/meta
#COMBINATION
Combinado (linha + coluna)


18. @UI.selectionPresentationVariant
Combina @UI.selectionVariant (filtros) e @UI.presentationVariant (layout) em um único bloco. Muito usado para criar abas ou visões completas em apps Fiori.
@UI.selectionPresentationVariant: [{
    qualifier:                    'MovimentacoesMensais',
    presentationVariantQualifier: 'MovimentacoesMensais',  // Liga com @UI.presentationVariant
    selectionVariantQualifier:    'MovimentacoesMensais'   // Liga com @UI.selectionVariant
}]


19. @UI.presentationVariant
Define como os dados serão organizados e quais elementos visuais (tabelas, gráficos, KPIs) serão exibidos na tela.
@UI.presentationVariant: [
  {
    qualifier: 'MovimentacoesMensais',
    sortOrder: [
      {
        by:        'AnoCadastro',
        direction: #DESC          // Ordena do mais recente para o mais antigo
      }
    ],
    visualizations: [
      // KPIs fixos no topo da tela
      { type: #AS_DATAPOINT, qualifier: 'KpiTotalMovimentacoes' },
      { type: #AS_DATAPOINT, qualifier: 'KpiQuantidadeEntrada'  },
      { type: #AS_DATAPOINT, qualifier: 'KpiQuantidadeSaida'    },
      { type: #AS_DATAPOINT, qualifier: 'KpiValorTotal'         },

      // Gráficos
      { type: #AS_CHART, qualifier: 'GraficoMovimentacoesMensais' },
      { type: #AS_CHART, qualifier: 'GraficoDirecaoMovimento'     },
      { type: #AS_CHART, qualifier: 'GraficoTopMateriais'         },

      // Tabela de detalhes
      { type: #AS_LINEITEM }
    ]
  }
]


20. @UI.selectionVariant
Aplica filtros pré-definidos no banco de dados antes de exibir os dados na tela.
@UI.selectionVariant: [
  {
    qualifier: 'FiltroEntradas',
    text:      'Entradas de Estoque',
    filter:    'DirecaoMovimentacao = ''Entrada'''
  },
  {
    qualifier: 'FiltroAnoAtual',
    text:      'Movimentações do Ano Corrente',
    selectOptions: [
      {
        element: 'AnoCadastro',
        sign:    #I,     // I = Include / E = Exclude
        option:  #EQ,    // EQ = Igual, BT = Entre, CP = Contém
        low:     '2026'
      }
    ]
  }
]

Resumo das três anotações:
@UI.selectionVariant → controla quais dados entram na tela (filtros)
@UI.presentationVariant → controla como os dados aparecem (ordenação e layout)
@UI.selectionPresentationVariant → combina os dois para criar uma visão completa ou uma aba funcional

📘 Metadata Extension — Corpo
21. @UI.facet
Define as seções da tela de detalhes (Object Page). Cada facet é um bloco/aba visível na tela.
@UI.facet: [
  {
    id:              'DadosBasicos',
    type:            #FIELDGROUP_REFERENCE,
    label:           'Dados Básicos',
    targetQualifier: 'DadosBasicos',  // Aponta para um @UI.fieldGroup
    position:        10
  },
  {
    id:              'DetalhesMovimento',
    type:            #FIELDGROUP_REFERENCE,
    label:           'Detalhes do Movimento',
    targetQualifier: 'DetalhesMovimento',
    position:        20
  }
]

Tipos de type:
Valor
Descrição
#FIELDGROUP_REFERENCE
Grupo de campos (@UI.fieldGroup)
#IDENTIFICATION_REFERENCE
Referência à seção de identificação
#LINEITEM_REFERENCE
Referência a uma tabela (@UI.lineItem)
#CHART_REFERENCE
Referência a um gráfico (@UI.chart)
#DATAPOINT_REFERENCE
Referência a um KPI (@UI.dataPoint)


22. @UI.lineItem
Exibe o campo como coluna na tabela da List Report.
@UI.lineItem: [{ position: 10 }]
DocumentoMaterial;

@UI.lineItem: [{ position: 20 }]
Material;

@UI.lineItem: [{ position: 30 }]
DirecaoMovimentacao;

@UI.lineItem: [{ position: 40 }]
Quantidade;

@UI.lineItem: [{ position: 50 }]
ValorTotal;


23. @UI.selectionField
Exibe o campo como filtro na barra de filtros da List Report.
@UI.selectionField: [{ position: 10 }]
Material;

@UI.selectionField: [{ position: 20 }]
DirecaoMovimentacao;

@UI.selectionField: [{ position: 30 }]
AnoCadastro;


24. @UI.fieldGroup
Agrupa campos para exibição nas seções da tela de detalhes (@UI.facet).
@UI.fieldGroup: [{ qualifier: 'DadosBasicos', position: 10 }]
DocumentoMaterial;

@UI.fieldGroup: [{ qualifier: 'DadosBasicos', position: 20 }]
Material;

@UI.fieldGroup: [{ qualifier: 'DetalhesMovimento', position: 10 }]
DirecaoMovimentacao;

@UI.fieldGroup: [{ qualifier: 'DetalhesMovimento', position: 20 }]
TipoMovimento;


25. @UI.dataPoint
Define um KPI (indicador-chave) exibido no topo da tela.
@UI.dataPoint: { qualifier: 'KpiTotalMovimentacoes', title: 'Total de Movimentações' }
DocumentoMaterial;

@UI.dataPoint: { qualifier: 'KpiValorTotal', title: 'Valor Total' }
ValorTotal;

@UI.dataPoint: { qualifier: 'KpiQuantidadeEntrada', title: 'Qtd. Entrada' }
Quantidade;


26. @Consumption.valueHelpDefinition
Define o Value Help (F4) de um campo — abre uma janela de busca ao pressionar F4 ou clicar no campo.
@Consumption.valueHelpDefinition: [{
    entity: {
        name:    'ZI_Material_VH',   // CDS View do Value Help
        element: 'Matnr'             // Campo chave da CDS de Value Help
    }
}]
@UI.selectionField: [{ position: 10 }]
Material;

@Consumption.valueHelpDefinition: [{
    entity: {
        name:    'ZI_DirecaoMovimento_VH',
        element: 'DirecaoMovimento'
    }
}]
@UI.selectionField: [{ position: 20 }]
DirecaoMovimentacao;


27. @EndUserText.label — Campo
Define o label (rótulo) exibido para o campo na interface Fiori.
@EndUserText.label: 'Documento de Material'
DocumentoMaterial;

@EndUserText.label: 'Direção da Movimentação'
DirecaoMovimentacao;

@EndUserText.label: 'Valor Total (R$)'
ValorTotal;


28. @UI.hidden
Oculta o campo da interface, mantendo-o disponível internamente (útil para chaves técnicas e campos de unidade/moeda).
@UI.hidden: true
UnidadePadrao;

@UI.hidden: true
Moeda;


📘 Passo a Passo — Criação de App Fiori
1. Criar a CDS principal (Eclipse)
Define tabelas, campos, chaves, relacionamentos e anotações de comportamento/labels.
2. Criar Metadata Extension (Eclipse) (opcional, mas recomendado)
Separa a apresentação visual da lógica de negócio.
3. Criar Service Definition (Eclipse)
Define quais CDS Views estarão disponíveis no serviço OData.
4. Criar Service Binding (Eclipse)
Expõe a CDS View como serviço OData.
5. Ativar o serviço OData — /n/IWFND/MAINT_SERVICE
Conecta o front-end (Fiori/UI5) ao back-end (ABAP/SAP).
Não esquecer de transportar o ⚠️ALIAS⚠️: Acessar o serviço → Alias → Customizing → fazer uma alteração qualquer → atribuir transporte → salvar → desfazer a alteração.
6. Criar List Report Page (VS Code)
Scaffold automático da aplicação usando o SAP Fiori Tools.
7. Deploy da aplicação (Terminal)
npm run deploy

8. Configurar Launchpad — /n/UI2/FLPD_CUST
Criar Catálogo (ZTC_...) e Grupo de Catálogo (ZTG_...)
Dentro do ZTC: criar bloco e Target Mapping
No Target Mapping do ZTC: clicar em "Criar referência" e referenciar ao ZTG
Criar Grupo (ZBG_...)
Adicionar o catálogo ZTG ao grupo
Bloco:

Targ. maping:

9. Criar/Ajustar a Role — /nPFCG
Criar ou abrir uma função existente
Aplicações → Adicionar catálogo e grupo
Usuário → Adicionar usuário
Autorizações → Exibir dados de autorização → Gerar
Transportar a função
10. Testar o app — /n/UI2/FLP
11. Correção de bug de label (SE11)
Campos sem formatação de label na tabela devem ter um Elemento de Dados criado na SE11 e associado via CAST na CDS:
cast( Empresa as zde_label_nome_empresas ) as Empresa

12. Tradução — SE63
Abrir a request e dar duplo clique (não na request principal)
Pegar o ID e o Tipo da CDS Metadata Extension

Dar duplo clique nas DDLXZC

Copiar, colar abaixo e clicar no botão verde

Salvar
13. Ativar serviços no SICF
default_host → sap → bc → ui5_ui5 → sap → <nome-do-app-vscode>
default_host → sap → opu → odata → sap → <nome-do-serviço-odata>


Documentação em constante atualização conforme novos desenvolvimentos.

