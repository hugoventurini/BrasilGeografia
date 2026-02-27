# Brasil Geografia — Power BI

Projeto de visualização de dados geográficos do Brasil em Power BI, abrangendo Regiões, Estados (UF), Regiões Intermediárias, Regiões Imediatas e Municípios, com base nas malhas do IBGE.

---

## Visão Geral

Este repositório compartilha o template Power BI (`.pbit`), as consultas M (Power Query), documentação e assets visuais do projeto **Brasil Geografia**. O arquivo `.pbix` (que contém dados locais) **não** é versionado aqui — apenas o template e os scripts são mantidos.

---

## Estrutura do Repositório

```
BrasilGeografia/
├── powerbi/
│   └── template/          # Template Power BI (.pbit) — abrir aqui
├── m/
│   ├── queries/           # Scripts M de consultas (.m ou .pq)
│   └── functions/         # Funções M reutilizáveis (.m ou .pq)
├── docs/
│   ├── fontes.md          # Fontes de dados utilizadas
│   └── dicionario-dados.md  # Dicionário de dados / glossário
├── assets/                # Imagens, screenshots e ícones do relatório
├── AR_BR_RG_UF_RGINT_MES_MIC_MUN_2022.xlsx  # Dados de referência IBGE
├── .gitignore
├── LICENSE
└── README.md
```

---

## Como Abrir o Template `.pbit`

1. Faça o download (ou clone) deste repositório.
2. Acesse a pasta `powerbi/template/`.
3. Abra o arquivo `BrasilGeografia.pbit` com o **Power BI Desktop** (versão gratuita disponível em [powerbi.microsoft.com](https://powerbi.microsoft.com/pt-br/desktop/)).
4. O Power BI solicitará que você informe o caminho da fonte de dados (planilha Excel ou outra fonte configurada).
5. Aponte para o arquivo correspondente em seu ambiente local e clique em **Carregar**.
6. O relatório será populado com seus dados locais.

> **Nota:** O `.pbit` é um template sem dados — cada usuário conecta à sua própria cópia dos dados.

---

## Como Atualizar / Editar Consultas M

As consultas M (Power Query) ficam em `m/queries/` e as funções reutilizáveis em `m/functions/`.

### Editar no Power BI Desktop

1. Abra o relatório (`.pbit` ou `.pbix`) no Power BI Desktop.
2. Vá em **Página Inicial → Transformar Dados** para abrir o Power Query Editor.
3. Selecione a consulta desejada e abra o **Editor Avançado** (`Exibir → Editor Avançado`).
4. Copie o código M correspondente do arquivo em `m/queries/`.
5. Após editar, clique em **Concluído** e em **Fechar e Aplicar**.

### Nomear os Arquivos

- Consultas: `m/queries/<NomeDaConsulta>.m` (ex.: `m/queries/Municipios.m`)
- Funções: `m/functions/<NomeDaFuncao>.m` (ex.: `m/functions/fnNormalizarTexto.m`)

---

## Onde Colocar Cada Arquivo

| Arquivo / Tipo | Pasta destino | Exemplo de nome |
|---|---|---|
| Template Power BI | `powerbi/template/` | `BrasilGeografia.pbit` |
| Script de consulta M | `m/queries/` | `Municipios.m` |
| Função M reutilizável | `m/functions/` | `fnNormalizarTexto.m` |
| Screenshot / imagem | `assets/` | `screenshot-mapa-brasil.png` |
| Documentação adicional | `docs/` | `dicionario-dados.md` |

---

## Como Publicar / Atualizar (para o autor)

1. **Exportar o template:** No Power BI Desktop, vá em **Arquivo → Exportar → Modelo do Power BI** e salve como `powerbi/template/BrasilGeografia.pbit`.
2. **Exportar consultas M:** Para cada consulta/função, abra o Editor Avançado, copie o código e salve no arquivo `.m` correspondente em `m/queries/` ou `m/functions/`.
3. **Adicionar screenshots:** Salve imagens do relatório em `assets/` (formato `.png` ou `.jpg`).
4. **Atualizar documentação:** Edite `docs/fontes.md` e `docs/dicionario-dados.md` conforme necessário.
5. **Commit e push:**
   ```bash
   git add powerbi/template/BrasilGeografia.pbit m/ assets/ docs/
   git commit -m "feat: atualiza template e consultas M"
   git push
   ```
6. Abra um Pull Request (se estiver em branch) ou faça push direto na `main`.

> **Atenção:** Nunca versione o arquivo `.pbix` neste repositório público (ele pode conter dados sensíveis ou volumosos).

---

## Como Contribuir

1. Fork este repositório.
2. Crie uma branch descritiva: `git checkout -b feat/minha-melhoria`.
3. Faça suas alterações (scripts M, documentação, assets).
4. Abra um Pull Request com uma descrição clara das mudanças.

Contribuições de melhorias em consultas M, documentação ou visualizações são bem-vindas!

---

## Licença

Este projeto está licenciado sob a [MIT License](LICENSE). Veja o arquivo `LICENSE` para mais detalhes.
