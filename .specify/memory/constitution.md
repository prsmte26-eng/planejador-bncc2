<!--
Sync Impact Report
Version change: 1.0.0 → 1.1.0 (material expansion of project principles)
Modified principles: previous five principles consolidated into the eight principles below
Added principles:
  Specification before implementation; Separation of frontend, API and external integrations;
  Teacher authentication and per-owner authorization; Teacher-reviewed AI drafts;
  Validated inputs and external responses with atomic failure behavior;
  Reproducible persistence; Figma-aligned accessible responsive interface;
  Critical behavior tests and credential-safe versioning
Added sections: none
Removed sections: none
Deferred TODO: confirm original ratification date
-->

# Constituição do Planejador BNCC

## Core Principles

### I. Especificação antes da implementação
Comportamento esperado e critérios de aceitação MUST ser registrados e revisados antes de
alterar código. Cada implementação MUST poder ser verificada contra esses critérios.

### II. Separação de responsabilidades e segredos no backend
Frontend, API e integrações externas MUST permanecer em camadas separadas, com contratos
explícitos. Credenciais e segredos de integrações MUST permanecer no backend e MUST NOT ser
expostos ao frontend, ao repositório ou aos logs.

### III. Autenticação e autorização por proprietário
O sistema MUST autenticar o professor antes de permitir acesso a planos privados e MUST
autorizar cada leitura, alteração ou exclusão verificando que o plano pertence ao usuário
autenticado. Identificadores fornecidos pelo cliente não substituem essa verificação.

### IV. IA como rascunho revisável
Toda saída da IA MUST ser identificada como rascunho editável. O professor MUST poder revisar
e alterar o conteúdo antes de qualquer aprovação ou uso como plano final.

### V. Validação e falha atômica
Entradas do usuário e respostas de serviços externos MUST ser validadas antes do uso ou da
persistência. Falhas de validação, geração ou integração MUST NOT deixar planos parciais
persistidos; operações com múltiplas etapas devem ser atômicas ou compensadas.

### VI. Persistência reproduzível
Alterações de esquema MUST ser entregues por migrations versionadas. Seeds necessários ao
desenvolvimento ou à verificação MUST ser reproduzíveis e documentados; executar migrations e
seeds em ambiente limpo MUST produzir o estado esperado.

### VII. Interface acessível e responsiva conforme o Design System
Componentes e tokens MUST seguir o Design System de referência do Figma. Interfaces MUST
atender aos critérios de acessibilidade definidos pelo projeto e permanecer utilizáveis em
desktop, tablet e celular, com estados de foco, carregamento, vazio, sucesso e erro claros.
Desvios do sistema visual MUST ser justificados nos artefatos da feature.

### VIII. Verificação crítica e versionamento seguro
Comportamentos críticos MUST ter testes automatizados adequados, e a documentação MUST
registrar como executar verificações e preparar dados reproduzíveis. Artefatos de projeto
relevantes MUST ser versionados; arquivos `.env`, credenciais e segredos MUST NOT ser
versionados. O `.gitignore` MUST manter essas exclusões.

## Restrições do produto e do design

- A BNCC MUST ser tratada como referência curricular somente para leitura; planos e rascunhos
  pertencem ao fluxo de trabalho editável do docente.
- A IA MUST permanecer identificada e subordinada à revisão e decisão humanas.
- Telas e componentes MUST seguir o arquivo Figma de referência do projeto, inclusive a
  identidade cívico-educacional, os tokens documentados e a comunicação inequívoca de estados.
- Estados de carregamento, vazio, sucesso, atenção e erro MUST ser representados quando
  aplicáveis, com mensagens que expliquem a situação e a próxima ação possível.

## Fluxo de desenvolvimento

- Trabalhar na branch atualmente selecionada. Criação ou troca de branches e integração por
  merge MUST ocorrer manualmente conforme o roteiro da aula; essas ações MUST NOT ocorrer
  automaticamente nem sem instrução do usuário.
- Reutilizar o mesmo diretório de feature durante todas as fases do Spec Kit após a
  specification inicial. Se a integração exigir uma branch específica, informar essa
  exigência antes de alterar a branch ou criar outra specification.
- Cada feature MUST manter alinhados specification, plano e tarefas. Revisões MUST verificar
  critérios de aceitação, riscos relevantes e aderência a esta constituição antes da integração.
- Verificações MUST ser definidas nos artefatos da feature e executadas conforme o fluxo
  aprovado; resultados e limitações relevantes MUST ser registrados.

## Governance

Esta constituição orienta specifications, planos, tarefas, implementação e revisão do
Planejador BNCC. Alterações nesta constituição MUST explicar o motivo, atualizar a versão e
registrar a data da emenda. Mudanças incompatíveis com princípios existentes MUST explicitar
quais princípios foram revistos; uma feature MUST NOT introduzi-las silenciosamente.

A versão segue SemVer: MAJOR para remoção ou redefinição incompatível de princípios; MINOR para
novos princípios ou expansão material da governança; PATCH para esclarecimentos e correções sem
mudança semântica. Revisores MUST conferir aderência aos princípios, aos critérios de
aceitação e aos requisitos de acessibilidade, privacidade e controle docente aplicáveis.

**Version**: 1.1.0 | **Ratified**: TODO(RATIFICATION_DATE): confirmar data original | **Last Amended**: 2026-10-01
