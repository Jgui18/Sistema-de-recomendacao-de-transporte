# Fluxograma — Recomendação de transporte público

O diagrama abaixo resume as etapas efetivamente implementadas no notebook principal.

```mermaid
flowchart TD
    A([Início]) --> B[Configurar e autenticar na API Olho Vivo]
    B --> C[Consultar veículos monitorados]
    C --> D[Receber e validar origem e destino]
    D --> E[Pré-selecionar linhas com veículos próximos]
    E --> F[Consultar paradas dessas linhas]
    F --> G{Há paradas próximas aos dois locais?}
    G -- Não --> N[Informar que não há viagem direta identificada]
    G -- Sim --> H[Baixar GTFS e conferir serviços ativos]
    H --> I[Validar a sequência de embarque e desembarque]
    I --> J{Existe viagem direta programada?}
    J -- Não --> N
    J -- Sim --> K[Ordenar opções pela soma das distâncias geográficas]
    K --> L[Selecionar a viagem recomendada]
    L --> M[Consultar previsão na API]
    M --> P{Há previsão disponível?}
    P -- Sim --> Q[Mostrar linha, paradas e horário estimado]
    P -- Não ou falha --> R[Mostrar recomendação sem horário em tempo real]
    Q --> S[Obter geometria da viagem no GTFS]
    R --> S
    S --> T{Geometria e recorte válidos?}
    T -- Sim --> U[Gerar mapa Folium e salvar HTML]
    T -- Não --> V[Informar que o mapa não pôde ser gerado]
    U --> Z([Fim])
    V --> Z
    N --> Z
```

**Observações:** a soma das distâncias até as paradas usa medidas em linha reta; não calcula caminhos reais de pedestres. A pré-seleção por posição dos veículos limita a busca e pode deixar de considerar algumas linhas. A previsão de chegada pode não estar disponível, mas isso não impede a apresentação da recomendação programada.
