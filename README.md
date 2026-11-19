# Arvoredo Urbano para QGIS 🌳

Plugin em português de Portugal para **inventariar árvores urbanas no mapa** e acompanhar inspeções e intervenções ao longo do tempo.

> **Estado:** versão experimental 0.1.1. Teste primeiro o fluxo de trabalho com um inventário de demonstração.

## Funcionalidades

| Área | O que permite fazer |
| --- | --- |
| Inventário | Criar árvores por clique no mapa, editar a localização e manter uma ficha por exemplar. |
| Caracterização | Registar espécies, freguesia, local, entidade de gestão, altura, DAP, diâmetro da copa, estado e observações. |
| Inspeções | Guardar data, técnico, defeitos, recomendações, risco e próxima inspeção; consultar o histórico da árvore. |
| Intervenções | Registar tipo, responsável, estado e prazo; acompanhar ações planeadas e em curso. |
| Consulta | Pesquisar por código, espécie ou local, filtrar por risco e visualizar as árvores no mapa por cor. |
| Exportação | Gerar três ficheiros CSV: árvores, inspeções e intervenções. |

Os dados ficam num **GeoPackage local** (`.gpkg`). A classificação de risco é registada pelo utilizador; o plugin não calcula risco biomecânico nem substitui a avaliação por um técnico habilitado.

## Instalação

1. Descarregue o ficheiro [`Arvoredo_Urbano_QGIS_0.1.1.zip`](dist/Arvoredo_Urbano_QGIS_0.1.1.zip) deste repositório. Se o GitHub apresentar a pré-visualização, utilize **Download raw file**.
2. No QGIS, abra **Plugins → Gerir e instalar plugins → Instalar a partir de ZIP**.
3. Selecione o ZIP e ative **Arvoredo Urbano**.
4. Abra o painel em **Vetor → Arvoredo urbano** ou pelo ícone da barra de ferramentas.

O ZIP contém a pasta `arvoredo_urbano` e não requer bibliotecas Python adicionais. Esta versão foi ensaiada em **QGIS 3.40.6**; o `metadata.txt` indica compatibilidade mínima com QGIS 3.34. Ainda não foi validada em QGIS 4.

### Atualização da versão 0.1.0

Instale o ZIP 0.1.1 e reinicie o QGIS. Se o gestor não permitir a substituição, desinstale a versão anterior no gestor e instale a nova. O inventário `.gpkg` é independente da instalação do plugin; a abertura e edição do formato anterior foram ensaiadas nesta versão.

## Primeiros passos

1. Clique em **Criar inventário** e escolha o nome e a localização do GeoPackage. Também pode usar **Abrir** para selecionar um inventário existente.
2. Clique em **Nova árvore no mapa** e marque a posição. Preencha o código e pelo menos o nome comum ou científico da espécie; depois clique em **Guardar ficha**.
3. Selecione a árvore na lista para editar os seus dados. Pode usar **Mover no mapa** para corrigir a posição.
4. Na aba **Inspeções**, registe a observação e o técnico. A inspeção mais recente atualiza os campos de síntese da árvore.
5. Na aba **Intervenções**, registe as ações. Consulte **Pendentes** para ver as intervenções planeadas ou em curso; os prazos ultrapassados aparecem a vermelho.
6. Use **Exportar CSV** para obter os três ficheiros de dados.

As novas árvores são guardadas em **ETRS89 / Portugal TM06 (EPSG:3763)**. O ponto marcado é transformado a partir do sistema de referência do mapa. O CSV das árvores inclui longitude e latitude em WGS 84 (EPSG:4326), usa UTF-8 com BOM e separador `;`.

## Estrutura do projeto

```text
arvoredo_urbano/      Código-fonte do plugin e ensaios
dist/                 ZIP instalável no QGIS
README.md             Este documento
```

No GeoPackage, a camada `arvores` guarda os pontos e atributos. As tabelas `au_inspecoes` e `au_intervencoes` guardam o histórico associado ao identificador único de cada árvore. Para cópias de segurança, copie o `.gpkg` com o QGIS fechado.

## Limitações atuais

- Não existe importação automática de inventários existentes, anexação de fotografias ou sincronização entre vários utilizadores.
- Os registos históricos não têm editor completo; o estado das intervenções pode ser alterado.
- A exclusão direta de árvores na tabela de atributos do QGIS pode deixar registos históricos sem ficha correspondente. Para manter o histórico, desmarque **Árvore presente / ativa** na ficha.
- Antes de guardar pelo painel, termine ou cancele qualquer sessão de edição aberta na camada do QGIS.

## Verificação técnica

Com PyQGIS disponível, a partir da pasta que contém `arvoredo_urbano`:

```bash
QT_QPA_PLATFORM=offscreen PYTHONPATH=. /usr/bin/python3 arvoredo_urbano/tests/smoke.py
QT_QPA_PLATFORM=offscreen PYTHONPATH=. /usr/bin/python3 arvoredo_urbano/tests/ui_smoke.py
QT_QPA_PLATFORM=offscreen PYTHONPATH=. /usr/bin/python3 arvoredo_urbano/tests/import_without_sqlite.py
```

A versão 0.1.1 usa o fornecedor GeoPackage do QGIS para o histórico e evita importar o módulo Python `_sqlite3`, que foi bloqueado por uma política de controlo de aplicações num ambiente Windows.

## Autor e colaboração

Desenvolvido por **Bruno Raposo**. Sugestões e relatos de erros são bem-vindos nos *Issues* do repositório. Inclua a versão do QGIS, a versão do plugin, os passos para reproduzir o problema e a mensagem de erro, retirando previamente dados pessoais ou sensíveis.
