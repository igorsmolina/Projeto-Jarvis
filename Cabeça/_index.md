# Índice do vault

## Projetos ativos
| Projeto | Status | Nota |
|---|---|---|
| Segundo Cérebro (Jarvis) | Estrutura criada, falta plugin Git | [[projetos/segundo-cerebro]] |
| rtsvibe (jogo de estratégia) | Em desenvolvimento, revisar commits do Roxo | [[projetos/rtsvibe]] |
| KSP — nuvens e mods | Nuvens ativas, falta decidir correção de Duna à noite | [[projetos/ksp-mods]] |

## Onde fica cada coisa
- `projetos/` — uma nota por projeto: repo, URL, stack, status, próximo passo.
- `conversas/` — histórico detalhado de conversas, linkado pela nota do projeto. Só ler quando pedido.
- `_modelos/` — modelos de nota (`projeto.md`, `conversa.md`).
- `design/tokens.md` — cores, fontes, raio de borda, espaçamento.
- `decisoes/` — data, contexto, decisão, motivo.
- `reunioes/` — data, participantes, decisões, próximos passos.
- `inbox/` — ideias soltas, revisadas semanalmente.
- `CLAUDE.md` — instruções para o Claude.

## Novo projeto
1. Criar `projetos/nome.md` e inserir o modelo `projeto` (Ctrl+P → "Inserir modelo").
2. Preencher repo/local, stack, status e próximo passo.
3. Adicionar uma linha na tabela acima.

## Nova conversa a guardar
1. Criar `conversas/AAAA-MM-DD-assunto.md` com o modelo `conversa`.
2. Linkar na seção "Links" da nota do projeto e atualizar status e próximo passo.
