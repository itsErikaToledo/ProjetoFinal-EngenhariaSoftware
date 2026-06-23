# Diagramas UML — SmartBite

## Diagrama de contexto

```mermaid
---
config:
  theme: base
---
flowchart TB
    %% Bloco central (Sistema)
    subgraph Sistema["Sistema SmartBite"]
      p1["Gerenciar Cardápio Digital"]
      p2["Gerenciar Pedidos e Carrinho"]
      p3["Acompanhar Status do Pedido"]
      p4["Controlar Cozinha (KDS)"]
      p5["Gerenciar Painel do Gerente"]
    end

    %% Entidades externas
    Cliente(["Cliente"])
    Cozinha(["Painel da Cozinha (KDS)"])
    Gerente(["Gerente"])
    Estoque(["Sistema de Estoque"])

    %% Fluxos de dados principais
    Cliente -->|"Acesso via QR Code / Ver Cardápio / Confirmar Pedido"| p1
    Cliente -->|"Itens e Observações"| p2
    p2 -->|"Status do Pedido (Recebido, Em Preparo, Pronto, Entregue)"| Cliente

    p2 -->|"Pedido com Detalhes e Restrições"| Cozinha
    Cozinha -->|"Status do Preparo / Tempo de Espera"| p3

    Gerente -->|"Consultas de Pedidos e Faturamento"| p5
    p5 -->|"Relatórios e Alertas"| Gerente

    p5 -->|"Atualização de Pratos / Controle de Estoque"| Estoque
    Estoque -->|"Disponibilidade de Itens"| p1

    %% Estilo
    classDef externo stroke:#fb923c,fill:#fff7ed
    classDef processo stroke:#818cf8,fill:#eef2ff
    class Cliente,Cozinha,Gerente,Estoque externo
    class p1,p2,p3,p4,p5 processo
```

## Diagrama de Caso de Uso

```mermaid
flowchart LR
    Cliente(("👤 Cliente\nLucas"))
    Chef(("👨‍🍳 Chef\nCarlos"))
    Gerente(("👩‍💼 Gerente\nMariana"))

    subgraph Sistema["SmartBite — Sistema Integrado"]
        direction TB

        subgraph ModCliente["Módulo do Cliente (Web App)"]
            UC01([Escanear QR Code da Mesa])
            UC02([Visualizar Cardápio Digital])
            UC03([Montar Carrinho de Pedido])
            UC04([Adicionar Observações / Restrições])
            UC05([Enviar Pedido])
            UC06([Acompanhar Status de Preparo])
            UC07([Solicitar Fechamento de Conta])
            UC08([Dividir Conta com Amigos])
        end

        subgraph ModCozinha["Módulo da Cozinha (KDS)"]
            UC09([Visualizar Pedidos por Ordem de Chegada])
            UC10([Receber Alertas de Alergias])
            UC11([Atualizar Status do Pedido])
        end

        subgraph ModAdmin["Módulo Administrativo"]
            UC12([Gerenciar Cardápio em Tempo Real])
            UC13([Controlar Estoque de Insumos])
            UC14([Desativar Prato Automaticamente])
            UC15([Visualizar Métricas e Faturamento])
        end
    end

    Cliente --> UC01
    Cliente --> UC02
    Cliente --> UC03
    Cliente --> UC04
    Cliente --> UC05
    Cliente --> UC06
    Cliente --> UC07
    Cliente --> UC08

    Chef --> UC09
    Chef --> UC10
    Chef --> UC11

    Gerente --> UC12
    Gerente --> UC13
    Gerente --> UC14
    Gerente --> UC15

    UC01 -- include --> UC02
    UC03 -- include --> UC04
    UC03 -- include --> UC05
    UC07 -- extend --> UC08
    UC13 -- include --> UC14
```

---

## Diagrama de Sequência — Fluxo Principal (Caminho Feliz do MVP)

> Representa o fluxo validado na Sprint 1: cliente realiza pedido pela mesa e cozinha recebe no KDS.

