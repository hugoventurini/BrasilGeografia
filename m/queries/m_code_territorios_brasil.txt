let
  // Conexão com a tabela 4714 da base SIDRA-IBGE que contém dados do CENSO 2022.
  Fonte = Json.Document(Web.Contents("https://apisidra.ibge.gov.br/values/t/4714/n1/all/n6/all/v/6318/p/all/d/v6318%203")),
  // Conversãoo do conjunto de registros retornados pela chamada da API em tabela.
  Tabela = Table.FromList(Fonte, Splitter.SplitByNothing(), null, null, ExtraValues.Error),
  // Expansãoo dos registros para obtenção do volume de dados bruto.
  Expandido = Table.ExpandRecordColumn(Tabela, "Column1", {"NC", "NN", "MC", "MN", "V", "D1C", "D1N", "D2C", "D2N", "D3C", "D3N"}, {"NC", "NN", "MC", "MN", "V", "D1C", "D1N", "D2C", "D2N", "D3C", "D3N"}),
  // Promoção da primeira linha para ajuste dos cabeçalhos.
  Headers = Table.PromoteHeaders(Expandido, [PromoteAllScalars=true]),
    // Exclusão de linha fora do padrão desejado na base
    LinhasFiltradas = Table.SelectRows(Headers, each ([#"Nível Territorial (Código)"] = "6")),
    // Tipificação das colunas de acordo com sua natureza e finalidade
    Tipo = Table.TransformColumnTypes(LinhasFiltradas, {{"Valor", type number}}, "en-US"),
    // Eliminação de espaços excedentes e desnecessários dos campos tipificados como texto.
    TextoAparado = Table.TransformColumns(Tipo,{{"Nível Territorial (Código)", Text.Trim, type text}, {"Nível Territorial", Text.Trim, type text}, {"Unidade de Medida (Código)", Text.Trim, type text}, {"Unidade de Medida", Text.Trim, type text}, {"Brasil e Município (Código)", Text.Trim, type text}, {"Brasil e Município", Text.Trim, type text}, {"Variável (Código)", Text.Trim, type text}, {"Variável", Text.Trim, type text}, {"Ano (Código)", Text.Trim, type text}, {"Ano", Text.Trim, type text}}),
    // Eliminação de quebras de linha, caracteres não imprimíveis e tabulações dos campos tipificados como texto.
    TextoLimpo = Table.TransformColumns(TextoAparado,{{"Nível Territorial (Código)", Text.Clean, type text}, {"Nível Territorial", Text.Clean, type text}, {"Unidade de Medida (Código)", Text.Clean, type text}, {"Unidade de Medida", Text.Clean, type text}, {"Brasil e Município (Código)", Text.Clean, type text}, {"Brasil e Município", Text.Clean, type text}, {"Variável (Código)", Text.Clean, type text}, {"Variável", Text.Clean, type text}, {"Ano (Código)", Text.Clean, type text}, {"Ano", Text.Clean, type text}})
in
    TextoLimpo
