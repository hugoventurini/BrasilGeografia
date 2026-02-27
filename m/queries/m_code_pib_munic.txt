let
  // Conexão ao endpoint da tabela 5938 do sistema SIDRA IBGE, que contém dados relativos ao PIB dos municípios.
  Fonte = Json.Document(Web.Contents("https://apisidra.ibge.gov.br/values/t/5938/n6/all/v/37/p/last%201/d/v37%200")),
  // Conversão da lista de registros resulta da chamada à API em tabela.
  Tabela = Table.FromList(Fonte, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
  // Expansão do bloco bruto de dados, para obtenção do formato tabular.
  Expandido = Table.ExpandRecordColumn(Tabela, "Column1", {"NC", "NN", "MC", "MN", "V", "D1C", "D1N", "D2C", "D2N", "D3C", "D3N"}, {"NC", "NN", "MC", "MN", "V", "D1C", "D1N", "D2C", "D2N", "D3C", "D3N"}),
  // Promoção dos cabeçalhos amigáveis
  Headers = Table.PromoteHeaders(Expandido, [PromoteAllScalars = true]),
  // Tipificação das colunas de acordo com a natureza e finalidade de uso de cada dado.
  Tipos = Table.TransformColumnTypes(Headers, {{"Valor", type number}, {"Nível Territorial (Código)", type text}, {"Nível Territorial", type text}, {"Unidade de Medida (Código)", type text}, {"Unidade de Medida", type text}, {"Município (Código)", type text}, {"Município", type text}, {"Variável (Código)", type text}, {"Variável", type text}, {"Ano (Código)", type text}, {"Ano", type text}}),
  // Eliminação de espaços excedentes e desnecessários dos campos tipificados como texto.
  Corte = Table.TransformColumns(Tipos, {{"Nível Territorial (Código)", each Text.Trim(_), type nullable text}, {"Nível Territorial", each Text.Trim(_), type nullable text}, {"Unidade de Medida (Código)", each Text.Trim(_), type nullable text}, {"Unidade de Medida", each Text.Trim(_), type nullable text}, {"Município (Código)", each Text.Trim(_), type nullable text}, {"Município", each Text.Trim(_), type nullable text}, {"Variável (Código)", each Text.Trim(_), type nullable text}, {"Variável", each Text.Trim(_), type nullable text}, {"Ano (Código)", each Text.Trim(_), type nullable text}, {"Ano", each Text.Trim(_), type nullable text}}),
    // Ajusta o valor mensal do PIB para a unidade comum, em Reais
    Ajuste = Table.AddColumn(Corte, "Valor Ajustado", each [Valor] * 1000, type number)
in
    Ajuste
