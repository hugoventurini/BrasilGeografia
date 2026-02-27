let
    // Conexão com a fonte de dados - arquivo CSV online
    Fonte = Csv.Document(
        Web.Contents("https://raw.githubusercontent.com/kelvins/Municipios-Brasileiros/main/csv/municipios.csv"),
        [Delimiter=",", Encoding=65001, QuoteStyle=QuoteStyle.Csv]
    ),

    // Cabeçalhos promovidos
    Promoted = Table.PromoteHeaders(Fonte, [PromoteAllScalars=true]),
    // Tipificação dos dados de acordo com sua natureza e finalidade.
    Tipos = Table.TransformColumnTypes(Promoted, {{"latitude", type number}, {"longitude", type number}}, "en-US")
in

    Tipos
