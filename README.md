# MedAgenda · Plano de Testes e Validação

Repositório do grupo para o projeto da disciplina **Teste de Software 2026.2** (Ciências da Computação, UNIFACISA), com o **Prof. Bruno Rafael Araújo Vasconcelos**.

Atuamos como a equipe de qualidade contratada pela **Clínica MedAgenda** para validar o sistema de agendamento e atendimento médico antes do uso com pacientes reais. Aqui ficam o plano de testes, os casos de teste, a suíte automatizada, os relatórios de execução e os defeitos encontrados.

> **Regra do projeto:** o sistema é testado **como foi entregue**. Nenhum arquivo dentro de `medagenda/` é alterado. Todo o trabalho do grupo fica em `tests/`, `docs/` e `.github/`.

## Equipe

| Integrante                             | Matrícula  | Papel principal                                                            |
| -------------------------------------- | ---------- | -------------------------------------------------------------------------- |
| Marcleidson José Fernandes de Oliveira | 2313080114 | Plano, riscos, defeitos e parecer                                          |
| Severino Ferreira da Silva Neto        | 2313080112 | Testes de API, matriz de permissões e PDFs; BDD (Cucumber)                 |
| Caio Victor Bezerra de Medeiros Souto  | 2313080109 | Testes E2E (Playwright) e acessibilidade; desempenho (k6) e pipeline de CI |

## Links

