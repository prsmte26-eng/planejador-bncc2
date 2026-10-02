# Feature Specification: Planejador BNCC para professores

**Feature Branch**: `main (branch atual)`

**Created**: 2026-10-01

**Status**: Draft

**Input**: User description: "Desenvolver o Planejador BNCC para uso de professores, com login por duas contas de demonstração, consulta e seleção de habilidades, geração assistida de planos privados em rascunho, edição Markdown, pré-visualização, salvamento explícito, lista privada, tratamento de falhas, sem cadastro público nem administração, PDF, finalização, versionamento ou publicação pública."

## Clarifications

### Session 2026-10-01

- Q: Após quanto tempo sem atividade a sessão do professor deve expirar? → A: A sessão permanece ativa até logout ou fechamento do navegador; não expira por inatividade.
- Q: Que conteúdo mínimo torna válida uma resposta gerada pela IA? → A: Título e uma ou mais atividades identificáveis.
- Q: Se a geração não responder em até 60 segundos, o que deve acontecer? → A: Informar falha, preservar o pedido e aguardar nova tentativa manual.
- Q: Quais dados podem ser enviados ao serviço de IA durante uma geração? → A: Somente a instrução pedagógica, as habilidades selecionadas, a duração e a escolha de recursos digitais; sem identidade da conta ou outros planos salvos.
- Q: Se o mesmo rascunho for editado em duas abas e uma delas salvar primeiro, o que deve acontecer quando a outra tentar salvar? → A: Impedir a sobrescrita e avisar que o rascunho mudou.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Entrar e selecionar habilidades BNCC (Priority: P1)

O professor entra com uma das contas de demonstração e encontra as habilidades relevantes
para seu planejamento por nível, ano quando aplicável, eixo, código ou texto. Pode selecionar
uma ou mais habilidades para compor o pedido de plano.

**Why this priority**: Identidade do professor e seleção curricular formam a entrada necessária
para produzir um plano alinhado ao trabalho docente.

**Independent Test**: Entrar com uma conta de demonstração, localizar habilidades usando cada
critério de consulta e verificar que uma seleção múltipla fica visível no pedido.

**Acceptance Scenarios**:

1. **Given** uma conta de demonstração válida, **When** o professor envia suas credenciais,
   **Then** entra na área autenticada e pode abrir o catálogo BNCC.
2. **Given** credenciais inválidas, **When** o professor tenta entrar,
   **Then** o acesso é negado e a tela informa que as credenciais não foram aceitas.
3. **Given** o catálogo aberto, **When** o professor filtra por nível, ano aplicável ou eixo,
   **Then** vê somente habilidades compatíveis com os filtros ativos.
4. **Given** o catálogo aberto, **When** o professor pesquisa por código ou texto,
   **Then** vê habilidades correspondentes e pode selecionar uma ou mais delas.
5. **Given** habilidades selecionadas, **When** o professor revisa o pedido,
   **Then** os códigos e descrições selecionados estão identificados antes da geração.
6. **Given** o catálogo e uma ou mais habilidades selecionadas, **When** o professor remove
   uma habilidade, **Then** ela deixa de fazer parte do pedido sem alterar o registro do catálogo.

---

### User Story 2 - Gerar um rascunho sem perder o pedido (Priority: P1)

O professor informa uma instrução pedagógica, a duração em minutos e se utilizará recursos
digitais. Ao confirmar, acompanha a preparação. Uma resposta válida cria um plano privado em
estado RASCUNHO, identificado como auxiliado por IA. Se houver falha, pode corrigir ou tentar
novamente sem reconstruir o pedido.

**Why this priority**: A geração converte a seleção curricular em um plano utilizável e precisa
preservar a confiança do professor tanto no sucesso quanto na falha.

**Independent Test**: Enviar um pedido válido e conferir o rascunho privado; repetir com falha
simulada e conferir que não há plano parcial e que uma nova tentativa exige ação explícita.

**Acceptance Scenarios**:

1. **Given** ao menos uma habilidade selecionada e os campos obrigatórios válidos,
   **When** o professor confirma a geração, **Then** vê imediatamente o estado de preparação.
2. **Given** uma solicitação em preparação, **When** o serviço retorna conteúdo válido,
   **Then** é criado um rascunho privado com estado RASCUNHO e indicação de auxílio por IA,
   associado ao professor que o solicitou.
