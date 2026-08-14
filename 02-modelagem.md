# Parte 2 — Modelagem e Linguagem de Projeto

Os diagramas abaixo usam a notação **UML**, representados em **Mermaid** — o GitHub renderiza esses blocos automaticamente na visualização do `.md`, sem necessidade de imagens externas.

## 2.1 Diagrama de Casos de Uso

Representa os três atores do sistema (Paciente, Médico, Administrador) e suas interações principais, rastreáveis aos requisitos funcionais da Parte 1.

```mermaid
flowchart LR
    subgraph Atores
        A1((Paciente))
        A2((Médico))
        A3((Administrador))
    end

    subgraph MedConecta["Sistema MedConecta"]
        UC1([Cadastrar-se / Autenticar - RF01])
        UC2([Agendar Consulta - RF02])
        UC3([Cancelar ou Remarcar Consulta - RF02])
        UC4([Realizar Teleconsulta - RF04])
        UC5([Consultar Prontuário - RF03])
        UC6([Emitir Receita ou Atestado - RF05])
        UC7([Responder Triagem Pré-consulta - RF07])
        UC8([Gerenciar Agenda e Especialidades - RF08])
        UC9([Gerar Relatórios - RF08])
        UC10([Verificar Cobertura de Convênio - RF09])
    end

    A1 --> UC1
    A1 --> UC2
    A1 --> UC3
    A1 --> UC4
    A1 --> UC5
    A1 --> UC7
    A1 --> UC10

    A2 --> UC1
    A2 --> UC4
    A2 --> UC5
    A2 --> UC6

    A3 --> UC8
    A3 --> UC9
```

## 2.2 Diagrama de Classes

Modelo conceitual das principais entidades do domínio e seus relacionamentos.

```mermaid
classDiagram
    class Usuario {
        <<abstract>>
        +UUID id
        +String nome
        +String email
        +String senhaHash
        +Date criadoEm
        +autenticar()
        +atualizarPerfil()
    }
    class Paciente {
        +String cpf
        +String convenio
        +agendarConsulta()
        +visualizarProntuario()
    }
    class Medico {
        +String crm
        +String especialidade
        +iniciarTeleconsulta()
        +emitirReceita()
    }
    class Administrador {
        +gerenciarUsuarios()
        +gerarRelatorio()
    }
    class Consulta {
        +UUID id
        +Date dataHora
        +String status
        +String linkVideochamada
        +confirmar()
        +cancelar()
    }
    class Prontuario {
        +UUID id
        +adicionarRegistro()
    }
    class RegistroClinico {
        +Date data
        +String descricao
        +String tipo
    }
    class Receita {
        +UUID id
        +String medicamento
        +String assinaturaDigital
        +Date validade
    }
    class Notificacao {
        +String canal
        +String mensagem
        +enviar()
    }

    Usuario <|-- Paciente
    Usuario <|-- Medico
    Usuario <|-- Administrador
    Paciente "1" --> "0..*" Consulta : agenda
    Medico "1" --> "0..*" Consulta : atende
    Paciente "1" --> "1" Prontuario : possui
    Prontuario "1" --> "0..*" RegistroClinico : contém
    Consulta "1" --> "0..1" Receita : gera
    Consulta "1" --> "0..*" Notificacao : dispara
```

## 2.3 Diagrama de Sequência — fluxo de teleconsulta

Complementa os diagramas estáticos mostrando a ordem temporal das interações entre paciente, médico e os serviços do sistema, do agendamento até a emissão da receita.

```mermaid
sequenceDiagram
    actor P as Paciente
    participant App as App / Web
    participant API as API Gateway
    participant Agend as Serviço de Agendamento
    participant Video as Serviço de Videochamada
    participant Pront as Serviço de Prontuário
    actor M as Médico

    P->>App: Seleciona especialidade e horário
    App->>API: POST /consultas
    API->>Agend: Cria consulta
    Agend-->>API: Consulta confirmada
    API-->>App: Retorna confirmação
    App-->>P: Envia notificação de confirmação

    Note over P,M: No horário agendado
    P->>App: Entra na sala de consulta
    M->>App: Entra na sala de consulta
    App->>Video: Solicita sessão de vídeo
    Video-->>App: Sessão estabelecida (WebRTC)

    M->>Pront: Consulta histórico do paciente
    Pront-->>M: Retorna registros anteriores
    M->>Pront: Adiciona novo registro clínico
    M->>App: Emite receita digital
    App-->>P: Disponibiliza receita e atestado
```

## 2.4 Rastreabilidade

Cada caso de uso do diagrama 2.1 corresponde a um ou mais requisitos funcionais listados em `docs/01-requisitos.md`, e cada classe do diagrama 2.2 é persistida conforme os requisitos não funcionais de confiabilidade (histórico imutável) e segurança (criptografia de campos sensíveis) descritos na Parte 3.