| Item                                        | Link                                                                                                                |
| ------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Documento da Primeira Entrega (v1)          | [Abrir documento](https://docs.google.com/document/d/1gPe3ZZ1SKLHZNjUpKB9JfoLutsSivFdgUMcqqpHd0t8/edit?usp=sharing) |
| Documento da Segunda Entrega (versão final) | [link, a partir de 11/12/2026]                                                                                      |
| Vídeo explicativo (até 5 min)               | [link, Segunda Entrega]                                                                                             |
| Issues (relatórios de defeito)              | aba **Issues** deste repositório                                                                                    |

## Entregas

| Entrega          | Data           | Conteúdo                                                                                                                                                                                                | Status       |
| ---------------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| Primeira Entrega | **14/11/2026** | Plano de testes (ISO/IEC/IEEE 29119-3): contexto, escopo, matriz de riscos, abordagem, critérios, ambiente, cronograma, gestão de defeitos, diagramas iniciais e este README                            | Em andamento |
| Segunda Entrega  | **11/12/2026** | Casos de teste (50+), cenários Gherkin, sessões exploratórias, matriz de rastreabilidade, suíte automatizada no CI, resultados, relatórios de defeito, relatório final com parecer de validação e vídeo | Planejada    |

As duas entregas são feitas **somente pelo link do documento** no Canvas (opção "URL do site"), com acesso liberado para `bruno.vasconcelos@maisunifacisa.com.br`

## Estrutura

```
.
├── README.md                  este arquivo
├── medagenda/                 sistema sob teste, exatamente como foi entregue (não alterar)
│   ├── docker-compose.yml
│   ├── api/                   Node.js 20 + Express + Mongoose
│   └── web/                   React 18 + Vite
├── tests/
│   ├── api/                   Jest + Supertest: regras de negócio, permissões e PDFs (pdf-parse)
│   ├── e2e/                   Playwright com Page Objects (5+ fluxos)
│   ├── bdd/                   Cucumber.js com cenários Gherkin em português
│   ├── carga/                 k6 com os limites do RNF03
│   ├── a11y/                  axe-core nas telas do RNF04
│   └── utils/                 login por perfil, datas relativas, seed
├── docs/
│   ├── plano-de-testes.md     resumo do plano (o documento oficial é o link do Canvas)
│   ├── casos-de-teste/        casos CT-xxx no modelo da seção 7.1
│   ├── exploratorios/         charters e anotações das sessões
│   └── diagramas/             arquitetura, casos de uso, modelo de dados, estados da consulta
└── .github/
    ├── workflows/testes.yml   pipeline: sobe o sistema, roda o seed e executa a suíte
    └── ISSUE_TEMPLATE/        modelo de relatório de defeito (seção 7.2)
```

## Pré-requisitos

- Docker Desktop (Windows ou macOS) ou Docker Engine com Compose (Linux)
- Node.js 20 ou superior (para rodar a suíte de testes)
- Chrome ou Firefox atualizados
- k6 instalado localmente, só para o teste de desempenho
- Pelo menos 8 GB de RAM e 10 GB livres em disco

## Como subir o MedAgenda

```bash
cd medagenda
docker compose up --build -d
```

| Serviço       | Endereço                            |
| ------------- | ----------------------------------- |
| Interface web | http://localhost:8080               |
| API REST      | http://localhost:3000/api           |
| Saúde da API  | http://localhost:3000/api/saude     |
| MongoDB       | mongodb://localhost:27017/medagenda |

Antes de **cada rodada de testes**, volte os dados ao estado inicial:

```bash
docker compose exec api npm run seed
```

Comandos úteis: `docker compose ps`, `docker compose logs -f api`, `docker compose down` (para) e `docker compose down -v` (para e apaga o banco).

## Contas de teste

| Perfil                 | E-mail                      | Senha           | Uso nos testes                             |
| ---------------------- | --------------------------- | --------------- | ------------------------------------------ |
| Administrador          | admin@medagenda.dev         | Admin@2026      | contas, relatórios, financeiro             |
| Médica (Clínica Geral) | helena.costa@medagenda.dev  | Medico@2026     | seg a sex, 08h às 12h e 14h às 18h, 30 min |
| Médico (Cardiologia)   | ricardo.alves@medagenda.dev | Medico@2026     | ter e qui, 08h às 17h, 40 min              |
| Médica (Pediatria)     | marina.duarte@medagenda.dev | Medico@2026     | seg, qua e sex, 13h às 18h, 20 min         |
| Secretária             | joana.lima@medagenda.dev    | Secretaria@2026 | recepção                                   |
| Secretário             | pedro.santos@medagenda.dev  | Secretaria@2026 | conta desativada (RN04)                    |
| Paciente               | ana.souza@email.dev         | Paciente@2026   | 3 consultas futuras (RN09)                 |
| Paciente               | carlos.lima@email.dev       | Paciente@2026   | particular, histórico                      |
| Paciente               | fernanda.rocha@email.dev    | Paciente@2026   | convênio, falta registrada                 |
| Paciente               | luiza.costa@email.dev       | Paciente@2026   | particular, alergia                        |

Somente dados fictícios. Nunca cadastrar dados reais de pessoas.

## Como executar os testes

Os comandos abaixo são o padrão que a suíte vai seguir. Cada pasta tem o próprio `package.json`.

```bash
# API, permissões e PDFs
npm ci --prefix tests/api && npm test --prefix tests/api

# E2E e acessibilidade (Chromium e Firefox)
npm ci --prefix tests/e2e && npx --prefix tests/e2e playwright test

# BDD
npm ci --prefix tests/bdd && npm test --prefix tests/bdd

# Desempenho (50 usuários, p95 < 800 ms, erros < 1%)
k6 run tests/carga/horarios-e-agenda.js
```

Regras da suíte:

- cada teste prepara os próprios dados e não depende da ordem de execução;
- datas são calculadas no próprio teste (ex.: "a próxima terça-feira"), nunca fixas no código;
- o fuso dos testes é `America/Sao_Paulo` (`TZ=America/Sao_Paulo`);
- o seed roda antes de cada execução da suíte.

## Pipeline (GitHub Actions)

A cada push, o workflow `.github/workflows/testes.yml`:

1. sobe o sistema com `docker compose -f medagenda/docker-compose.yml up --build -d`;
2. aguarda `http://localhost:3000/api/saude` responder;
3. roda o seed;
4. executa API, BDD, E2E, acessibilidade e desempenho;
5. guarda os relatórios (HTML, JUnit, k6) como artefatos da execução.

## Convenções

| Item              | Padrão                                                                           | Exemplo                                                          |
| ----------------- | -------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Caso de teste     | `CT-xxx` com requisito e técnica                                                 | CT-021 · RN07 · partição de equivalência                         |
| Defeito           | issue `DEF-xxx · o quê + onde + quando`                                          | DEF-004 · Paciente cancela consulta a 13h do horário pelo portal |
| Labels de defeito | `severidade:critica/alta/media/baixa`, `prioridade:alta/media/baixa`, `req:RNxx` | `severidade:alta` `req:RN12`                                     |
| Branches          | `tipo/descricao`                                                                 | `testes/api-agendamento`                                         |
| Commits           | verbo no presente + escopo                                                       | `adiciona casos de valor limite da RN12`                         |

Toda falha de execução vira uma issue no modelo da seção 7.2 do enunciado, com passos, resultado esperado (citando a regra), resultado obtido, frequência e evidência.

## Cronograma

| Período       | Atividades                                                                                                          | Marco                       |
| ------------- | ------------------------------------------------------------------------------------------------------------------- | --------------------------- |
| 09/11 a 13/11 | Ambiente, repositório, README, plano de testes, riscos e diagramas                                                  | **14/11: Primeira Entrega** |
| 16/11 a 20/11 | Projeto dos casos, cenários Gherkin, matriz de rastreabilidade, base da automação e do CI                           | Casos revisados             |
| 23/11 a 27/11 | Rodada 1 de execução (interface e API, todos os perfis), sessões exploratórias, registro de defeitos, testes de API | Rodada 1 concluída          |
| 30/11 a 04/12 | Fluxos E2E, PDFs, BDD, k6, axe, pipeline completo, rodada 2 e retestes                                              | Suíte verde no CI           |
| 07/12 a 10/12 | Resultados, relatório final, parecer de validação, vídeo e apresentação                                             | Congelamento em 10/12       |
| 11/12         | Cópia do documento completada e novo link enviado no Canvas                                                         | **11/12: Segunda Entrega**  |

## Referências

- Enunciado: _Projeto Plano de Testes MedAgenda_ (modelo de entrega)
- _Guia de como rodar o projeto MedAgenda_
- _Instruções de Entrega do Projeto Final_
- [ISO/IEC/IEEE 29119-3: documentação de teste](https://www.iso.org/standard/79429.html)
- [Jest](https://jestjs.io/docs/getting-started) · [Supertest](https://github.com/ladjs/supertest)
- [Playwright](https://playwright.dev/docs/intro) · [Page Object Models](https://playwright.dev/docs/pom) · [Testes de acessibilidade](https://playwright.dev/docs/accessibility-testing)
- [Cucumber.js](https://github.com/cucumber/cucumber-js) · [Referência Gherkin](https://cucumber.io/docs/gherkin/reference/)
- [k6](https://grafana.com/docs/k6/latest/) · [Thresholds](https://grafana.com/docs/k6/latest/using-k6/thresholds/)
- [axe-core](https://github.com/dequelabs/axe-core) · [WCAG 2.1](https://www.w3.org/TR/WCAG21/)
- [GitHub Actions](https://docs.github.com/en/actions) · [Docker Compose](https://docs.docker.com/compose/)
