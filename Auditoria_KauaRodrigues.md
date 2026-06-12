# Auditoria de Realidade DesignOps: REACH sob a lente humana

## Introducao

Nesta auditoria eu usei o framework REACH, da Nielsen Norman Group, para comparar o que o grupo planejou em DesignOps com o que acabou acontecendo na pratica. A ideia nao e defender que o plano estava errado do inicio ao fim, porque ele tinha boas intencoes. O ponto e olhar para o projeto real e entender onde o processo ajudou, onde travou e onde ficou pesado demais para um time universitario.

O REACH divide DesignOps em cinco lentes: **Results**, **Efficiency**, **Ability**, **Clarity** e **Health**. Aqui eu foquei nas combinacoes pedidas pela atividade: eficiencia e habilidade na execucao, clareza e resultado na validacao, e saude do time olhando para a carga operacional.

---

## Parte 1 - Efficiency & Ability

O processo que eu escolhi analisar foi a validacao obrigatoria de commits dentro da esteira de CI/CD. No plano de DesignOps, esse fluxo aparecia como uma forma de garantir qualidade, rastreabilidade e controle antes de qualquer merge. Na teoria, faz sentido. O problema e que, na pratica, esse gate virou um dos maiores gargalos do projeto.

As principais evidencias que usei foram a pagina de [pipelines com falha](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/pipelines?page=1&scope=all&status=failed), a sequencia de tentativas fechadas para uma mesma correcao ([tentativa 1](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/61), [tentativa 2](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/62), [tentativa 3](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/63), [tentativa 4](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/64) e [correcao final](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/65)), alem da [correcao feita na propria validacao de commits](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/58) e de um [merge request que ainda ficou preso no job de validacao](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/72/pipelines).

Pela lente de **Efficiency**, o problema foi que o processo, que deveria reduzir risco, passou a consumir muito tempo operacional. O volume de pipelines falhando mostra que a esteira nao era so uma verificacao final; ela virou uma fonte constante de retrabalho. O caso das varias tentativas fechadas para a mesma correcao deixa isso bem claro: uma mudanca simples precisou ser repetida ate conseguir passar pelo fluxo. Em vez de acelerar a entrega com feedback objetivo, o processo empurrou o time para tentativa e erro.

Pela lente de **Ability**, eu nao vejo isso apenas como erro individual. A regra exigia que todo mundo soubesse lidar com convencao de commits, issue vinculada, GitLab CI, regra de branch e leitura de log de pipeline. Isso e normal em um time mais maduro, mas ficou pesado para estudantes com pouco tempo por dia. A prova mais forte disso e que o grupo precisou criar uma correcao especifica para arrumar a propria captura do ID de issue dentro da validacao. Ou seja, o processo que deveria controlar qualidade tambem virou uma peca tecnica que precisava de manutencao.

Minha leitura e que a falha veio de dois lugares ao mesmo tempo: falta de dominio tecnico em CI/CD e um desenho de processo um pouco corporativo demais para a realidade do grupo. No fim, o time contornou do jeito mais pragmatico possivel: fechando tentativas que nao passavam, abrindo novas, ajustando aos poucos e seguindo em frente quando a entrega precisava andar. Isso mostra que a validacao de commits tinha valor para rastreabilidade, mas ficou acima da capacidade operacional do time naquele momento. O aprendizado existiu, mas veio com bastante retrabalho.

---

## Parte 2 - Clarity & Results

Para analisar **Clarity** e **Results**, eu escolhi a validacao do scorecard e do dashboard da Sprint 3. Essa entrega tentava transformar a esteira de CI/CD em algo mais visivel: indicadores de MIL/SIL, rollback, secret scan, rastreabilidade e score geral. Era uma boa tentativa de sair do "a pipeline rodou" para algo que pudesse ser explicado para quem nao esta dentro do codigo.

As evidencias principais foram o [merge request do scorecard 100/100](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/66), o [merge request de avaliacao da Sprint 3 que foi integrado mesmo com pipeline falhando](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/59) e o proprio [documento de DesignOps](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/blob/main/docs/DESIGN_OPS.md), que previa criterios de qualidade, validacoes e checklist.

O scorecard tinha resultados tecnicos fortes no papel: MIL/SIL passando, rollback dentro do limite, rastreabilidade completa, dashboard gerado e secret scan limpo. Isso ajuda em **Results**, porque cria um jeito mais concreto de mostrar progresso. So que o ponto critico e que parte da evidencia foi descrita como execucao local, com comandos manuais como `python3 src/tests/run_mil_scipy.py` e `bash src/tests/run_sil.sh`. Quando a validacao depende de execucao local descrita no texto, ela fica menos auditavel para a parceira, para o orientador e ate para outros integrantes do grupo.

