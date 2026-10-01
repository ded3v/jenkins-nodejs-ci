# Jenkins + GitHub Actions · CI para Node.js

[![CI Node.js](https://github.com/ded3v/jenkins-nodejs-ci/actions/workflows/ci.yml/badge.svg)](https://github.com/ded3v/jenkins-nodejs-ci/actions/workflows/ci.yml)

API Express usada para praticar integração contínua em dois ambientes: Jenkins em Windows e GitHub Actions em Linux. A cada execução, o código passa pela instalação de dependências, pelo script de build e por testes HTTP com Jest e Supertest.

Projeto de estudo desenvolvido durante a formação FAP DevOps — Turma 5, a partir de [carlhenriquex/pipeline-jenkins-nodejs](https://github.com/carlhenriquex/pipeline-jenkins-nodejs).

## O que está implementado

| Ambiente | Etapas | Execução |
| --- | --- | --- |
| Jenkins | Checkout → instalar dependências → build → testes | Job configurado no Jenkins |
| GitHub Actions | Checkout → configurar Node.js 20 → `npm ci` → build → testes | Push e pull request para `main` |

O script de build usa um `echo` para representar essa etapa. Ele não compila nem gera um artefato. Esta versão implementa **CI**: os pipelines não publicam a aplicação.

## Executar localmente

Requisitos: Git, Node.js e npm. O workflow usa Node.js 20.

```bash
git clone https://github.com/ded3v/jenkins-nodejs-ci.git
cd jenkins-nodejs-ci
npm ci
npm run build
npm test
npm start
```

Com o servidor iniciado, acesse [http://localhost:3000](http://localhost:3000).

| Rota | Resposta |
| --- | --- |
| `GET /` | Mensagem de funcionamento da API |
| `GET /usuarios` | Lista de usuários de exemplo |
| `GET /status` | `{"status":"online"}` |

Os dados são mantidos no código; esta API não utiliza banco de dados.

## Configurar no Jenkins

1. Prepare um agente Windows com Git, Node.js e npm disponíveis para o usuário do serviço Jenkins.
2. Crie um job do tipo Pipeline.
3. Em **Definition**, selecione **Pipeline script from SCM** e escolha Git.
4. Informe a URL deste repositório, a branch `*/main` e o caminho `Jenkinsfile`.
5. Execute **Build Now** e acompanhe o console e os stages.

O `Jenkinsfile` utiliza `bat`, portanto precisa de um agente Windows. Ao final, os blocos `post` registram sucesso ou falha. O job não inicia o servidor com `npm start`.

## Arquivos principais

| Arquivo | Responsabilidade |
| --- | --- |
| `src/app.js` | Configura a API Express e os endpoints |
| `server.js` | Inicia o servidor |
| `test/app.test.js` | Testa os endpoints com Jest e Supertest |
| `Jenkinsfile` | Define a pipeline declarativa para Windows |
| `.github/workflows/ci.yml` | Define o CI executado no GitHub |
| `package-lock.json` | Registra as versões usadas por `npm ci` |

## O que pratiquei

- Separação da aplicação e da inicialização do servidor para facilitar testes.
- Pipeline declarativa e tratamento de resultado no Jenkins.
- Instalação com lockfile no GitHub Actions.
- Diferenças entre agentes Windows e Linux.
- Validação automática de mudanças antes da integração.

## Próximos passos

Publicar relatórios de testes, adicionar verificações de segurança e definir uma estratégia de empacotamento e deploy.

## Autoria

André Chagas Assis Costa — FAP DevOps, Turma 5.
