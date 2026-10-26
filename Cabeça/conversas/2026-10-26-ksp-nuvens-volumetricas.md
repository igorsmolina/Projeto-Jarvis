# KSP — nuvens volumétricas e ajustes de mods

- **Data:** 2026-10-26
- **Projeto:** [[projetos/ksp-mods]]
- **Pasta do jogo:** `C:\Program Files (x86)\Steam\steamapps\common\Kerbal Space Program`

Itens "não verificado" não foram confirmados dentro do jogo.

## Pedido
Saber se o Volumetric Clouds V3 funciona em outros planetas com atmosfera (Duna, Eve, planetas de mod com dunas). Primeiro só análise, sem alterar.

Achados: a mod é `GameData/StockVolumetricClouds` (nuvens raymarched para o EVE), configurada por corpo em `StockVolumetricClouds/Clouds/clouds.cfg`. Já cobre Kerbin (6 camadas), Duna (3), Eve (2), Jool e Laythe, e auroras em Cind, Blas e Jot. Para novo planeta: copiar um bloco `OBJECT` e trocar `body = ...`. O `BoulderCo/Atmosphere/clouds.cfg` também define nuvens para esses corpos, com nomes de camada iguais.

## Planetas com atmosfera (configs do Kopernicus)
- Kerbin (~1 atm), Eve (~5 atm), Duna (~0,07 atm, dunas), Laythe (~0,6 atm, oceano).
- Blas (QuackPack): super-Kerbin, raio 811 km, ~1,6 atm, 788 K, dunas globais.
- Cind (QuackPack): rochoso denso com lava, raio 571 km, ~0,19 atm.
- Tekto (OPM): lua de Sarnus estilo Titã, raio 280 km, ~1,2 atm, 94 K.
- Thatmo (OPM): lua de Neidon, raio 286 km, ~0,01 atm.
- Gigantes gasosos ignorados a pedido: Jool, Sarnus, Urlum, Neidon, Jot.
- Sem atmosfera: Moho, Gilly, Mun, Minmus, Ike, Dres, Vall, Tylo, Bop, Pol, Eeloo, Plock, Nissee, Karen, Hale, Ovok, Slate, Polta, Priax, Tal, Wal, Iszmi, Phybi, Geet, Subon.
- O AtmosphericBeats traz camadas para Venus, Mars e Titan, que não existem aqui.

## Diagnóstico (ModuleManager, Scatterer, EVE)
Verificado no cache (`ModuleManager.ConfigCache`) e no `KSP.log`:
- O AtmosphericBeats apagava as camadas do Volumetric Clouds em Duna, Eve, Jool e Laythe: Duna, Eve e Laythe sem nuvens; Kerbin usava as camadas `Earth-*` dele.
- 26 camadas `EVE_CLOUDS` sem `body` (auroras de Cind/Blas/Jot via `%OBJECT`) causavam NullReferenceException em `CloudsManager.PreprocessConfigs`.
- QuackPack tinha nuvens antigas em Blas (`Blas-Duststorms`) e Cind (`Cind-MainClouds`).
- Nenhum patch altera atmosfera; o Scatterer já cobre todos os planetas do plano.
- Plano: `docs/superpowers/plans/2026-10-24-nuvens-volumetricas-planetas.md`.

## O que foi criado / alterado
**A) `GameData/VolumetricCloudsExtra/` (patches do ModuleManager)**
- `00_Limpeza.cfg`: remove camadas sem body e as de Mars/Venus/Titan.
- `01_Resgate_SVC.cfg`: devolve camadas do Volumetric Clouds em Duna/Eve/Laythe (troca temporária de body antes/depois do AtmosphericBeats) e particle fields `dust-Duna` e `snow-Laythe`.
- `02_Duna_Visivel.cfg`: densidade da poeira de Duna de 0,001 para 0,02.
- `03_Duna_NuvensKerbin.cfg`: `Duna-clouds1/2` (cópias das de Kerbin, rosadas).
- `04_Blas.cfg`: `Blas-clouds1/2` (cópias de Kerbin, areia, altitude +2000 m); remove `Blas-Duststorms`.
- `05_Cind.cfg`: `Cind-sulfur-clouds` (cópia de `Eve-clouds2`, amarelada); remove `Cind-MainClouds`.
- `06_Tekto.cfg`: `Tekto-clouds1/2` (cópias de Kerbin, laranja).
- `07_Thatmo.cfg`: `Thatmo-haze` (opcional, névoa alta).
- As nuvens estilo Kerbin dependem das camadas `Earth-clouds1/2` do AtmosphericBeats. Cind e Thatmo têm atmosfera bem menos densa (~8% e ~4% da de Kerbin).

**B) `tools/check_clouds.sh`:** lista camadas `EVE_CLOUDS` do cache e erros do EVE/Scatterer. Testes: `tools/baseline.txt`, `final.txt`, `now.txt`, `now2.txt`, `now3.txt`.

**C) `GameData/LocalFixes/`**
- `Blas_RimRamp.cfg`: troca `BlasRimColorRamp3.dds` (inexistente) por `BlasRimColorRamp.dds`.
- `ProtonTanks_NoDoubleFuelSwitch.cfg`: remove seletor de combustível duplicado do CryoTanks nos tanques do Proton v2.
- `UA120_InlineWaterfall.cfg`: remove efeito Waterfall inexistente do `bluedog_UA120` base.