Por isso, eu vejo uma diferenca entre clareza de intencao e clareza operacional. A intencao estava certa: medir, pontuar e transformar a entrega em indicadores. Mas a operacao ainda dependia muito do proprio time declarando que rodou os testes. O plano de DesignOps prometia um processo mais objetivo, com validacoes automatizadas e criterio claro de pronto, mas a realidade ficou mais manual.

O outro ponto e que a validacao nao ficou bem conectada com experiencia de usuario ou decisao de interface. Nos artefatos que eu analisei, apareceu comentario sobre ajuste de sumario em documentacao, mas nao uma validacao externa clara sobre se o dashboard ficou mais facil de entender, se reduziu duvidas ou se ajudou a Jacto a confiar mais na entrega. Entao, a decisao nao caiu totalmente em opiniao subjetiva, porque havia dados tecnicos por tras. Mas tambem nao chegou no nivel ideal de DesignOps, em que a melhoria visual ou de fluxo e comprovada por evidencia externa.

Minha conclusao dentro da propria analise e que essa parte teve **Clarity parcial** e **Results indireto**. O grupo melhorou a organizacao das evidencias e criou uma forma mais comunicavel de mostrar a esteira. Ao mesmo tempo, faltou fechar o ciclo com dados mais rastreaveis na pipeline e com algum feedback externo registrando que aquilo realmente melhorou a experiencia de acompanhamento do projeto.

---

## Parte 3 - Health

Na parte de **Health**, eu escolhi criticar o proprio plano de DesignOps do grupo. O documento era completo e bem intencionado, mas parecia desenhado para um time com mais tempo e maturidade do que a nossa realidade permitia. O proprio plano reconhecia que o grupo tinha seis estudantes com disponibilidade media proxima de uma hora por dia, e isso muda bastante a leitura.

As evidencias principais aqui foram o [documento de DesignOps](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/blob/main/docs/DESIGN_OPS.md), a lista de [merge requests fechados](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests?state=closed), a lista de [merge requests integrados](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests?state=merged), as [pipelines com falha](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/pipelines?page=1&scope=all&status=failed) e os casos em que o fluxo precisou ser repetido ou flexibilizado para a entrega andar.

O plano previa Design Sync semanal, Prototype Review, handoff assincrono, revisao de MR/PR, Sprint Review e Sprint Retrospective. Somando os rituais fixos e recorrentes, a carga podia chegar perto de 170 minutos por semana. Para um time universitario com pouco tempo livre, isso e muito. Um processo de DesignOps deveria diminuir ansiedade e facilitar a entrega; quando ele adiciona reuniao, ferramenta e checklist demais, ele vira mais uma camada de pressao.

O checklist de PR tambem mostra esse excesso. O plano falava em tokens semanticos, testes em 1280, 1440 e 1920px, contraste WCAG AA, Storybook, build de Storybook, regressao visual via Chromatic e relatorio de acessibilidade. Esses criterios fazem sentido em um produto digital com equipe dedicada de design system. No nosso caso, boa parte do projeto era firmware, simulacao, CI/CD e documentacao tecnica sobre drone. A regra ficou maior do que a necessidade real.

Nos artefatos do GitLab, nao aparece um uso consistente de Storybook, regressao visual, lint de tokens ou Chromatic como parte real da esteira. Isso mostra uma diferenca entre o plano escrito e o processo vivido. Quando uma pratica aparece no documento mas nao aparece no fluxo, ela vira divida de processo: algo que o grupo diz que faz, mas nao consegue sustentar.

Tambem da para enxergar sinais de fadiga operacional nos MRs fechados sem merge, nas pipelines falhando e nos casos em que o gate precisou ser flexibilizado. Para mim, isso nao significa falta de compromisso do time. Significa que a burocracia ficou cara demais para o contexto. O plano tentou aplicar praticas de mercado, mas sem adaptar o suficiente para a carga mental de estudantes no meio de sprint, prova, prazo e varias disciplinas ao mesmo tempo.

Aqui tambem entra uma evidencia pessoal que eu considero importante para a lente de **Health**. Perto da entrega, eu precisei virar noites para fechar partes do projeto que ainda estavam pendentes, e isso teve impacto direto na minha saude. Como evidencia privada, tenho um atestado medico emitido no dia 11/06/2026, bem proximo da entrega da sprint. Eu nao estou usando isso para expor ninguem do grupo nem para transformar a auditoria em desabafo, mas para mostrar que o problema de Health nao ficou so no campo teorico. A sobrecarga operacional saiu do planejamento e chegou no corpo.

