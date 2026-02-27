let
    // Base dos municípios.
    Fonte = Table.NestedJoin(Municipios, {"Cód. Mun. IBGE"}, Populacional, {"Brasil e Município (Código)"}, "Populacional", JoinKind.LeftOuter),
    // Obtenção da população dos municípios
    PopulacionalExpandido = Table.ExpandTableColumn(Fonte, "Populacional", {"Valor"}, {"População"}),
    // Mescla com a consulta do PIB
    PIB = Table.NestedJoin(PopulacionalExpandido, {"Cód. Mun. IBGE"}, PIB, {"Município (Código)"}, "PIB", JoinKind.LeftOuter),
    // Obtenção do valor do PIB por município.
    PIBExpandido = Table.ExpandTableColumn(PIB, "PIB", {"Valor", "Valor Ajustado"}, {"PIB", "PIB Ajustado"}),
    // Mescla com a consulta de territórios.
    Territorios = Table.NestedJoin(PIBExpandido, {"Cód. Mun. IBGE"}, Territorios, {"Brasil e Município (Código)"}, "Territorios", JoinKind.LeftOuter),
    // Obtenção dos valores dos territórios.
    TerritoriosExpandido = Table.ExpandTableColumn(Territorios, "Territorios", {"Valor"}, {"Território (km²)"}),
    // Mescla com a consulta que contém as coordenadas e outras informãções.
    Coordenadas = Table.NestedJoin(TerritoriosExpandido, {"Cód. Mun. IBGE"}, Coordenadas, {"codigo_ibge"}, "Coordenadas", JoinKind.LeftOuter),
    // Obtenção das coordenadas e demais dados.
    CoordenadasExpandidas = Table.ExpandTableColumn(Coordenadas, "Coordenadas", {"latitude", "longitude", "siafi_id", "ddd", "fuso_horario"}, {"Latitude", "Longitude", "Siafi", "DDD", "Fuso Horário"}),
    // Cálculo do PIB per Capita.
    PIBPerCapita = Table.AddColumn(CoordenadasExpandidas, "PIB per Capita", each [PIB Ajustado] / [População], type number),
    // Cálculo da densidade demográfica.
    Densidade = Table.AddColumn(PIBPerCapita, "Densidade Demográfica", each [População] / [#"Território (km²)"], type number),
    // Coluna mesclada,  compondo a descrição da localidade, com município, estado e país.
    Localidade = Table.AddColumn(Densidade, "Localidade", each [Município] & ", " & [Estado] & ", " & "Brasil", type text),
    // Criação de coluna personalizada, com o nome do município, sucedido pela UF.
    Local = Table.AddColumn(Localidade, "Local", each Text.Combine({[Município], [UF]}, ", "), type text)
in
    Local
