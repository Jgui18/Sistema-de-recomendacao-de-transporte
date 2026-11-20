# Sistema de Recomendação de Transporte Público — SPTrans

Projeto acadêmico em **Python e Jupyter Notebook** que sugere uma viagem de ônibus **sem baldeação** na cidade de São Paulo a partir das coordenadas de origem e destino informadas pelo usuário.

## Funcionalidades

- Autenticação na API **Olho Vivo** da SPTrans.
- Consulta de veículos monitorados e pré-seleção de linhas candidatas.
- Busca das paradas de cada linha e verificação de proximidade à origem e ao destino.
- Validação da ordem de embarque e desembarque com os dados **GTFS** (paradas, viagens e calendário).
- Seleção de uma recomendação por menor soma de distâncias geográficas até as paradas; desempate pela quantidade de veículos monitorados.
- Consulta da previsão de chegada, quando disponível na API.
- Mapa interativo em **Folium** com a geometria da linha e o trecho recomendado, exportado como `mapa_recomendacao.html`.
- Mensagens para casos sem viagem compatível, falta de previsão e falha na geração do mapa.

## Requisitos

- Python 3.12 (ou versão compatível com as dependências)
- Conexão à internet para consultar a API e baixar o GTFS
- Token pessoal da API Olho Vivo (obtido no portal de desenvolvedores da SPTrans)

## Instalação

No terminal, na pasta do projeto:

```bash
python -m venv .venv
```

No **Windows (PowerShell)**:

```powershell
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

Abra o notebook no VS Code ou Jupyter e selecione o kernel da `.venv`.

## Como usar

1. Abra `sistemas_recomendacao_transporte_final.ipynb`.
2. Execute as células em ordem, a partir da primeira.
3. Informe seu **token da SPTrans** quando solicitado. O token é digitado sem aparecer na tela e não deve ser salvo no GitHub.
4. Informe a latitude e longitude da origem e do destino, como números decimais.
5. Aguarde as consultas e leia a recomendação exibida no notebook.
6. Se uma viagem compatível for encontrada e houver geometria GTFS, abra `mapa_recomendacao.html` no navegador.

Exemplo de **coordenadas para teste**, não fixadas no código:

| Local | Latitude | Longitude |
|---|---:|---:|
| Origem | -23.534377 | -46.654322 |
| Destino | -23.525653 | -46.679493 |

## Interpretação do mapa

- **Cinza:** itinerário completo planejado segundo o GTFS.
- **Azul grosso:** trecho recomendado entre as paradas de embarque e desembarque.
- **Verde e vermelho pontilhados:** ligações ilustrativas, em **linha reta**, até as paradas; **não são rotas de caminhada pelas ruas**.
- **Azul fino pontilhado:** conexão cartográfica entre a parada e o eixo aproximado da geometria GTFS.

## Limitações

- A pré-seleção usa veículos monitorados nas proximidades e analisa um número limitado de linhas; **não é uma busca exaustiva de toda a rede**.
- O GTFS descreve viagens e trajetos **programados**, não garante que um ônibus esteja operando naquele momento.
- A previsão em tempo real depende da disponibilidade da API e pode não estar disponível.
- As distâncias entre o usuário e as paradas são calculadas em **linha reta**, não por roteamento de pedestres.
- O recorte da geometria do ônibus usa projeções sobre os segmentos GTFS. Em trajetos com laços, retornos ou cruzamentos, o desenho deve ser conferido.
- É necessário consultar a cobertura da API Olho Vivo e a atualidade do GTFS para outras regiões ou datas.

## Estrutura principal

```text
├── sistemas_recomendacao_transporte_final.ipynb  # Sistema principal
├── testes_e_validacao.ipynb                       # Testes da API / GTFS
├── testes_e_validacao_mapa.ipynb                  # Investigação do mapa
├── requirements.txt
├── FLUXOGRAMA.md                                   # Fluxo do programa
└── .gitignore
```

## Fluxograma

O fluxo de execução está documentado em [FLUXOGRAMA.md](FLUXOGRAMA.md).

## Boas práticas

Não adicione a `.venv/`, o token da API ou os HTMLs temporários ao repositório. Antes de publicar um notebook, revise as saídas das células para evitar compartilhar informações acidentalmente.
