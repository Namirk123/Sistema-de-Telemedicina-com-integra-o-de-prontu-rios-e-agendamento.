# MedConecta — Sistema de Gestão Inteligente para Telemedicina

> Trabalho acadêmico de Engenharia de Requisitos e Arquitetura de Software.
> **Cenário escolhido:** Opção A — Sistema de Telemedicina com integração de prontuários e agendamento.

## Sobre o projeto

O **MedConecta** é um sistema de telemedicina que conecta pacientes e médicos por meio de teleconsultas, centraliza o prontuário eletrônico do paciente e automatiza o agendamento de consultas. O objetivo é reduzir barreiras geográficas e de tempo no acesso à saúde, manter o histórico clínico do paciente unificado (independente de qual médico/especialidade ele consulte) e garantir que dados sensíveis de saúde sejam tratados com o nível de segurança e conformidade exigido pela LGPD.

O termo "inteligente" no nome do sistema se refere a duas capacidades específicas do produto (detalhadas na Parte 4):

- **Sugestão inteligente de horários**, que cruza a disponibilidade do médico, a urgência relatada pelo paciente e o histórico de faltas para otimizar a agenda;
- **Triagem assistida por IA**, um questionário pré-consulta que direciona o paciente à especialidade correta e sinaliza casos potencialmente urgentes para a equipe médica.

## Estrutura do repositório

```
telemedicina-sgi/
├── README.md                          → este arquivo (visão geral)
├── docs/
│   ├── 01-requisitos.md               → Parte 1: Engenharia de Requisitos
│   ├── 02-modelagem.md                → Parte 2: Modelagem (diagramas UML)
│   ├── 03-qualidade-seguranca-ux.md   → Parte 3: Qualidade, Segurança e UX
│   └── 04-arquitetura-tecnologias.md  → Parte 4: Arquitetura e Tecnologias
└── wireframes/
    ├── 01-login.svg
    ├── 02-dashboard-paciente.svg
    ├── 03-agendamento.svg
    ├── 04-prontuario-medico.svg
    └── 05-sala-teleconsulta.svg
```

## Sumário

| Parte | Conteúdo | Link |
|---|---|---|
| 1 | Requisitos Funcionais, Não Funcionais e interpretação do problema | [docs/01-requisitos.md](docs/01-requisitos.md) |
| 2 | Diagrama de Casos de Uso, Diagrama de Classes e Diagrama de Sequência | [docs/02-modelagem.md](docs/02-modelagem.md) |
| 3 | Integridade de dados, segurança da informação e wireframes de UX | [docs/03-qualidade-seguranca-ux.md](docs/03-qualidade-seguranca-ux.md) |
| 4 | Stack tecnológica e interoperabilidade com sistemas externos | [docs/04-arquitetura-tecnologias.md](docs/04-arquitetura-tecnologias.md) |

## Como publicar este repositório no GitHub

```bash
cd telemedicina-sgi
git init
git add .
git commit -m "Documentação inicial do MedConecta"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/telemedicina-sgi.git
git push -u origin main
```

Os diagramas em `docs/` usam blocos ```mermaid```, que o GitHub renderiza automaticamente na visualização do arquivo `.md` — não é necessário nenhum plugin adicional.
