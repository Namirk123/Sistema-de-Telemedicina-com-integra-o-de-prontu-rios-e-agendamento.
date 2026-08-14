# Parte 1 — Engenharia de Requisitos

## 1.1 Contexto e dor do cenário

O acesso à saúde no Brasil é marcado por forte desigualdade geográfica: municípios pequenos e regiões rurais têm pouca ou nenhuma oferta de especialistas, e mesmo em grandes centros o agendamento presencial costuma ser lento, feito por telefone, sem visibilidade de horários e sujeito a retrabalho quando o paciente esquece exames ou histórico anterior. Além disso, o prontuário do paciente fica fragmentado entre diferentes clínicas, sem continuidade do cuidado.

O **MedConecta** ataca três dores específicas:

1. **Acesso** — pacientes distantes de centros médicos conseguem consulta por vídeo, sem deslocamento.
2. **Continuidade do cuidado** — o prontuário eletrônico único acompanha o paciente entre especialidades, evitando repetição de exames e perda de histórico.
3. **Eficiência operacional** — agendamento automatizado, lembretes e triagem reduzem faltas e mau direcionamento de pacientes para a especialidade errada.

## 1.2 Requisitos Funcionais (RF)

| ID | Requisito | Descrição |
|---|---|---|
| RF01 | Cadastro e autenticação de usuários | O sistema deve permitir cadastro e login de três perfis distintos — paciente, médico e administrador — com controle de acesso próprio para cada perfil. |
| RF02 | Agendamento de consultas | O paciente deve poder escolher especialidade, visualizar médicos disponíveis e selecionar um horário livre na agenda do profissional. |
| RF03 | Gestão de prontuário eletrônico | O sistema deve manter um prontuário único por paciente, com histórico de consultas, diagnósticos, exames e prescrições, acessível pelos médicos que o atendem. |
| RF04 | Teleconsulta por videochamada | O sistema deve disponibilizar uma sala de videochamada integrada à plataforma, sem exigir aplicativos externos, no horário agendado. |
| RF05 | Emissão de receitas e atestados digitais | O médico deve poder gerar receitas e atestados em formato digital, assinados eletronicamente, disponíveis para o paciente logo após a consulta. |
| RF06 | Notificações automáticas | O sistema deve enviar lembretes de consulta, confirmações e avisos de cancelamento/remarcação por e-mail, SMS ou WhatsApp. |
| RF07 | Triagem pré-consulta assistida | Antes da consulta, o paciente responde a um questionário guiado que sugere a especialidade adequada e sinaliza sintomas de possível urgência. |
| RF08 | Painel administrativo | O administrador deve poder gerenciar cadastro de médicos, especialidades, horários de atendimento e emitir relatórios de uso do sistema. |
| RF09 | Verificação de convênio | O paciente deve poder informar seu convênio/plano de saúde e o sistema deve indicar se a especialidade/médico escolhido está coberto. |

## 1.3 Requisitos Não Funcionais (RNF)

### Desempenho
- Tempo de resposta das requisições da API abaixo de 2 segundos em 95% dos casos (p95).
- A sala de videochamada deve operar com latência de áudio/vídeo inferior a 150 ms em condições normais de rede.
- O sistema deve suportar picos de acesso simultâneo (ex.: início do expediente médico) sem degradação perceptível, por meio de escalabilidade horizontal.

### Segurança
- Toda comunicação entre cliente e servidor deve usar TLS 1.3.
- Dados sensíveis de saúde armazenados devem ser criptografados em repouso (AES-256).
- Autenticação multifator (MFA) obrigatória para o perfil médico e administrador.
- Controle de acesso baseado em papéis (RBAC): um médico só acessa prontuários de pacientes que atendeu ou atende.
- Conformidade com a LGPD, incluindo consentimento explícito do paciente para tratamento de dados de saúde (dado sensível, art. 11 da LGPD).

### Confiabilidade
- Disponibilidade mínima de 99,5% (SLA), com monitoramento contínuo.
- Backups diários automatizados do banco de dados, com teste periódico de restauração.
- Nenhum dado de prontuário pode ser sobrescrito ou apagado — apenas complementado, garantindo trilha de auditoria completa.

### Portabilidade
- Aplicação web responsiva (desktop, tablet e mobile) e aplicativo nativo para Android e iOS.
- API RESTful documentada (OpenAPI/Swagger), permitindo integração futura com outros clientes.

### Usabilidade
- Interface aderente às diretrizes WCAG 2.1 nível AA, considerando o público idoso, um dos maiores usuários de telemedicina.
- Fluxo de agendamento completável em no máximo 4 passos.

## 1.4 Como os requisitos resolvem a dor do cenário

A tabela abaixo conecta cada dor identificada em 1.1 aos requisitos que a endereçam:

| Dor | Requisitos relacionados |
|---|---|
| Acesso limitado a especialistas | RF02, RF04, RNF de desempenho na videochamada |
| Prontuário fragmentado | RF03, RNF de confiabilidade (imutabilidade do histórico) |
| Agendamento ineficiente / faltas | RF02, RF06, RF07 |
| Confiança e privacidade dos dados de saúde | RNF de segurança e conformidade com LGPD |
| Dificuldade de uso por público não familiarizado com tecnologia | RNF de usabilidade (WCAG, fluxo curto) |

A documentação técnica foi organizada em nível de **especificação de requisitos de software (ERS)** simplificada, adequada ao escopo acadêmico do projeto: requisitos identificados por ID, priorizados implicitamente pela ordem de apresentação (RF01–RF03 são o núcleo mínimo viável) e rastreáveis até os diagramas apresentados na Parte 2.