Essa parte, para mim, e uma das mais importantes da auditoria. Quando o DesignOps e saudavel, ele distribui melhor o trabalho, deixa o fluxo claro e evita que uma pessoa precise segurar tudo no ultimo momento. Quando ele falha, o processo ate pode parecer bonito no documento, mas a execucao real vira concentracao de responsabilidade, noites sem dormir e pressao acumulada. Foi isso que aconteceu em alguns momentos do modulo.

O ponto mais critico e que o plano media muita coisa tecnica, mas quase nao media risco humano. Ele falava de pipeline, checklist, validacao e qualidade, mas nao tinha um mecanismo simples para perceber que uma pessoa estava acumulando responsabilidade demais. Para mim, esse foi o maior erro de DesignOps: tratar saude do time como um ritual separado, e nao como um sinal operacional que deveria acender alerta dentro da sprint.

### O que eu cortaria do plano

Se eu pudesse reescrever o plano de DesignOps com a maturidade que o grupo tem agora, eu cortaria ou simplificaria estes pontos:

1. **Regressao visual automatizada com BackstopJS, Percy ou Chromatic.**  
   Eu cortaria porque nao foi implementada de verdade e tinha custo alto para pouco retorno no contexto do projeto.

2. **Storybook obrigatorio em todo PR.**  
   Eu deixaria como opcional apenas para componentes visuais relevantes. Para firmware, CI/CD e dashboards tecnicos, isso nao deveria bloquear entrega.

3. **Sprint Retrospective longa toda semana.**  
   Eu trocaria por um checkpoint assincrono com tres perguntas: o que travou, o que andou e o que precisa mudar.

4. **Questionario de Health em toda sprint.**  
   Eu manteria uma versao bem simples, talvez quinzenal ou por marco importante. Se ninguem responde, a metrica vira ritual vazio.

5. **Checklist unico para todo tipo de MR.**  
   Eu separaria por tipo de entrega: firmware, dashboard, documentacao, pipeline e UX. Um MR de CI nao deveria carregar exigencia de contraste WCAG se nao mexe em interface.

### Minha proposta mais realista

Para um time como o nosso, eu usaria um DesignOps menor e mais direto:

- Um criterio de pronto por tipo de entrega.
- Um link de evidencia obrigatorio por MR.
- Pipeline obrigatoria apenas para o que realmente consegue rodar automaticamente.
- Revisao humana focada em risco, nao em checklist gigante.
- Uma retrospectiva curta por sprint, preferencialmente assincrona.
- Um quadro simples de bloqueios, com dono e prazo.

Isso manteria o valor do DesignOps sem transformar o processo em uma segunda entrega paralela. No fim, a saude do time melhoraria justamente porque o processo deixaria de competir com a entrega principal.

---

## Matriz REACH da auditoria

| Pilar REACH | Evidencia principal | Diagnostico |
|---|---|---|
| Results | [Scorecard da Sprint 3](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/66) | Houve resultado tecnico, mas ainda indireto em relacao a experiencia da parceira. |
| Efficiency | [Pipelines falhando](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/pipelines?page=1&scope=all&status=failed) e tentativas repetidas de correcao | O fluxo de CI/CD criou gargalo e retrabalho recorrente. |
| Ability | [Correcao da validacao de commits](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/58) | O time ainda nao tinha maturidade plena para sustentar regras automatizadas complexas. |
| Clarity | [Scorecard com execucao local](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/66) e [avaliacao integrada com pipeline falhando](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/59) | A clareza existia no discurso, mas a evidencia rastreavel ficou incompleta. |
| Health | [Plano de DesignOps](https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/blob/main/docs/DESIGN_OPS.md) | O plano era mais ambicioso do que saudavel para a capacidade real do grupo. |

---

## Diagnostico de maturidade REACH

Para deixar a auditoria menos subjetiva, eu montei uma nota de maturidade de 1 a 5 para cada pilar. Nao e uma metrica oficial do artigo, mas ajuda a transformar a leitura em um diagnostico mais comparavel.

