# AutoMaster

> Sistema de gerenciamento para oficinas mecânicas — documentação de projeto de software

[![Versão](https://img.shields.io/badge/versão-1.0-blue.svg)](PDF/Trabalho%20Final-%20Gabriel%20Lacerda.pdf)
[![Disciplina](https://img.shields.io/badge/disciplina-Projeto%20de%20Software-green.svg)](#)
[![Autor](https://img.shields.io/badge/autor-Gabriel%20Lacerda%20Lemos%20da%20Silva-orange.svg)](#autor)

---

## Sumário

- [Sobre o Projeto](#sobre-o-projeto)
- [Estrutura do Repositório](#estrutura-do-repositório)
- [Atores e Casos de Uso](#atores-e-casos-de-uso)
- [Documentação dos Diagramas](#documentação-dos-diagramas)
- [Tecnologias Previstas](#tecnologias-previstas)
- [Documentação Completa (PDF)](#documentação-completa-pdf)
- [Autor](#autor)

---

## Sobre o Projeto

O **AutoMaster** auxilia o gerenciamento de oficinas mecânicas, organizando o fluxo desde a entrada do veículo até sua entrega ao cliente. O sistema controla clientes, veículos, ordens de serviço, peças, pagamentos e funcionários.

Este repositório contém a documentação do projeto de software da disciplina **Projeto de Software**, incluindo diagramas UML e modelo de dados.

---

## Estrutura do Repositório

```
trabalho-final-automaster/
├── Diagramas/          # Diagramas UML e modelo de dados
├── PDF/                # Documento oficial do trabalho
└── README.md
```

---

## Atores e Casos de Uso

| Ator | Responsabilidade |
|------|------------------|
| **Cliente** | Solicita serviços para seu veículo |
| **Atendente** | Cadastra clientes/veículos, abre OS e registra pagamentos |
| **Mecânico** | Realiza diagnósticos, serviços e finaliza OS |
| **Administrador** | Gerencia o sistema e emite relatórios |

| ID | Caso de Uso | Ator |
|----|-------------|------|
| UC-01 | Cadastrar Cliente | Atendente |
| UC-02 | Cadastrar Veículo | Atendente |
| UC-03 | Abrir Ordem de Serviço | Atendente |
| UC-04 | Registrar Diagnóstico | Mecânico |
| UC-05 | Registrar Serviços Executados | Mecânico |
| UC-06 | Registrar Peças Utilizadas | Mecânico |
| UC-07 | Finalizar Ordem de Serviço | Mecânico |
| UC-08 | Registrar Pagamento | Atendente |
| UC-09 | Emitir Relatórios | Administrador |

---

## Documentação dos Diagramas

### Requisitos

#### Diagrama de Casos de Uso

<p align="center">
  <img src="Diagramas/Caso%20de%20Uso%201.0.png" alt="Diagrama de Casos de Uso" width="700"/>
</p>

#### Contratos de Operação

| Operação | UC | Pré-condições | Pós-condições |
|----------|----|---------------|---------------|
| `abrirOrdemServico()` | UC-03 | Cliente e veículo cadastrados | OS criada com status **Aberta** |
| `registrarDiagnostico()` | UC-04 | OS existente | Diagnóstico registrado na OS |
| `finalizarOrdemServico()` | UC-07 | Serviços e peças registrados | OS finalizada |

#### Sequência do Sistema — UC-03

<p align="center">
  <img src="Diagramas/Servico.png" alt="UC-03 — Abrir Ordem de Serviço" width="600"/>
</p>

Atendente chama `cadastrarVeiculo()` e, em seguida, `abrirOrdemServico()`.

#### Sequência do Sistema — UC-04

<p align="center">
  <img src="Diagramas/Registrar%20diagnostico.png" alt="UC-04 — Registrar Diagnóstico" width="600"/>
</p>

Mecânico envia `registrarDiagnostico(descricao)` e recebe confirmação.

#### Sequência do Sistema — UC-07

<p align="center">
  <img src="Diagramas/Finalizar%20servico.png" alt="UC-07 — Finalizar Ordem de Serviço" width="600"/>
</p>

Mecânico chama `finalizarOrdemServico()` e recebe `ordem finalizada`.

---

### Arquitetura

#### Arquitetura em 3 Camadas

<p align="center">
  <img src="Diagramas/Arquitetura.png" alt="Arquitetura do Sistema" width="700"/>
</p>

| Camada | Componentes |
|--------|---------------|
| **Apresentação** | Telas do Atendente, Mecânico, Cliente e Relatórios |
| **Negócio** | ClienteService, VeiculoService, OrdemServicoService, ServicoService, PecaService, PagamentoService, RelatorioService |
| **Persistência** | AutoMasterDB (Cliente, Veículo, OrdemServico, Serviço, Peça, Pagamento, Funcionário) |

#### Diagrama de Componentes

<p align="center">
  <img src="Diagramas/Componentes.png" alt="Diagrama de Componentes" width="600"/>
</p>

Telas da apresentação consomem os serviços de negócio, que acessam o **AutoMasterDB**.

#### Diagrama de Implantação

<p align="center">
  <img src="Diagramas/Diagrama%20Implanta%C3%A7%C3%A3o.png" alt="Diagrama de Implantação" width="500"/>
</p>

`Navegador Web` → `Sistema AutoMaster` → `PostgreSQL`

---

### Projeto

#### Diagrama de Classes

<p align="center">
  <img src="Diagramas/Diagrama%20Classe%201.0.png" alt="Diagrama de Classes" width="700"/>
</p>

Entidades principais: **Cliente**, **Veículo**, **OrdemServico**, **Diagnóstico**, **Serviço**, **Peça** e **Pagamento**. Funcionários herdam de **Funcionario** (Mecânico, Atendente, Administrador).

#### Sequência de Projeto — Abrir OS

<p align="center">
  <img src="Diagramas/Diagrama%20Sequencia.png" alt="Diagrama de Sequência de Projeto" width="600"/>
</p>

| # | Mensagem |
|---|----------|
| 1 | Atendente → Tela: `solicitarAberturaOS()` |
| 2 | Tela → Controller: `abrirOrdemServico(dados)` |
| 3 | Controller → Entity: `criar()` |
| 4–6 | Persistência e confirmação no banco |
| 7–8 | Retorno à tela e `exibirSucesso()` |

#### Diagrama de Comunicação

<p align="center">
  <img src="Diagramas/Diagrama%20Comunicacao.png" alt="Diagrama de Comunicação" width="500"/>
</p>

`Atendente` → `TelaOS` → `Controller` → `Banco` → confirmação → `ordem criada`.

#### Diagrama de Estados da OS

<p align="center">
  <img src="Diagramas/Diagrama%20Estado.png" alt="Diagrama de Estados" width="400"/>
</p>

| Estado | Evento de transição | Próximo estado |
|--------|---------------------|----------------|
| Aberta | `iniciarDiagnostico` | EmDiagnostico |
| EmDiagnostico | `diagnosticoConcluido` | AguardandoAprovacao |
| AguardandoAprovacao | `aprovadoCliente` / `rejeitadoCliente` | EmServico / Cancelada |
| EmServico | `servicoConcluido` | Finalizada |
| Finalizada | `retirarVeiculo` | Entregue |

---

### Modelo de Dados

#### Diagrama Entidade-Relacionamento

<p align="center">
  <img src="Diagramas/Entidade%20Relacionamento.png" alt="Diagrama Entidade-Relacionamento" width="700"/>
</p>

9 entidades, com tabelas de junção **OrdemServicoServico** e **OrdemServicoPeca** para relacionamentos N:N.

| Relação | Cardinalidade |
|---------|---------------|
| Cliente → Veículo | 1 : N |
| Veículo → OrdemServico | 1 : N |
| OrdemServico → Diagnóstico / Pagamento | 1 : N |
| OrdemServico ↔ Serviço / Peça | N : N |

---

## Tecnologias Previstas

| Camada | Tecnologia |
|--------|------------|
| Frontend | Navegador Web |
| Backend | Sistema AutoMaster (camada de serviços) |
| Banco de Dados | PostgreSQL |
| Arquitetura | 3 camadas + MVC |

---

## Documentação Completa (PDF)

📄 **[Trabalho Final — Gabriel Lacerda.pdf](PDF/Trabalho%20Final-%20Gabriel%20Lacerda.pdf)** — versão 1.0, 7 de junho de 2026.

---

## Autor

**Gabriel Lacerda Lemos da Silva** — disciplina Projeto de Software.