3. **Given** uma solicitação em preparação, **When** o serviço falha ou retorna conteúdo
   inválido, **Then** nenhum plano parcial é salvo, os campos e habilidades selecionadas são
   preservados e uma mensagem explica a falha.
4. **Given** uma geração que falhou, **When** o professor não aciona novamente a geração,
   **Then** nenhum novo pedido é enviado automaticamente.
5. **Given** duração ausente, zero, negativa, fracionária ou não numérica, ou instrução pedagógica vazia,
   **When** o professor tenta gerar, **Then** a validação indica o campo a corrigir e não
   inicia a geração.
6. **Given** um pedido válido, **When** o professor indica se usará recursos digitais,
   **Then** a escolha fica visível no pedido e é mantida durante a geração.
7. **Given** que a geração continua sem resposta ao completar 60 segundos,
   **When** o limite é atingido, **Then** o sistema informa falha, preserva o pedido e aguarda
   uma nova tentativa iniciada pelo professor.
8. **Given** um pedido confirmado, **When** os dados são enviados ao serviço de IA,
   **Then** incluem somente a instrução, as habilidades selecionadas, a duração e a escolha de
   recursos digitais, sem identidade da conta nem conteúdo de outros planos salvos.

---

### User Story 3 - Revisar, editar e salvar o plano (Priority: P1)

O professor abre o rascunho gerado, lê e edita seu conteúdo em Markdown, alterna para a
pré-visualização e salva explicitamente as alterações. Depois, pode encontrar e reabrir o
rascunho na lista dos próprios planos.

**Why this priority**: A revisão docente é obrigatória para transformar a sugestão da IA em
material que o professor controla e pode continuar usando.

**Independent Test**: Abrir um rascunho existente, alterar o Markdown, conferir a
pré-visualização, salvar explicitamente e reabrir para verificar a persistência.

**Acceptance Scenarios**:

1. **Given** um rascunho gerado, **When** o professor o abre, **Then** vê o conteúdo Markdown,
   o estado RASCUNHO e a indicação de auxílio por IA.
2. **Given** o editor de um rascunho, **When** o professor altera o Markdown,
   **Then** pode alternar para a pré-visualização e conferir a apresentação correspondente.
3. **Given** alterações ainda não salvas, **When** o professor não escolhe salvar,
   **Then** o conteúdo persistido do rascunho não é substituído pelas alterações pendentes.
4. **Given** alterações no editor, **When** o professor aciona salvar,
   **Then** a versão atual do conteúdo é persistida e uma confirmação é apresentada.
5. **Given** um rascunho salvo, **When** o professor abre a lista de planos,
   **Then** encontra seus rascunhos e pode reabrir o plano salvo.
6. **Given** a tela de edição, **When** o professor navega pelos controles usando teclado,
   **Then** os controles principais recebem foco visível e podem ser acionados sem mouse.
7. **Given** o mesmo rascunho aberto em duas abas e uma delas já salvou alterações,
   **When** a outra aba tenta salvar conteúdo desatualizado, **Then** o salvamento é recusado,
   o conteúdo já salvo permanece intacto e o professor recebe um aviso para recarregar.

---

### User Story 4 - Manter os planos privados por professor (Priority: P1)

Cada professor vê e altera somente os próprios rascunhos. As duas contas de demonstração
permitem verificar que conhecer ou informar a referência de outro plano não concede acesso.

**Why this priority**: A privacidade dos planos é uma condição essencial para disponibilizar o
fluxo de edição a mais de um professor.

**Independent Test**: Criar rascunhos com as duas contas e, autenticado em cada uma, tentar
listar, abrir, alterar e salvar o rascunho pertencente à outra.

**Acceptance Scenarios**:

1. **Given** dois professores com rascunhos próprios, **When** cada um consulta sua lista,
   **Then** vê somente os seus planos.
2. **Given** um professor autenticado, **When** tenta abrir ou alterar um rascunho de outro,
   **Then** o acesso é negado e o conteúdo privado não é revelado.
3. **Given** duas contas de demonstração, **When** o professor encerra a sessão e entra com a
   outra conta ou fecha o navegador e depois retorna, **Then** a sessão anterior não autoriza
   operações e somente os planos da nova conta ficam disponíveis após novo login.

### Edge Cases

- Busca sem resultados deve apresentar estado vazio claro e permitir limpar ou alterar os
  filtros.
- Filtros combinados sem correspondências não devem mostrar habilidades fora dos critérios.
- O catálogo pode conter campos de ano ou eixo não aplicáveis; esses casos devem ser exibidos
  sem atribuir um valor inexistente.