**D) `GameData/TweakScaleCompanion/FS`:** TweakScale Companion para Firespitter, `TweakScaleCompanion.FS-1.3.0.2.zip` (52.100 bytes), de github.com/TweakScale/Companion_FS (release de 11/07/2024), SHA-256 `6a2e9503c8724e9111363e6054b9ae86e44099a59da0eb7bfa79776371ded544`. Instalado fora do CKAN.

**E) Saves (editados a pedido, sem backup)**
- `saves/better i do` (Ciência): 26 planetas de `isResearched=False/researchState=0` para `True/100` (a mod ResearchBodies esconde corpos não pesquisados).
- `saves/fds` (Sandbox): todos os 39 corpos marcados como pesquisados.
- Cópia do `better i do` original ficou em `/tmp/_unused` (11.200.378 bytes).

## Erros do jogo e o que foi feito
- Kopernicus "Missing Textures" (Blas): corrigido (`Blas_RimRamp.cfg`).
- Kopernicus "Terrain Detail": só aviso, não é erro.
- B9PartSwitch, tanques Proton v2: corrigido (conflito Tantares x CryoTanks).
- B9PartSwitch, `bluedog.UA120` subtipo Inline: corrigido (patch do RSMP aplicava efeito inexistente).
- TweakScale "Houston": faltava o companion do Firespitter; instalado. Não verificado se o popup sumiu.
- Não resolvidos: B9PartSwitch na tinta do Titan do Bluedog (só no log); ~1.900 linhas de erro inofensivas no `KSP.log` (texturas, shaders, efeitos); DangIt (módulos inexistentes); SpaceDust.
- 3 NullReferenceException do EVE (`CloudsManager.PreprocessConfigs`) ainda no log antes das nuvens carregarem; nuvens carregam normalmente. Causa não confirmada.

## Tempo de carregamento
~7 min por abertura: ~3,5 min do ModuleManager refazendo o cache quando algum arquivo de mod muda, ~4 min de compilação de peças e texturas. Sem mudanças, o cache é reutilizado.

## Nuvens em Duna e Eve
- Camadas de Duna carregadas e ligadas ao Scatterer, mas a poeira tinha densidade 0,001; subida para 0,02 (`02_Duna_Visivel.cfg`).
- Eve de órbita (lilás/esbranquiçado, borda roxa) é o esperado com as 2 camadas densas (2,7 km e 12,7 km); a borda roxa vem do Scatterer.

## Nuvens estilo Kerbin em Blas, Tekto e Duna
Critério: atmosfera com ≥ ~60% da densidade de Kerbin (P*M/T): Blas ~87%, Tekto ~377%. Duna (~7%) entrou por pedido explícito. Cores por planeta.
Erro corrigido: dois comandos na mesma linha em `04_Blas.cfg` impediam a criação do cache.
Verificado no cache: `Blas-clouds1` em 3.700–5.500 m e `clouds2` em 8.100–8.800 m, cores aplicadas. Não verificado: o visual no jogo.

## Investigação do motor AE-2B S.C.I.M.I.T.A.R. (nave eve glider V2)
A nave perdia potência e não chegava à órbita.
- Motor (Near Future Aeronautics), entradas de ar, atmosfera de Kerbin e a nave no save: nada alterado.
- Com as 3 pastas desativadas (`_desativado_teste`): motores normais. Com LocalFixes e TweakScale ativos: normal. Com VolumetricCloudsExtra devolvida e a mesma trajetória: normal.
- Conclusão: era a trajetória do primeiro teste. O fluxo de ar das entradas depende da velocidade na direção da entrada e da densidade; em subida muito inclinada, o ar captado cai.

## Perguntas rápidas
- Desligar 2 motores no espaço economiza combustível? Sim, só gasta com motor ligado e com empuxo.
- O DV muda ao desligar motores? Não; é estimativa por combustível e motores do estágio.

## Pendências
- **Nuvens pretas à noite em Duna** (nenhuma alteração feita). Em Kerbin também escurecem, menos visível. Hipótese: as cópias usam cor 0-255 (ex.: 160,120,100) e as originais de Kerbin usam decimais (0.55,0.55,0.55); as camadas originais de Duna têm `skylightMultiplier` = 0 / 0,45 e as cópias não definem. Proposta: converter as cores das cópias para decimal mantendo o tom. O usuário ainda não respondeu se aplica.
- Visual das nuvens e popup do TweakScale: não verificados.

## Como desfazer
- Nuvens: apagar `GameData/VolumetricCloudsExtra` (ou um arquivo específico).
- Correções de avisos: apagar `GameData/LocalFixes`.
- Companion do Firespitter: apagar `GameData/TweakScaleCompanion/FS`.
- Sobras apagáveis: `_desativado_teste/` (2 pastas vazias), `tools/`.
- `/tmp/_unused` guarda o estado original dos planetas no save.
- Se o AtmosphericBeats for removido: as camadas `Earth-*` somem, Blas/Tekto/Duna perdem as nuvens estilo Kerbin e o BoulderCo pode reaparecer sobreposto (limpar também `01_Resgate_SVC.cfg`).
