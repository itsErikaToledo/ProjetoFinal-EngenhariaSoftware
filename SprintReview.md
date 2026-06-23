# Relatório de Sprint Review - Sprint 1

## Informações Gerais
* **Projeto:** SmartBite - Sistema Integrado de Autoatendimento e Gestão de Cozinha
* **Data da Reunião:** [18/06/2026]
* **Duração:** [1h30min]
* **Participantes:** * **Product Owner:** Larissa S. Pereira
  * **Developers:** Erika Toledo, Talita Braz, Karen Evelyn
  * **Stakeholders/Avaliadores:** [Diretoria do Gourmet Express e Banca de Avaliação Técnica]

---

## Meta da Sprint 1
Garantir o funcionamento do "caminho feliz" do MVP: leitura do QR Code, visualização do cardápio, envio do pedido com observações pelo cliente e recepção organizada no painel da cozinha (KDS).

* **A Meta foi atingida?** [ ] Sim  [X] Parcialmente  [ ] Não
* **Justificativa:** [O fluxo principal de ponta a ponta (Cliente fazendo o pedido e Cozinha recebendo na tela) funcionou com sucesso no ambiente de homologação. No entanto, o módulo administrativo de estoque inteligente estourou o tempo de desenvolvimento devido a uma complexidade técnica inesperada na API e precisou ser cortado da entrega final desta Sprint.].

---

## Status do Backlog (Critério de Pronto - DoD)

### Itens Aceitos (Entregues e Validados)
* **[US01] Acesso via QR Code e Cardápio:** Cliente consegue escanear o código e visualizar os pratos e preços sem travar.
* **[US02] Realização de Pedido:** Cliente monta o carrinho, adiciona observações (ex: "sem cebola") e envia o pedido.
* **[US03] Painel da Cozinha (KDS):** Os pedidos entram na tela por ordem de chegada com alertas visuais de tempo.

### Itens Rejeitados / Incompletos (Retornam ao Backlog)
* *[[US04] Desativação Automática de Prato por Falta de Insumo: O time de desenvolvimento encontrou problemas na sincronização em tempo real entre o banco de dados de insumos e o cardápio. Para não gerar bugs na tela do cliente, a funcionalidade foi retida e retornará para o topo do backlog da Sprint 2.].*

---

## 💬 Feedbacks dos Stakeholders & Pensamento Crítico

Durante a inspeção do software, foram levantadas as seguintes questões e sugestões:

* **Pergunta Central:** *Como a interface do KDS ajuda o Chef Carlos sob alta pressão?*
  * **Feedback coletado:** O destaque visual para alergias funcionou bem, mas a fonte dos itens do pedido precisa ser maior para visualização rápida à distância na cozinha.
* **Pergunta Central:** *O fluxo de fechamento de conta confunde o usuário?*
  * **Feedback coletado:** Sugerido incluir um botão de "chamar garçom" visível na tela do cardápio caso o cliente tenha problemas com o pagamento digital.

---

## 🔄 Adaptação do Backlog para a Sprint 2

Com base nos feedbacks coletados, as seguintes ações serão tomadas para o próximo ciclo:

1. **Correção/Ajuste (Débito Técnico):** Aumentar o contraste e o tamanho da fonte no módulo KDS (Cozinha).
2. **Priorização da Sprint 2:** Trazer a história de [Fechamento e Divisão de Conta] para o topo do desenvolvimento, junto com a finalização do módulo de estoque que ficou pendente.