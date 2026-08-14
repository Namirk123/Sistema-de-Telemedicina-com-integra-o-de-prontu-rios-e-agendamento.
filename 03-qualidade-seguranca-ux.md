# Parte 3 — Qualidade, UX e Segurança

## 3.1 Integridade de dados

O prontuário eletrônico é o ativo mais sensível do sistema, então sua integridade é garantida em três camadas:

- **Modelo append-only** — nenhum registro clínico (`RegistroClinico`) pode ser editado ou apagado após salvo; correções são feitas por meio de um novo registro que referencia o anterior, preservando o histórico completo para fins clínicos e legais (semelhante a uma trilha de auditoria contábil).
- **Transações ACID** — operações críticas (criação de consulta, emissão de receita, gravação de registro clínico) são executadas em transações de banco relacional, evitando estados inconsistentes (ex.: consulta marcada sem horário reservado).
- **Validação de schema** — toda entrada da API é validada contra um esquema (tipos, formatos, campos obrigatórios) antes de chegar ao banco, reduzindo dados corrompidos ou incompletos.
- **Assinatura digital em receitas e atestados** — cada documento emitido recebe um hash assinado digitalmente pelo médico responsável, permitindo verificar posteriormente que o conteúdo não foi alterado.

## 3.2 Segurança da informação

| Prática | Aplicação no MedConecta |
|---|---|
| Criptografia em trânsito | TLS 1.3 em toda comunicação cliente-servidor e nas chamadas de vídeo (DTLS-SRTP, padrão do WebRTC). |
| Criptografia em repouso | Campos sensíveis do prontuário e dados pessoais criptografados com AES-256 no banco de dados. |
| Controle de acesso (RBAC) | Cada papel (paciente, médico, administrador) só enxerga os recursos permitidos; um médico só acessa o prontuário de pacientes com quem tem vínculo de atendimento. |
| Autenticação multifator (MFA) | Obrigatória para médicos e administradores, que têm acesso a dados de múltiplos pacientes. |
| Consentimento e LGPD | Coleta de consentimento explícito do paciente antes do tratamento de dados de saúde (dado sensível conforme art. 5º e 11 da LGPD), com opção de revogação. |
| Minimização e anonimização | Dados usados para relatórios administrativos ou treinamento do módulo de IA são anonimizados/agregados, sem expor identidade do paciente. |
| Proteção contra ataques | Rate limiting e bloqueio progressivo contra tentativas de força bruta no login; WAF na borda da API. |
| Auditoria | Log de todo acesso a prontuário (quem acessou, quando, o quê), retido para investigação de incidentes. |

## 3.3 Interface do usuário (wireframes de baixa fidelidade)

Os rascunhos abaixo priorizam **poucos cliques**, **hierarquia visual clara** e **linguagem simples**, considerando que parte relevante do público de telemedicina tem baixa familiaridade com tecnologia (RNF de usabilidade, item 1.3).

### Tela de Login

<img src="../wireframes/01-login.svg" width="600" alt="Wireframe da tela de login" />

Login único para todos os perfis, com seletor de "tipo de usuário" no canto superior — evita ter três telas de entrada diferentes e reduz a chance do paciente errar o link de acesso.

### Dashboard do Paciente

<img src="../wireframes/02-dashboard-paciente.svg" width="600" alt="Wireframe do dashboard do paciente" />

A ação mais frequente (agendar consulta) fica como botão de destaque logo no topo. A próxima consulta marcada aparece em card fixo, com botão "Entrar" que só fica ativo perto do horário — evita que o paciente precise procurar o link da videochamada em e-mail ou WhatsApp.

### Agendamento (fluxo em etapas)

<img src="../wireframes/03-agendamento.svg" width="600" alt="Wireframe da tela de agendamento" />

O agendamento é dividido em 4 passos curtos (especialidade → médico → horário → confirmação), com indicador de progresso visível. A especialidade sugerida pela triagem (RF07) já vem pré-destacada, reduzindo decisão e erro de encaminhamento.

### Prontuário Eletrônico — visão do médico

<img src="../wireframes/04-prontuario-medico.svg" width="600" alt="Wireframe do prontuario na visao do medico" />

Histórico apresentado em linha do tempo, do mais recente ao mais antigo, para que o médico entenda o contexto do paciente em segundos antes ou durante a consulta. A adição de novo registro é sempre um formulário novo (nunca edição de um antigo), reforçando visualmente a regra de integridade da seção 3.1.

### Sala de Teleconsulta

<img src="../wireframes/05-sala-teleconsulta.svg" width="600" alt="Wireframe da sala de teleconsulta" />

Layout inspirado em ferramentas de videochamada já conhecidas do público (vídeo em destaque, controles na parte inferior), reduzindo a curva de aprendizado. Um painel lateral com dados essenciais do paciente (alergias, última consulta) fica visível ao médico durante toda a chamada, sem precisar trocar de tela.

## 3.4 Justificativa das escolhas de usabilidade

- **Consistência de padrões conhecidos**: controles de videochamada seguem convenções já usadas por outros aplicativos de vídeo, reduzindo o tempo de aprendizado.
- **Redução de carga cognitiva**: fluxos longos (como agendamento) são quebrados em etapas pequenas com indicador de progresso, em vez de um formulário único extenso.
- **Feedback constante**: status de consulta (confirmada, cancelada, em andamento) sempre visível, evitando dúvida do paciente sobre o que já foi concluído.
- **Acessibilidade**: contraste adequado, textos com tamanho mínimo legível e área de toque ampla nos botões, alinhados à WCAG 2.1 AA citada nos requisitos não funcionais.