- Uma solicitação pode demorar até 60 segundos, falhar, retornar resposta vazia ou em formato
  inválido; ao atingir o limite ou detectar falha, o sistema preserva os campos e não cria um
  rascunho parcial.
- A resposta pode falhar depois de o professor deixar a tela; o sistema não deve criar mais de
  um rascunho para a mesma tentativa.
- Uma sessão encerrada por logout ou fechamento do navegador não pode continuar autorizando
  leitura ou edição de planos; deixar o navegador aberto sem atividade não encerra a sessão.
- Dados de perfil e conteúdo de outros planos não são contexto implícito de uma solicitação de
  geração e não são enviados ao serviço de IA.
- Falha ao salvar alterações deve manter o rascunho persistido anterior e informar que a
  alteração não foi salva.
- Se outra aba salvar o mesmo rascunho primeiro, uma tentativa desatualizada de salvamento deve
  ser recusada sem substituir o conteúdo mais recente.
- Texto Markdown vazio ou inválido não pode substituir o último conteúdo salvo.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: O sistema MUST oferecer login para exatamente duas contas de demonstração
  previamente cadastradas e MUST NOT oferecer cadastro público ou administração de contas.
- **FR-002**: O sistema MUST manter sessão autenticada entre ações do professor e oferecer
  logout que encerre o acesso da sessão encerrada.
- **FR-003**: O sistema MUST fornecer um catálogo mínimo de habilidades BNCC, somente para
  consulta, identificadas por nível e, quando aplicável, ano, eixo, código e descrição.
- **FR-004**: O professor MUST poder filtrar por nível, ano aplicável e eixo, e pesquisar por
  código ou texto; critérios informados em conjunto MUST restringir conjuntamente os resultados.
- **FR-005**: O professor MUST poder selecionar e remover uma ou mais habilidades antes de
  enviar a solicitação de geração.
- **FR-006**: O pedido MUST permitir uma instrução pedagógica, duração inteira positiva em
  minutos e indicação de uso de recursos digitais.
- **FR-007**: O sistema MUST validar campos obrigatórios e confirmar que a duração é um número
  inteiro positivo antes de solicitar uma geração; MUST apresentar indicação clara dos campos
  inválidos.
- **FR-008**: Ao confirmar um pedido válido, o sistema MUST apresentar um estado de preparação
  enquanto aguarda a resposta do serviço de IA, por no máximo 60 segundos.
- **FR-009**: O sistema MUST validar a resposta da IA antes de criar o plano; resposta válida
  significa conteúdo Markdown não vazio com título e pelo menos uma atividade identificável.
- **FR-010**: Para cada resposta válida, o sistema MUST salvar um plano privado pertencente ao
  professor solicitante, em estado RASCUNHO e com indicação de auxílio por IA.
- **FR-011**: Falha, resposta vazia, inválida ou não recebida em até 60 segundos MUST NOT criar
  plano parcial; o pedido, a instrução, a duração, a opção de recursos digitais e as habilidades
  selecionadas MUST ser preservados para correção ou nova tentativa.
- **FR-012**: Uma nova tentativa de geração MUST ocorrer somente após ação explícita do professor.
- **FR-013**: O sistema MUST exibir o Markdown do rascunho, permitir sua edição e oferecer uma
  pré-visualização correspondente ao conteúdo atual do editor.
- **FR-014**: Alterações feitas no editor MUST ser persistidas somente após ação explícita de
  salvar; falha ao salvar MUST preservar o último conteúdo persistido e informar o resultado.
- **FR-015**: O professor MUST poder consultar a lista dos próprios rascunhos e reabrir qualquer
  rascunho seu que tenha sido salvo.
- **FR-016**: Toda leitura ou alteração de rascunho MUST ser autorizada para o professor
  proprietário; tentativa de outro professor MUST ser negada sem revelar o conteúdo.
- **FR-017**: O sistema MUST oferecer logout, encerrar a sessão ao fechar o navegador e MUST
  impedir que uma sessão encerrada continue autorizando acesso a planos privados; a sessão não
  expira por inatividade enquanto o navegador permanece aberto.
- **FR-018**: O fluxo MUST ser utilizável em desktop, tablet e celular e MUST comunicar foco,
  carregamento, vazio, sucesso, validação e falha de forma acessível e consistente com o
  Design System de referência.
