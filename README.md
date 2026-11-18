# homeops

Repositório da organização [Tech-Preta](https://github.com/Tech-Preta) com documentos de comunidade, políticas e automações do GitHub. A branch padrão é `main`.

Nenhum arquivo descreve um produto, um ambiente doméstico ou um serviço para operar. O que está versionado são políticas, modelos de issue e pull request, configuração de ferramentas e workflows. Não há código de aplicação, `go.mod`, `package.json`, `Dockerfile`, `Makefile`, chart Helm, manifesto Kubernetes nem script de instalação, execução ou deploy.

A versão anotada em [`CHANGELOG.md`](CHANGELOG.md) é `0.0.0`, datada de 2025-03-25. O texto dessa entrada é o placeholder `Esse é apenas um exemplo de descrição.` O arquivo declara o formato [Mantenha um Changelog](https://keepachangelog.com/pt-BR/1.1.0/) e [Versionamento Semântico](https://semver.org/lang/pt-BR/). Não há tags Git no repositório.

## Estrutura do repositório

```text
.
├── CHANGELOG.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── README.md
├── SECURITY.md
├── .coderabbit.yaml
├── .editorconfig
├── .gitignore
├── .pre-commit-config.yaml
└── .github/
    ├── CODEOWNERS
    ├── FUNDING.yml
    ├── dependabot.yml
    ├── ISSUE_TEMPLATE/
    │   └── BUG_REPORT.md
    ├──  PULL_REQUEST_TEMPLATE/
    │   └── PULL_REQUEST_TEMPLATE.md
    └── workflows/
        ├── dependabot.yml
        ├── dependency-review.yml
        ├── issue.yml
        └── trivy.yml
```

O diretório do modelo de pull request se chama ` PULL_REQUEST_TEMPLATE`, com um espaço no início do nome. O arquivo é [`.github/ PULL_REQUEST_TEMPLATE/PULL_REQUEST_TEMPLATE.md`](<.github/ PULL_REQUEST_TEMPLATE/PULL_REQUEST_TEMPLATE.md>). Nenhum arquivo diz se o GitHub carrega esse modelo sozinho.

## Pré-requisitos

Para clonar e ler os arquivos basta o Git. Nenhum arquivo exige runtime de linguagem, gerenciador de pacotes ou ferramenta de deploy.

| Uso | O que está declarado |
| --- | --- |
| Hooks locais | [`.pre-commit-config.yaml`](.pre-commit-config.yaml) aponta para `gitleaks` `v8.16.3` e `pre-commit-hooks` `v4.4.0`. A versão do executável `pre-commit` não está no repositório. |
| GitHub Actions | Os workflows usam `ubuntu-latest`. A imagem do runner não está fixada. |
| Segredo `USER_TOKEN` | Referenciado em [`.github/workflows/issue.yml`](.github/workflows/issue.yml) e [`.github/workflows/trivy.yml`](.github/workflows/trivy.yml). O escopo desse segredo não está descrito. |

## Como usar

### Clonar

```sh
git clone https://github.com/Tech-Preta/homeops
cd homeops
```

### Instalação, execução e deploy

Não há comando de instalação, execução ou deploy. Nenhum workflow publica imagem, pacote, página ou ambiente. Não há `workflow_dispatch` nos workflows versionados.

### Hooks locais

[`.pre-commit-config.yaml`](.pre-commit-config.yaml) define estes hooks:

| Repositório | Revisão | Hooks |
| --- | --- | --- |
| `https://github.com/gitleaks/gitleaks` | `v8.16.3` | `gitleaks` |
| `https://github.com/pre-commit/pre-commit-hooks` | `v4.4.0` | `end-of-file-fixer`, `trailing-whitespace` |

Não há `.gitleaks.toml` nem script de instalação. Com o executável `pre-commit` já disponível, a configuração deste arquivo é acionada pelos comandos da própria ferramenta:

```sh
pre-commit install
pre-commit run --all-files
```

Esses dois comandos não estão scriptados no repositório, e a versão do executável não está fixada.

### Automação

Os workflows em [`.github/workflows/`](.github/workflows/) disparam pelos eventos descritos em [Fluxos de CI](#fluxos-de-ci). O Dependabot lê [`.github/dependabot.yml`](.github/dependabot.yml).

## Configuração

### Segredos

Nenhum arquivo contém valor de segredo. [`.gitignore`](.gitignore) ignora `.env`. Não há `.env.example` nem outra lista de variáveis de aplicação.

| Nome | Arquivo | Uso no arquivo |
| --- | --- | --- |
| `USER_TOKEN` | `.github/workflows/issue.yml` | Valor da variável `GITHUB_TOKEN` entregue à action `derekprior/add-autoresponse` |
| `USER_TOKEN` | `.github/workflows/trivy.yml` | Entrada `github-pat` da action `aquasecurity/trivy-action` |

O repositório não descreve como criar esse token nem quais permissões ele precisa ter.

### Dependabot

[`.github/dependabot.yml`](.github/dependabot.yml) é a configuração do Dependabot:

- `version: 2`
- ecossistema `github-actions`
- diretório `/`
- intervalo `daily`

Não há outro `package-ecosystem`. Não há manifesto de dependência de aplicação para o Dependabot acompanhar.

[`.github/workflows/dependabot.yml`](.github/workflows/dependabot.yml) repete o mesmo ecossistema, diretório e intervalo, com os comentários `Para GitHub Actions` e `Localização do workflow YAML`. Esse arquivo não declara `on` nem `jobs`. A mensagem que o GitHub Actions emite ao encontrar esse YAML em `.github/workflows/` não está nos arquivos.

### Editor, ignore e revisão automática

[`.editorconfig`](.editorconfig) define `root = true` e, para todos os arquivos: `charset = utf-8`, `end_of_line = lf`, `indent_size = 4`, `indent_style = space`, `insert_final_newline = true`, `max_line_length = 120`, `tab_width = 4`, `trim_trailing_whitespace = true`. Exceções no próprio arquivo:

- `*.md`: `trim_trailing_whitespace = false` e `max_line_length = off`
- `*.go`, `*.go2`, `go.mod`, `go.sum`: `indent_style = tab`
- `*.yaml` e `*.yml`: `indent_size = 2`

Não há arquivos Go na árvore.

[`.gitignore`](.gitignore) ignora artefatos de IntelliJ IDEA (`.idea`, `*.iws`, `*.iml`, `*.ipr`, `out/`), binários e saídas associadas a Go (`*.exe`, `*.exe~`, `*.dll`, `*.so`, `*.dylib`, `*.test`, `*.out`, `go.work`), Visual Studio Code (`.vscode/*`, `.history`, `.history/`, `.ionide`, `*.vsix`, `golangci-lint.xml`), `*.DS_Store`, `.env` e `**/__debug_bin*`. A linha `vendor/` está comentada, então esse diretório não é ignorado. Não há código Go, projeto IntelliJ nem extensão VS Code versionados.

[`.coderabbit.yaml`](.coderabbit.yaml) configura o CodeRabbit com `language: pt-BR`, `tone_instructions` vazio, `early_access: true` e `enable_free_tier: true`. Em `reviews`: perfil `chill`, `request_changes_workflow: true`, resumo de alto nível ligado, `abort_on_close: true`, `auto_review` ligado com revisão incremental, `drafts: false` e listas vazias de filtros, labels e branches base. As ferramentas com `enabled: true` são shellcheck, ruff, markdownlint, github-checks (`timeout_ms: 90000`), languagetool (nível `default`, categorias `TYPOS`, `TYPOGRAPHY` e `CASING` desligadas, `enabled_only: false`), biome, hadolint, swiftlint, phpstan, golangci-lint, yamllint, gitleaks, checkov, detekt, eslint, rubocop e buf. `chat.auto_reply` é `true`. Em `knowledge_base`, `opt_out` é `false`, `learnings` e `issues` usam `scope: auto`, e as chaves de Jira e Linear estão vazias.

Checkov e gitleaks aparecem aí como ferramentas de revisão do CodeRabbit. Não há workflow do Checkov em `.github/workflows/`.

### Financiamento

[`.github/FUNDING.yml`](.github/FUNDING.yml) declara as plataformas `github`, `ko_fi` e `buy_me_a_coffee` com o usuário `nataliagranato`, e uma chave Pix. A chave está só nesse arquivo.

### Proprietários

[`.github/CODEOWNERS`](.github/CODEOWNERS) atribui todos os caminhos (`*`) a `@nataliagranato`.

## Fluxos de CI

### Dependency Review

Arquivo: [`.github/workflows/dependency-review.yml`](.github/workflows/dependency-review.yml)

- Nome do workflow: `Dependency Review`
- Disparo: `pull_request`
- Permissão: `contents: read`
- Runner: `ubuntu-latest`
- Passos:
  1. `step-security/harden-runner` no commit `bf7454d06d71f1098171f2acdf0cd4708d7b5920` (o comentário do arquivo indica v2.20.0), com `egress-policy: audit`
  2. `actions/checkout` no commit `9c091bb21b7c1c1d1991bb908d89e4e9dddfe3e0` (comentário: v7.0.0)
  3. `actions/dependency-review-action` no commit `a1d282b36b6f3519aa1f3fc636f609c47dddb294` (comentário: v5.0.0)

O comentário no início do arquivo diz que o check bloqueia o merge de dependências vulneráveis quando a execução está marcada como obrigatória. Nenhum arquivo deste repositório marca o check como obrigatório.

### SBOM com Trivy

Arquivo: [`.github/workflows/trivy.yml`](.github/workflows/trivy.yml)

- Nome do workflow: `Generate SBOM`
- Disparo: `push` na branch `main`
- Permissão: `contents: write`
- Runner: `ubuntu-latest`
- Passos:
  1. `actions/checkout@v7` (tag, sem commit fixado no arquivo)
  2. `aquasecurity/trivy-action@v0.36.0`, com `scan-type: fs`, `format: github`, `output: dependency-results.sbom.json`, `image-ref: .` e `github-pat` igual a `secrets.USER_TOKEN`

O job varre o sistema de arquivos do repositório e envia o resultado no formato de SBOM (lista de materiais de software) esperado pelo Dependency Graph. `dependency-results.sbom.json` é o caminho de saída do job; esse arquivo não está commitado. Este workflow não usa `harden-runner`. `checkout` e `trivy-action` usam tag, sem SHA no arquivo.

### Resposta automática a issues

Arquivo: [`.github/workflows/issue.yml`](.github/workflows/issue.yml)

- Nome do workflow: `Resposta Automática a Issues`
- Disparo: `issues`, tipo `opened`
- Permissões: `contents: read` e `issues: write`
- Runner: `ubuntu-latest`
- Passos:
  1. `step-security/harden-runner` no mesmo commit do Dependency Review, com `egress-policy: audit`
  2. `derekprior/add-autoresponse` no commit `500f8788d667c31d43a4469f8a3ec3b491470fd1` (o comentário do arquivo diz `master`)

A action recebe `respondableId` igual a `github.event.issue.node_id`, `author` igual ao login de quem abriu a issue, e `GITHUB_TOKEN` igual a `secrets.USER_TOKEN`. O texto configurado agradece a abertura da issue e, para bug, pergunta sobre o produto ou solicitação de funcionalidade, pede e-mail para `contato@nataliagranato.xyz`.

### Workflows que não estão mais na árvore

O commit `3313ede` (pull request #69) removeu `.github/workflows/osv-scanner.yml` e `.github/workflows/scorecard.yml`, e removeu a linha `Scorecard Supply-Chain Security` de [`SECURITY.md`](SECURITY.md). Esses workflows não fazem parte da árvore atual.

## Segurança

[`SECURITY.md`](SECURITY.md) é a política versionada:

- Versão com suporte declarado: `0.0.0`.
- Vulnerabilidade deve ser enviada por e-mail para `contato@nataliagranato.xyz`, sem issue pública.
- O arquivo pede descrição, passos para reproduzir, versão afetada e possível impacto.
- Prazos escritos no arquivo: confirmação de recebimento em até 48 horas e avaliação inicial em até 7 dias.
- Práticas citadas no arquivo: code review obrigatório para todas as alterações, atualizações automáticas via Dependabot e análise de código com CodeQL.
- Ferramentas citadas: GitHub Advanced Security, Dependabot, CodeQL Analysis, Trivy e Checkov.

O que os workflows atuais cobrem é Dependabot, Dependency Review e Trivy. CodeQL Analysis e Checkov continuam nomeados em `SECURITY.md`. Checkov só aparece como ferramenta ligada em `.coderabbit.yaml`. Não há arquivo de workflow do CodeQL nem do Checkov. Não há arquivo de ruleset nem de branch protection; a frase sobre code review obrigatório não é acompanhada de configuração versionada.

## Como contribuir

O passo a passo escrito está em [`CONTRIBUTING.md`](CONTRIBUTING.md):

1. Fazer fork.
2. Clonar o fork. O comando publicado nesse arquivo é `git clone https://github.com/Tech-Preta/repository_sample` e o diretório seguinte é `repository_sample`. O clone deste repositório é `https://github.com/Tech-Preta/homeops`.
3. Criar uma branch com `git checkout -b minha-feature`.
4. Alterar os arquivos.
5. O mesmo arquivo pede testes cobrindo a mudança. Não há suíte de testes, diretório de teste nem comando de teste no repositório.
6. Commit, `git push origin minha-feature` e pull request contra a branch `main`.

O modelo de pull request pede título, descrição, motivação, dependências, tipo de mudança e um checklist. Os tipos listados são bugfix, nova funcionalidade, mudança de funcionalidade existente, documentação, segurança de código, Kubernetes, Helm, Docker, GitHub Actions e outro. Kubernetes, Helm e Docker aparecem só como opções desse modelo e do modelo de bug. Não há manifestos correspondentes na árvore.

O modelo de bug está em [`.github/ISSUE_TEMPLATE/BUG_REPORT.md`](.github/ISSUE_TEMPLATE/BUG_REPORT.md). O título sugerido é `[BUG]`, a label é `bug` e o assignee é `nataliagranato`. O formulário pede descrição, capturas de tela, ambiente (Kubernetes com manifestos, Kubernetes com Helm ou Docker localmente, sistema operacional, navegador e versão da aplicação), logs e issues relacionadas.

[`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md) adapta o [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct.html) versão 2.1. Relatos de conduta vão para `contato@nataliagranato.xyz`.

Mudanças notáveis, no formato já citado, entram em [`CHANGELOG.md`](CHANGELOG.md).

## Licença

O arquivo [`LICENSE`](LICENSE) é o texto da GNU General Public License, versão 3, de 29 de junho de 2007, da Free Software Foundation. Não há, nesse arquivo, aviso de copyright adicional nomeando uma pessoa ou a organização dona deste repositório.

## Contato

- Código: `@nataliagranato`, em [`.github/CODEOWNERS`](.github/CODEOWNERS). A mesma conta é assignada no modelo de bug.
- Segurança, conduta e a resposta automática de issues: `contato@nataliagranato.xyz`.
- Financiamento: plataformas e chave Pix em [`.github/FUNDING.yml`](.github/FUNDING.yml).

## Pendências

Itens citados pelos arquivos, ou sugeridos pelo nome do repositório, que não dá para fechar só com o que está versionado:

- Não há descrição de produto para o nome `homeops`.
- [`CONTRIBUTING.md`](CONTRIBUTING.md) ainda clona `Tech-Preta/repository_sample`. O vínculo deste repositório com um template do GitHub não está escrito em arquivo.
- O escopo e a forma de criar o segredo `USER_TOKEN` não estão documentados.
- `SECURITY.md` cita GitHub Advanced Security, CodeQL e code review obrigatório sem configuração correspondente na árvore. Checkov está só no CodeRabbit.
- Não está escrito no repositório se o modelo de pull request, no diretório com espaço no nome, é carregado pelo GitHub.
- A versão do executável `pre-commit` não está fixada.
