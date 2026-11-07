# Entrega — tela de indicadores

07/11/2026. Projeto recebido em ZIP, sem histórico Git. Alterações locais, sem commit, push, criação de branch ou Docker Compose. Apenas dados sintéticos.

## O que foi implementado

Rota /equipe/indicadores para GESTOR e ADMIN, menu restrito e redirecionamento de ATENDENTE antes da consulta. Filtros de período e fila, parâmetros de/ate/fila na URL, validação local e aplicação explícita de datas personalizadas. Cartões com explicações, taxas nulas como travessão, mediana/P90 e amostra; fila atual com riscos em cor e texto. Gráfico CSS e tabela expansível acessível; estados de carregamento, erro, consulta vazia e Retry-After. Sem polling ou persistência no navegador.

A navegação de retorno restaura as datas do formulário. As consultas usam o HTTP e o ciclo de sessão existentes, com AbortSignal. B1/B2 permanecem cobertos pelos testes existentes; não foi criada uma bateria completa de cenários de troca de identidade específica desta tela.

Arquivos: a lista exata está em arquivos-alterados.txt. Foram criados Indicadores.tsx, indicadores.css, periodo-indicadores.ts e respectivos testes. Foram alterados Rotas.tsx, EquipeLayout.tsx, Fila.test.tsx, schema.d.ts (pela ferramenta), as três documentações autorizadas e somente a linha de indicadores de PLAN.md, além da normalização de fim de arquivo.

## Verificações e limitações

- Node 24.14.0: compatível com engines do projeto. npm ci concluído após autorização de rede e uso de cache no workspace.
- Lint e TypeScript: passaram.
- Build: passou usando npm run build -- --configLoader native. O comando sem a opção falha no carregador padrão do esbuild por acesso negado ao diretório ancestral no ambiente Windows restrito.
- Testes: suíte completa final passou com 484 testes em 18 arquivos, incluindo axe, controle de acesso, filtros e histórico.
- Axe: passou na tela com tabela aberta. O jsdom imprime avisos de getContext de canvas; não são falhas de testes nem comprovam contraste real.
- API: npm run api:generate -- --check sem URL falhou porque o Python .venv do backend não está disponível. A ferramenta regenerou os tipos a partir do OpenAPI salvo no ZIP; a conferência com URL file: passou. Isso não compara o contrato com uma API em execução.
- Git: status/log e diff normal indisponíveis por ausência de .git. Conferência equivalente de whitespace por git diff --no-index --check contra a extração original: sem erros.
- Visual: navegador Edge headless com API interceptada e dados sintéticos, 1280/1024/360 px. Capturas entregues; nenhuma rolagem horizontal da página, storage local e de sessão vazios. Não foi um E2E com backend real.
- Original: pacote JS principal 511,30 kB (gzip 158,60). Alterado: 502,26 kB (gzip 156,67). Carregamento sob demanda também aplicado ao detalhe existente da equipe para compensar o custo inicial de lazy/Suspense. O aviso acima de 500 kB permanece, assim como dois avisos conhecidos de comentários do Zod.

## Divergências e decisões

- Não se confirmou branch ou atualização da main a partir do ZIP.
- Filtro por unidade não foi inventado: FilaEquipeSaida não retorna unidade_id.
- Últimas 4/8 semanas significam 28/56 dias inclusivos até hoje.
- Personalizado exige início anterior ao fim, conforme o prompt; o backend aceita um único dia. Para Este mês no dia 1, a tela admite esse único dia conforme o contrato, para manter o preset funcional.
- Confirmadas são agrupadas pela data da proposta; espera usa primeiras confirmações no período. A tela explica as âncoras diferentes.
- O contrato salvo reorganiza os tipos gerados em relação ao arquivo recebido; schema.d.ts foi regenerado pela ferramenta, sem editar DTOs manualmente.
- A cobertura nova não esgota todos os cenários pedidos: liberação do botão ao vencer Retry-After, resposta antiga durante logout, vazio e manutenção dos cartões durante atraso merecem testes específicos adicionais. Não foi declarada revisão externa.

## E2E manual para João

1. No ambiente preparado por João, usar somente o seed sintético de 8 semanas e HTTPS. João entra com gestor.cisam e suas credenciais, sem transmiti-las no chat.
2. Abrir Indicadores pelo menu, confirmar padrão de 4 semanas e comparar os números com a resposta da API. Conferir explicações, nulos e os três riscos.
3. Escolher 8 semanas, Este mês, uma fila e Todas. Na aba Rede, conferir uma consulta de indicadores por mudança e ausência de polling. Uma renovação de sessão pode repetir a requisição autenticada, conforme o transporte existente.
4. Personalizado: digitar não consulta; Aplicar consulta; datas inválidas/futuras ou mais de 366 dias não consultam. Usar Voltar/Avançar e conferir URL, formulário e dados.
5. Conferir barras e tabela, teclado, foco, leitor de tela e 1280/1024/360 px.
6. Simular falhas 403/404/422 e timeout 503 num ambiente de teste. Conferir mensagem neutra, erro junto ao período e Retry-After; nova tentativa não deve liberar antes do prazo.
7. Testar acesso direto como atendente: redireciona à Fila sem consultar indicadores. Como paciente, não acessa a área da equipe.
8. Atrasar uma consulta, sair e entrar com outro usuário sintético; confirmar que nenhum dado ou erro tardio da sessão anterior altera a nova sessão. Testar expiração/renovação e troca obrigatória de senha.
9. Confirmar auditoria CONSULTAR_INDICADORES com ator e filtros, sem dados clínicos. Não publicar dados ou credenciais das sessões.

A avaliação jurídica e suas pendências estão em verificacao-lgpd-pernambuco.md. O sistema não foi certificado para tratamento de dados reais.