- **FR-019**: A primeira versão MUST NOT incluir exportação PDF, finalização de planos,
  versionamento de planos ou publicação pública.
- **FR-020**: O sistema MUST enviar ao serviço de IA somente a instrução pedagógica, as
  habilidades selecionadas, a duração e a escolha de recursos digitais; MUST NOT enviar a
  identidade da conta nem conteúdo de outros planos salvos.
- **FR-021**: Se um rascunho for salvo em outra aba depois que o professor abriu o conteúdo
  atual, o sistema MUST recusar o salvamento desatualizado, preservar o conteúdo já salvo e
  informar ao professor que recarregue o rascunho.

### Key Entities *(include if feature involves data)*

- **Professor**: Conta de demonstração autenticável que possui seus próprios rascunhos.
- **Habilidade BNCC**: Referência curricular consultável, com nível, ano e eixo quando
  aplicáveis, código e descrição.
- **Solicitação de planejamento**: Instrução do professor, duração, indicação de recursos
  digitais e conjunto de habilidades escolhidas para geração.
- **Plano**: Conteúdo Markdown gerado e editável, associado ao professor proprietário, com
  estado RASCUNHO e indicação de auxílio por IA.
- **Sessão**: Contexto de acesso autenticado que identifica o professor até logout ou expiração.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: Em uma execução de aceitação, um professor completa o fluxo de login, consulta,
  seleção, geração, edição, salvamento explícito e reabertura sem cadastro adicional.
- **SC-002**: Em 100% dos cenários de autorização com as duas contas de demonstração, um
  professor não lê nem altera rascunhos pertencentes ao outro.
- **SC-003**: Em 100% das falhas simuladas de geração, timeout de 60 segundos ou salvamento,
  nenhum plano parcial ou conteúdo não confirmado substitui os dados persistidos; os dados do
  pedido de geração são preservados após falha de geração.
- **SC-004**: Para cada critério de consulta suportado, 100% dos resultados exibidos satisfazem
  os filtros ativos; uma busca no catálogo de demonstração retorna resultados ou estado vazio
  em até 2 segundos.
- **SC-005**: O estado de preparação fica visível em até 1 segundo após a confirmação do pedido.
- **SC-006**: Os fluxos de consulta, edição e salvamento podem ser concluídos em viewports de
  1280 × 800 px (desktop), 768 × 1024 px (tablet) e 390 × 844 px (celular), sem perda de
  acesso aos controles principais nem rolagem horizontal da página.
- **SC-007**: Em 100% dos pedidos enviados à IA, os dados limitam-se aos campos atuais da
  solicitação e não incluem identidade do professor nem conteúdo de outros planos.
- **SC-008**: Os escopos explicitamente excluídos (cadastro público, administração, PDF,
  finalização, versionamento e publicação pública) não aparecem como ações disponíveis.
- **SC-009**: Em 100% dos testes de edição concorrente, uma alteração desatualizada é recusada
  sem sobrescrever o conteúdo salvo mais recentemente e apresenta aviso ao professor.

## Assumptions

- As duas contas de demonstração e suas credenciais são fornecidas apenas em ambiente de
  demonstração/desenvolvimento e não representam um fluxo de cadastro de usuários reais.
- O método de entrada de demonstração será usuário ou e-mail e senha; credenciais específicas
  serão definidas fora desta specification e não serão registradas em arquivos versionados.
- O catálogo inicial será pequeno, de consulta, e conterá registros suficientes para exercitar
  todos os filtros e a seleção de múltiplas habilidades; a cobertura curricular completa não
  está implícita.
- Ano e eixo podem não se aplicar a todos os níveis ou habilidades e, nesses casos, a busca
  respeita a estrutura curricular disponível.
- Uma resposta válida do serviço de IA contém Markdown não vazio, um título e uma ou mais
  atividades identificáveis; conteúdo gerado permanece sujeito à revisão do professor.
- A sessão permanece ativa enquanto o navegador estiver aberto, mesmo sem atividade, e encerra
  no logout ou quando o navegador é fechado; um novo acesso exige novo login.
- A aplicação usa o Design System e o arquivo de referência do Figma já indicados para este
  projeto.
- Conectividade com o serviço de IA é necessária para geração; uma falha de conexão ou ausência
  de resposta por 60 segundos segue o mesmo fluxo de falha sem perda dos campos.
- Somente os campos da solicitação atual são contexto para a geração; o perfil do professor e
  os planos anteriores não são enviados.
