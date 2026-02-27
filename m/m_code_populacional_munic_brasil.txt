let
  // Conexão com a tabela 4714 da base SIDRA-IBGE que contém dados do CENSO 2022.
  Fonte = Json.Document(Web.Contents("https://apisidra.ibge.gov.br/values/t/4714/n1/all/n6/all/v/93/p/all")),
  // Conversão do conjunto de registros retornados pela chamada da API em tabela.
  Tabela = Table.FromList(Fonte, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
  // Expansão dos registros para obtenção do volume bruto de dados.
  Expandido = Table.ExpandRecordColumn(Tabela, "Column1", {"NC", "NN", "MC", "MN", "V", "D1C", "D1N", "D2C", "D2N", "D3C", "D3N"}, {"NC", "NN", "MC", "MN", "V", "D1C", "D1N", "D2C", "D2N", "D3C", "D3N"}),
  // Promoção dos cabeçalhos para melhor compreensão dos campos da tabela.
  Headers = Table.PromoteHeaders(Expandido, [PromoteAllScalars=true]),
  // Eliminação da linha em desacordo com o padrão desejado.
  LinhasFiltradas = Table.SelectRows(Headers, each ([#"Nível Territorial (Código)"] = "6")),
    // Tipificação dos dados de acordo com sua netureza e finalidade.
    TipoAlterado = Table.TransformColumnTypes(LinhasFiltradas,{{"Valor", Int64.Type}}),
    // Eliminação de espaços excedentes e desnecessários dos campos tipificados como texto.
    TextoAparado = Table.TransformColumns(TipoAlterado,{{"Nível Territorial (Código)", Text.Trim, type text}, {"Nível Territorial", Text.Trim, type text}, {"Unidade de Medida (Código)", Text.Trim, type text}, {"Unidade de Medida", Text.Trim, type text}, {"Brasil e Município (Código)", Text.Trim, type text}, {"Brasil e Município", Text.Trim, type text}, {"Variável (Código)", Text.Trim, type text}, {"Variável", Text.Trim, type text}, {"Ano (Código)", Text.Trim, type text}, {"Ano", Text.Trim, type text}}),
    // Eliminação de caracteres não imprimíveis, quebras de linhas e tabulações dos campos tipificados como texto.
    TextoLimpo = Table.TransformColumns(TextoAparado,{{"Nível Territorial (Código)", Text.Clean, type text}, {"Nível Territorial", Text.Clean, type text}, {"Unidade de Medida (Código)", Text.Clean, type text}, {"Unidade de Medida", Text.Clean, type text}, {"Brasil e Município (Código)", Text.Clean, type text}, {"Brasil e Município", Text.Clean, type text}, {"Variável (Código)", Text.Clean, type text}, {"Variável", Text.Clean, type text}, {"Ano (Código)", Text.Clean, type text}, {"Ano", Text.Clean, type text}})
in
    TextoLimpo