```mermaid
sequenceDiagram
    actor Cliente
    participant App as 📱 Web App<br/>(QR Code)
    participant Backend as ⚙️ Backend<br/>SmartBite
    participant BD as 🗄️ Banco de<br/>Dados
    participant KDS as 🖥️ KDS<br/>(Cozinha)
    actor Chef

    %% --- Acesso ao Cardápio ---
    Cliente->>App: Escaneia QR Code da mesa
    App->>Backend: GET /cardapio/{mesa_id}
    Backend->>BD: Consulta pratos com estoque ativo
    BD-->>Backend: Lista de pratos disponíveis
    Backend-->>App: Retorna cardápio digital
    App-->>Cliente: Exibe cardápio interativo

    %% --- Realização do Pedido ---
    Cliente->>App: Seleciona itens e adiciona observações
    Note right of Cliente: Ex: "sem cebola",<br/>alerta de alergia
    Cliente->>App: Confirma e envia pedido
    App->>Backend: POST /pedido {itens, observacoes, mesa}
    Backend->>BD: Salva pedido — status: "Recebido"
    BD-->>Backend: Pedido ID gerado
    Backend-->>App: Confirmação do pedido recebido
    App-->>Cliente: "Pedido enviado! Acompanhe o status"

    %% --- Recepção na Cozinha ---
    Backend-)KDS: Notifica novo pedido (tempo real)
    KDS-->>Chef: Exibe pedido por ordem de chegada<br/>com destaque visual para alergias

    %% --- Preparo do Prato ---
    Chef->>KDS: Inicia preparo — atualiza status
    KDS->>Backend: PUT /pedido/{id}/status → "Em Preparo"
    Backend->>BD: Atualiza status do pedido
    Backend-)App: Push: "Seu pedido está sendo preparado"
    App-->>Cliente: Notificação de status atualizado

    %% --- Finalização do Pedido ---
    Chef->>KDS: Marca pedido como "Pronto"
    KDS->>Backend: PUT /pedido/{id}/status → "Pronto"
    Backend->>BD: Atualiza status final
    Backend-)App: Push: "Seu pedido está pronto!"
    App-->>Cliente: Notificação — pedido pronto para retirada

    %% --- Fechamento de Conta ---
    Cliente->>App: Solicita fechamento da conta
    App->>Backend: GET /conta/{mesa_id}
    Backend->>BD: Calcula total de itens consumidos na mesa
    BD-->>Backend: Subtotal, itens e descontos
    Backend-->>App: Retorna resumo detalhado da conta
    App-->>Cliente: Exibe conta com opção de divisão entre amigos

    Cliente->>App: Seleciona forma de pagamento e confirma
    App->>Backend: POST /pagamento {mesa_id, metodo, divisao}
    Backend->>BD: Registra pagamento e encerra mesa
    BD-->>Backend: Transação confirmada
    Backend-->>App: Pagamento aprovado
    App-->>Cliente: Comprovante e encerramento da sessão da mesa
```
## Diagrama de caso de uso

```mermaid
---
config:
  theme: base
  layout: elk
---
classDiagram
    class Mesa {
      +numero: int
      +qrCode: string
    }

    class Cardapio {
      +categorias: List~Categoria~
      +exibir()
    }

    class Categoria {
      +nome: string
      +listarPratos(): List~Prato~
    }

    class Prato {
      +nome: string
      +descricao: string
      +preco: float
      +foto: string
      +ativo: bool
    }

    class Pedido {
      +id: int
      +observacao: string
      +valorTotal: float
      +status: StatusPedido
      +confirmar()
      +atualizarStatus()
    }

    class ItemPedido {
      +quantidade: int
      +subtotal: float
    }

    class PainelCozinha {
      +listarPedidos()
      +destacarRestricoes()
      +tempoDeEspera()
    }

    class PainelGerente {
      +quantidadePedidos()
      +faturamentoDia()
      +controlarEstoque()
      +ativarDesativarPrato()
    }

    class StatusPedido {
      <<enumeration>>
      Recebido
      EmPreparo
      Pronto
      Entregue
    }

    %% Relações
    Mesa "1" --> "1" Pedido : "associa"
    Pedido "1" --> "many" ItemPedido
    ItemPedido "1" --> "1" Prato
    Cardapio "1" --> "many" Categoria
    Categoria "1" --> "many" Prato
    PainelCozinha "1" --> "many" Pedido
    PainelGerente "1" --> "many" Prato
```