| Pilar | Nota | Leitura |
|---|---:|---|
| Results | 3/5 | O grupo conseguiu mostrar resultados tecnicos, principalmente pelo scorecard. O problema e que esses resultados ainda nao provavam melhoria real de experiencia para a parceira. |
| Efficiency | 2/5 | A esteira ajudou a controlar qualidade, mas tambem gerou muito retrabalho. Pipelines falhando e tentativas repetidas de merge mostram que o fluxo ainda era pesado. |
| Ability | 2/5 | O time aprendeu durante o processo, mas a quantidade de problemas na validacao de commits mostrou que a habilidade tecnica exigida era maior do que a habilidade disponivel no momento. |
| Clarity | 2/5 | Havia intencao de clareza, mas parte das evidencias ficou local/manual e nem toda decisao ficou registrada de forma auditavel. |
| Health | 1/5 | Foi o ponto mais fraco. O plano tinha rituais e checklists demais para a capacidade real do grupo, e a sobrecarga chegou a gerar impacto pessoal de saude. |

Essa leitura muda um pouco a conclusao geral. O grupo nao falhou por falta de vontade. Ele falhou porque tentou operar um DesignOps mais maduro do que a estrutura do time permitia. O plano parecia bom isoladamente, mas nao tinha mecanismos suficientes para detectar gargalo, redistribuir carga e simplificar processo antes que a situacao virasse retrabalho ou desgaste.

---

## Triangulacao final

O ponto central do REACH e nao olhar uma metrica sozinha. Por isso, eu cruzei tres tipos de evidencias: tecnica, operacional e humana.

| Tipo de evidencia | O que apareceu | O que isso indica |
|---|---|---|
| Tecnica | Pipelines falhando, validacao de commits quebrando e execucoes locais de teste | A automacao existia, mas ainda nao estava madura o suficiente para sustentar o fluxo sem retrabalho. |
| Operacional | MRs fechados sem merge, gates flexibilizados e checklist maior que o uso real | O processo planejado nao cabia totalmente na rotina do grupo. |
| Humana | Sobrecarga perto da entrega e atestado medico privado de 11/06/2026 | O custo do processo nao ficou so no projeto; ele atingiu a saude de quem estava segurando parte da entrega. |

Essa triangulacao e importante porque evita uma conclusao rasa. Se eu olhasse so para o plano, poderia dizer que o grupo tinha DesignOps bem estruturado. Se eu olhasse so para os MRs, poderia dizer que era apenas problema tecnico de CI. Mas quando junto pipeline, retrabalho e saude, a conclusao fica mais forte: o processo estava tentando compensar falta de maturidade com mais regra, e isso aumentou o custo humano da entrega.

---

## O que eu manteria, ajustaria e cortaria

Para ir alem da critica, eu separo o plano em tres grupos: o que vale manter, o que precisa ser ajustado e o que deveria ser cortado.

| Decisao | Acao | Motivo |
|---|---|---|
| Pipeline como gate antes do merge | Ajustar | A ideia e boa, mas o erro precisa ser mais facil de entender. Se o job falha, ele deveria explicar exatamente qual commit ou arquivo quebrou a regra. |
| Validacao de commits | Manter com simplificacao | Rastreabilidade importa, mas a regra deveria aceitar excecoes controladas ou ter uma mensagem de erro mais didatica. |
| Scorecard de sprint | Manter | Foi uma das melhores formas de transformar evidencia tecnica em linguagem compreensivel. |
| Execucao local de testes como evidencia principal | Ajustar | Pode existir como apoio, mas a evidencia principal deveria sair da pipeline. |
| Storybook obrigatorio | Cortar | Nao apareceu como ferramenta real do fluxo e nao combinava com a maior parte das entregas do projeto. |
| Regressao visual automatizada | Cortar neste contexto | A proposta era boa, mas o custo de setup era alto demais para o tempo real do grupo. |
| Retrospectiva longa toda sprint | Ajustar | Eu trocaria por retrospectiva curta e assincrona, focada em bloqueios reais. |
| Checklist unico para qualquer PR | Ajustar | O checklist deveria mudar conforme o tipo de entrega: CI, firmware, dashboard, documentacao ou UX. |

Minha proposta final seria um DesignOps bem mais simples: cada MR precisaria ter um dono, um tipo de entrega, uma evidencia principal e um risco declarado. Isso ja resolveria boa parte dos problemas sem criar uma camada de burocracia paralela.

---

## Plano de recuperacao para a proxima sprint

Se eu fosse transformar essa auditoria em acao, eu nao tentaria consertar tudo de uma vez. Eu faria um plano de recuperacao pequeno, com foco em reduzir atrito.

