# Wandora — propriedade do SDK e política de imagem

**Esta é uma distribuição independente**, derivada do OpenHands SDK e licenciada nos termos dos arquivos originais. A relação de fork serve para origem, atribuição e consulta; não é um contrato de atualização automática.

## Repositórios sob nosso controle

- SDK + Agent Server: `https://github.com/OARANHA/software-agent-sdk`
- Canvas: `https://github.com/OARANHA/OpenHands`
- Imagens planejadas: `ghcr.io/oaranha/wandora-agent-server` (componente) e `ghcr.io/oaranha/openhands-wandora` (produto all-in-one)

**Proibido no fluxo automático:** `git pull`/Sync Fork do OpenHands original, merges ou PRs para `OpenHands/*`, rastreamento de branch upstream em imagem, builds de commits flutuantes, publicação no namespace `ghcr.io/openhands/*`, deploy/redeploy.

**Permitido somente após avaliação explícita:** ler changelogs upstream, cherry-pick seletivo, atualizações de dependências que preservem as correções, migrações com teste e rollback. Isso requer PR interno e revisão humana.

## Base congelada

Em 2026-10-10 a `main` foi verificada em `c4b93299cf2b31c8b5d0b2c5a89965ccc5ff24f9` (Agent Server e SDK `1.54.0`). O Canvas `main` estava em `e0a26af82f0b2016423c0d53cc9ae32a357cef2d`, e `config/defaults.json` pedia Agent Server `1.54.0`.

O workflow de build herdado `.github/workflows/server.yml` foi isolado **nesta branch** para `workflow_dispatch`, com destino `ghcr.io/oaranha/wandora-agent-server`. Nenhuma publicação ou merge está provada.

A distribuição Canvas deverá fixar SHA do SDK em `config/wandora-image.json`; os dois checkouts devem estar limpos. O comando do Canvas `node scripts/wandora-build.mjs /caminho/software-agent-sdk` constrói as duas imagens somente no Docker **local**. A build final não reinstala SDK do upstream via `AGENT_SERVER_SDK_GIT_REF`.

## Critérios antes de publicar ou implantar

1. Testes Python do Agent Server e SDK; testes do Canvas; revisão das alterações para Chutes e DSML.
2. Confirmar se o `source` target do Dockerfile inclui todo o código Wandora e compatibilidade de pacotes/versionamento (não confundir SDK reinstalado com Agent Server atualizado).
3. Validar a imagem em ambiente isolado, endpoints de saúde/modelos e chamada real de ferramenta com confirmação.
4. Conferir construção para arquitetura da VPS, digest imutável, SBOM/varredura e rollback.
5. Solicitar autorização independente para publicar GHCR e outra para deploy na VPS; nunca mexer em credenciais do usuário sem necessidade.

Este arquivo orienta o fork, mas não substitui `AGENTS.md`, código, CI e evidências verificáveis.