| Problema | Acao simples | Como eu mediria |
|---|---|---|
| Pipeline falha e ninguem entende rapido o motivo | Melhorar as mensagens de erro dos jobs principais | Menos tempo entre pipeline falhar e alguem corrigir. |
| Checklist unico para qualquer entrega | Criar checklist por tipo de MR | Menos itens irrelevantes em cada revisao. |
| Execucao local usada como evidencia principal | Fazer a pipeline gerar os artefatos principais | Mais MRs com evidencia automatica anexada. |
| Trabalho concentrado em poucas pessoas | Colocar dono e co-dono em cada entrega | Menos tarefas sem responsavel claro perto do prazo. |
| Sobrecarga aparece tarde demais | Criar um check-in assincrono de carga toda semana | Bloqueios e excesso de trabalho aparecendo antes da vespera. |

Para mim, o ponto principal seria trocar a pergunta "o processo foi seguido?" por "o processo ajudou a entregar sem quebrar ninguem?". Essa mudanca parece pequena, mas muda o DesignOps inteiro. O foco deixa de ser cumprir ritual e passa a ser remover atrito real.

---

## Linha de base para uma proxima sprint

Se o grupo fosse continuar o projeto, eu usaria esta auditoria como linha de base. Em vez de tentar medir tudo, eu escolheria poucos sinais:

| Sinal | Como medir | Por que importa |
|---|---|---|
| Taxa de pipelines verdes na primeira tentativa | Comparar pipelines verdes vs falhos por sprint | Mede se a esteira esta ajudando ou travando. |
| Tempo medio entre abrir MR e mergear | Olhar historico dos MRs | Mostra gargalo real de integracao. |
| Quantidade de MRs fechados sem merge | Contar por sprint | Indica retrabalho e perda de energia. |
| Evidencia automatica por MR | Ver se o MR tem artefato da pipeline | Mede clareza e auditabilidade. |
| Bloqueios repetidos | Agrupar falhas por tipo | Ajuda a decidir onde simplificar o processo. |

Essas metricas sao mais simples do que o plano original, mas seriam mais honestas para a realidade do grupo. Elas tambem seguem a ideia de triangulacao do REACH: nenhuma metrica sozinha prova tudo, mas varias apontando para o mesmo lado mostram se o processo esta melhorando ou piorando.

---

## Limites da auditoria

Um limite importante desta auditoria e que nem tudo que aconteceu no grupo ficou registrado no GitLab. Algumas decisoes, combinados e momentos de sobrecarga aconteceram fora do repositorio, em conversa, mensagem ou na rotina individual de cada integrante. Por isso, eu tratei o GitLab como fonte principal para evidencias tecnicas e usei a evidencia pessoal de saude apenas na parte de **Health**, sem publicar o documento medico no repositorio por ser um dado sensivel.

Isso tambem mostra uma falha de DesignOps: se o processo dependia de alinhamentos fora do fluxo oficial, mas esses alinhamentos nao viravam registro, o grupo perdia rastreabilidade. No fim, a ausencia de algumas evidencias tambem e uma evidencia. Ela mostra que o plano prometia clareza, mas a execucao real ainda deixava muita coisa importante fora dos artefatos do projeto.

---

## Fechamento

Olhando para o modulo inteiro, eu nao acho que o DesignOps do grupo foi inutil. Ele ajudou a dar linguagem, organizacao e alguma rastreabilidade para as entregas. O problema foi outro: o plano tentou nascer maduro demais.

Na pratica, o grupo precisava de um DesignOps mais leve, mais proximo da rotina real e menos dependente de ferramentas que ninguem teria tempo de configurar direito. A melhor versao desse processo nao seria a que tem mais rituais ou mais checks, mas a que consegue responder tres perguntas sem sobrecarregar ninguem:

- O que foi feito?
- Qual evidencia prova isso?
- O que ainda esta bloqueando a entrega?

Se essas tres perguntas estivessem mais bem resolvidas desde o inicio, o grupo teria tido mais clareza, menos retrabalho e mais energia para focar no produto.

---

## Indice de evidencias

- Documento de DesignOps: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/blob/main/docs/DESIGN_OPS.md
- Pipelines falhando: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/pipelines?page=1&scope=all&status=failed
- Tentativas fechadas para a mesma correcao: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/61, https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/62, https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/63, https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/64
- Correcao final da sequencia: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/65
- Correcao da validacao de commits: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/58
- Merge request preso em validacao de commits: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/72/pipelines
- Scorecard da Sprint 3: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/66
- Avaliacao da Sprint 3 integrada com pipeline falhando: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests/59
- Merge requests fechados: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests?state=closed
- Merge requests integrados: https://git.inteli.edu.br/graduacao/2026-1b/t13/g05/-/merge_requests?state=merged
- Evidencia pessoal de Health: atestado medico privado de 11/06/2026, citado na auditoria sem publicacao do arquivo por conter dado sensivel.
