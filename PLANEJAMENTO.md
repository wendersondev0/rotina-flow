# Planejamento — rotina-flow

Plataforma pessoal de rotinas, em português, usável como site, aplicativo instalado e no desktop. Este arquivo é a lista de execução do começo ao fim. Cada fase só começa quando a anterior cumpre o critério de saída.

Repositório: `rotina-flow` (clone local da pasta `rotina-flow`).

Fonte das rotinas atuais: [Schedule no Notion](https://app.notion.com/p/Schedule-1cbdde39d3d680b88857de54a7e91903). Id da página: `1cbdde39-d3d6-80b8-8857-de54a7e91903`.

A assistente da plataforma se chama **Secretaria Pessoal**. Ela conversa no app e no WhatsApp. Quem decide o que entra na rotina, na meta, na lista, no evento e na recompensa é a pessoa.

## 1. O que a plataforma faz

O rotina-flow separa quatro coisas que não se misturam: a rotina que se repete, a meta sem data, a lista de metas com prazo e o evento que acontece uma vez. Em volta disso ficam ofensiva, pontuação, recompensa e a Secretaria Pessoal.

Uso do dia a dia, em um toque sempre que der:

- Ver o que fazer agora, entre a rotina de hoje e o evento de hoje, e concluir.
- Ver pontuação do período e da categoria, ao lado da pontuação máxima.
- Ver ofensiva positiva ou negativa de cada tarefa recorrente.
- Resgatar recompensa quando a condição for cumprida.
- Abrir uma tarefa e ver subtarefas, metas ligadas, anotações, histórico e lembrete.
- Falar com a Secretaria Pessoal no app ou no WhatsApp.
- Aceitar ou recusar o que ela propõe.

## 2. Decisões fechadas

Estas regras não ficam em aberto na implementação.

### Quatro registros

| Registro | O que é | Repete | Data |
| --- | --- | --- | --- |
| Tarefa | Atividade da rotina | Sim, a cada dia, semana, mês, ano ou a cada N desses | Horário dentro da recorrência. Não tem “dia único” |
| Evento | Atividade de uma vez | Não | Data e hora marcadas. Exemplo: levar a TV ao conserto amanhã ao meio-dia |
| Meta | Objetivo a cumprir | Não | Sem data. O progresso vem de submetas, de uma tarefa ou de uma ofensiva |
| Lista de metas | Conjunto de metas | Não | Prazo da lista. Exemplo: Lista de Metas para 2027 |

Tarefa nunca é gravada como evento. Evento nunca ganha recorrência. Meta nunca ganha data de início nem data de fim. O prazo fica na lista. Se a pessoa descrever algo ambíguo para a Secretaria, ela pergunta qual dos quatro é antes de propor a gravação.

### Idioma e edição

- Interface, estados, botões, vazios e falas da Secretaria em português.
- O nome visível da IA é Secretaria Pessoal.
- Nomes vindos do Notion podem permanecer em inglês até a pessoa editar.
- Categoria, foco, tarefa, subtarefa, evento, meta, submeta, lista, recompensa, pontuação, horário, recorrência, vínculo do WhatsApp e condição de recompensa são editáveis. Nada disso fica hardcoded depois do seed inicial.

### Tarefa e recorrência

Toda tarefa ativa repete. A recorrência tem unidade, intervalo e uma data âncora.

| Unidade | Intervalo | Ocorrência |
| --- | --- | --- |
| Dia | 1 | Cada dia. Se houver dias da semana marcados, só esses dias |
| Dia | N | A cada N dias, contados da âncora |
| Semana | 1 | Cada semana, de segunda a domingo, no dia marcado |
| Semana | N | A cada N semanas |
| Mês | 1 | Cada mês. Com dia do mês, a ocorrência é esse dia. Sem dia, a janela é o mês |
| Mês | N | A cada N meses a partir do mês da âncora. N = 3 é o trimestre |
| Ano | 1 | Cada ano |
| Ano | N | A cada N anos a partir do ano da âncora |

Fuso padrão: `America/Sao_Paulo`, editável em Ajustes.

Situação da tarefa: Ativa, Futura ou Arquivada. Futura já tem recorrência definida, não gera ocorrência e não entra na pontuação máxima até ser promovida.

Não existe tarefa de uma ocorrência só. O que acontece uma vez é evento.

### Tipo da tarefa

O tipo é independente da recorrência.

| Tipo | Efeito |
| --- | --- |
| Obrigatória | Entra na pontuação máxima, na ofensiva do período e na condição padrão de recompensa. Importada do Notion já como rotina ativa. |
| Normal | Entra na pontuação máxima. Não bloqueia sozinha a recompensa padrão. Tem ofensiva própria. |
| Extra | Entra na pontuação e na pontuação máxima. Não bloqueia a recompensa padrão nem a ofensiva do período. Vale ponto além do núcleo do dia. |

Exemplos de obrigatórias: manter a ofensiva do GitHub e a do Duolingo. Exemplo de extra: não usar o Instagram para procrastinar fora do horário combinado.

### Evento

- Uma data e uma hora de início. Hora de fim é opcional.
- Não repete, não tem ofensiva e não entra na pontuação nem na pontuação máxima da rotina.
- Entra em Hoje no dia marcado e no cartão “o que fazer agora” quando a hora chega.
- Status: Não iniciada, Em andamento, Concluída.
- Pode ter categoria, anotação e lembrete no Google Calendar.
- Cancelar arquiva. Não vira tarefa.

Exemplo: “Levar a TV ao conserto”, amanhã, 12:00. No dia seguinte ao meio-dia o destaque mostra esse evento. No outro dia ele não volta.

### Status

Estados persistidos, em tarefa (na ocorrência), evento e meta: Não iniciada, Em andamento, Concluída.

Atraso não é um quarto status. A interface marca como atrasada a ocorrência ou o evento cujo horário já passou e que ainda não está concluído.

Tarefa composta (academia com musculação e cardio):

- Filhos definem o pai quando o pai está no modo agrupador.
- Nenhum filho iniciado: pai Não iniciada.
- Algum filho em andamento ou concluído, sem fechar todos: pai Em andamento.
- Todos os filhos concluídos: pai Concluída.
- Dá para desligar o modo agrupador e marcar o pai na mão.

Evento não tem filhos. Meta tem submetas, que são metas.

### Pontuação

- Ponto padrão de uma tarefa: 1. O campo `pontos` aceita inteiro maior que zero.
- Evento não pontua.
- Tarefa agrupadora não soma os próprios pontos. Somam as folhas. Um interruptor “também pontua” faz o pai somar além dos filhos.
- Pontuação do período = soma dos pontos das ocorrências concluídas naquele horizonte, mais a soma das ocorrências de recorrências contidas nele. O mês inclui os dias, as semanas e a tarefa trimestral que cai naquele mês.
- Pontuação máxima = soma dos pontos de todas as ocorrências de tarefa planejadas naquele recorte, inclusive extras. É o teto da rotina disponível. Evento fica de fora.
- Pontuação e pontuação máxima existem no geral e em cada categoria.
- Tarefa futura não entra na pontuação máxima até ser promovida.
- Ao fechar o período, a pontuação fica gravada. O período aberto é calculado ao vivo.

Os períodos que fecham e têm pontuação são dia, semana, mês e ano. “A cada 3 meses” é recorrência de tarefa, não um quinto placar.

### Ofensiva

A ofensiva conta ocorrências encerradas daquela tarefa, não um saldo com sinal. Evento não tem ofensiva.

- Cumpriu a ocorrência e a sequência negativa estava em zero: sequência positiva sobe 1.
- Cumpriu a ocorrência e a sequência negativa era maior que zero: negativa vai a zero e a positiva passa a valer 1. Não se subtrai a negativa.
- O ciclo encerrou sem conclusão: positiva vai a zero e a negativa sobe 1.
- Enquanto o ciclo da ocorrência não encerrou, a falta ainda não conta.
- Concluir depois do encerramento não reescreve a ofensiva já fechada. A próxima ocorrência cumprida é que inicia a positiva em 1.
- Guardar também a maior sequência positiva e a maior sequência negativa, só como histórico.
- Na recorrência “a cada N”, cada ciclo é uma ocorrência. Perder um trimestre sobe a negativa em 1. Os meses do meio não geram falta.

Exemplo que a implementação tem de passar: a tarefa é toda quinta. Cinco quintas sem fazer deixam a ofensiva negativa em 5 e a positiva em 0. Na sexta quinta feita, a negativa zera e a positiva fica em 1. Não existe “5 negativas menos 1”.

Ofensiva do período (dia, semana, mês, ano) olha só as obrigatórias daquele horizonte. Uma extra perdida não quebra essa ofensiva. Cada tarefa recorrente, inclusive extra, tem a própria ofensiva, que pode ser desligada.

Uma meta pode usar uma ofensiva como progresso: “30 dias de dieta” completa quando a sequência positiva da tarefa vinculada chega a 30. A meta não ganha data por causa disso. Se a sequência quebra antes, a meta volta a Em andamento. O interruptor é editável.

### Recompensa

Recompensa é uma definição editável mais um resgate.

Condições disponíveis, combináveis com E/OU:

- Concluir todas as obrigatórias do horizonte.
- Atingir uma pontuação.
- Atingir uma ofensiva.
- Concluir uma meta ou uma lista.
- Resgatar todas as recompensas do horizonte inferior (dia alimenta semana, semana alimenta mês, mês alimenta ano).

Padrão inicial, trocável:

- Diária, semanal, mensal e anual existem e começam ligadas à condição “todas as obrigatórias daquele horizonte”.
- Meta pode não ter recompensa, ter recompensa ao concluir aquele nó, ou só a meta raiz pagar recompensa.
- Lista de metas pode ter uma recompensa própria de fechar a lista.

A Secretaria propõe recompensa (por exemplo, academia em todos os dias da semana → lanche ou delivery). A proposta só vira recompensa depois do aceite, e em seguida pode ser editada.

### Metas e listas

- Meta é objetivo. Não tem início, fim nem dia marcado.
- Submeta é a mesma entidade, com meta pai. A árvore não tem limite de profundidade.
- Meta pode pertencer a uma lista, estar ligada a uma tarefa, ou às duas.
- Progresso do pai com filhos: concluídas ÷ filhas diretas. Ao concluir todas as filhas, o pai conclui. Dá para desligar.
- Lista de metas é o único lugar com prazo. Tem título, início opcional e fim. “Lista de Metas para 2027” termina em 31/12/2027. “Major Goals by 2028” é uma lista com fim em 2028.
- As metas dentro da lista continuam sem data. O prazo é da lista.
- Exemplo: lista até 2028 → meta “Perder peso e ter um corpo saudável” → submeta “Publicar comparativos de antes e depois” → submeta “Vídeos curtos de frente, costas, lado e rosto, com música, diferença de peso, datas e medida do abdômen”.
- Lista ou meta futura não aparece no ciclo ativo até ser promovida.
- A lista conclui quando todas as metas dela concluem. Chegar no prazo também encerra o ciclo da lista para a Secretaria, mesmo que ainda exista meta aberta.

### O que fazer agora

No fuso da pessoa, entre ocorrências de tarefa e eventos de hoje ainda não concluídos:

1. Janela de horário contém a hora atual.
2. Nesse grupo: obrigatória, evento, normal, extra.
3. Horário de início mais cedo.
4. Empate: categoria cujo foco principal está marcado como prioridade da pessoa, depois ordem manual.

Se nenhuma janela contém agora, mostrar o próximo horário do dia, seja tarefa ou evento. Tarefas sem horário ficam numa lista “Sem horário”, abaixo do destaque. Evento sem hora não existe.

### Secretaria Pessoal

Ela é a secretária pessoal da conta. Conversa em texto, no app e no WhatsApp, no mesmo histórico. Não grava rotina, meta, lista, evento nem recompensa sem um aceite explícito na conversa ou no cartão da sugestão.

Momentos em que ela fala primeiro:

- Fechamento do dia, da semana, do mês ou do ano.
- Conclusão de uma lista de metas.
- Chegada do prazo de uma lista, mesmo com meta ainda aberta.
- Quando a pessoa abre a conversa e escreve.

Em todo fechamento de período e em todo encerramento de lista, a fala inclui, nesta ordem:

1. Um resumo curto do que fechou.
2. Dica, se houver o que observar.
3. Proposta de tarefa, de promoção de futura ou de meta nova, se fizer sentido.
4. Proposta de recompensa, se fizer sentido.
5. A pergunta, sempre: “Tem algum evento novo para eu agendar?”

A pergunta do evento entra mesmo quando não houver dica nem proposta. “Não” encerra esse assunto sem criar evento. Uma descrição (“amanhã ao meio-dia levar a TV ao conserto”) vira uma proposta com título, data e hora. Só o “pode agendar” cria o evento.

Escolha da futura a promover:

1. Ignorar itens recusados duas vezes seguidas, até a pessoa reabilitar em Ajustes ou na própria futura.
2. Preferir a mesma categoria das tarefas que a pessoa está cumprindo.
3. Preferir o que se encaixa no foco principal das categorias.
4. Se a pessoa recusou aquela futura na última sugestão, oferecer outra na próxima.

No prazo de uma lista com meta ainda aberta, ela pode propor levar essas metas para uma lista nova. A lista nova só nasce com aceite.

Modelo: Gemini Flash gratuito vigente, nome do modelo em Ajustes. A chave fica só no servidor. O nome Secretaria Pessoal não muda quando o modelo muda.

Classificação que o prompt dela obriga:

- Acontece uma vez, com data ou hora → evento.
- Se repete → tarefa.
- Objetivo sem data → meta.
- Conjunto com prazo → lista de metas.
- Dúvida → pergunta, sem gravar.

### WhatsApp

O mesmo diálogo do app. A pessoa vincula o próprio número em Ajustes e passa a falar com a Secretaria como fala com um contato.

- Só o número vinculado mexe na conta. Mensagem de outro número recebe aviso de que não está vinculado e não grava nada.
- O histórico mostrado no app inclui as mensagens do WhatsApp, marcadas com o canal.
- Fechamento e encerramento de lista também chegam no WhatsApp quando o canal está vinculado.
- Dentro da janela de 24 horas aberta pela pessoa, a resposta vai em texto livre. Fora dela, a Secretaria envia o modelo de mensagem aprovado na Meta para avisar que há uma conversa no app, e o texto completo fica no app.
- Áudio, imagem e figurinha não são interpretados. Ela responde pedindo a mesma coisa por escrito.

O Gemini continua gratuito. A conversa do WhatsApp Business Platform é cobrada pela Meta. A tela de vínculo mostra isso antes de conectar.

### Importação do Notion

- Obrigatórias do Schedule entram ativas na primeira importação, como tarefas recorrentes.
- Item de uma vez entra como evento, se a prévia tiver data e hora. Sem data, fica na prévia para a pessoa marcar ou mandar para futura.
- Objetivo sem recorrência entra como meta. Conjunto com prazo entra como lista. O restante da rotina entra como tarefa futura.
- A pessoa puxa mais itens depois, aos poucos, pela mesma tela.
- A importação não apaga edição local. Itens já importados atualizam só os campos que a pessoa não marcou como editados localmente.
- Não há sincronização contínua de volta para o Notion nesta versão.

### O que esta versão não faz

- Vários usuários na mesma rotina.
- Publicar nas lojas nativas. O PWA instalável cobre celular e desktop.
- Escrever de volta no Notion.
- Trocar o provedor de IA por um pago.
- Cliente não oficial de WhatsApp.
- Entender áudio ou imagem na conversa.

## 3. Stack

| Camada | Escolha |
| --- | --- |
| Interface | Vue 3, TypeScript, Vite, Pinia, Vue Router, Tailwind CSS, lucide-vue-next |
| PWA | vite-plugin-pwa, manifesto, ícones 192 e 512, cache do app e fila offline |
| API | ASP.NET Core na LTS atual, controllers, autenticação por cookie |
| Domínio | Projeto separado, sem referência a EF nem HTTP |
| Dados | EF Core e SQLite em `backend/data/rotinaflow.db` |
| Testes | xUnit no domínio (ofensiva, pontuação, recorrência, evento, progresso de meta, ritual) |
| IA | Gemini Flash, chamada só no servidor, resposta em JSON. Nome de produto: Secretaria Pessoal |
| WhatsApp | WhatsApp Business Platform (Cloud API), webhook na API |
| Calendário | Google Calendar, OAuth, lembretes criados aqui |
| Notion | API oficial, leitura da página Schedule e dos bancos ligados a ela |

Estrutura:

```text
rotina-flow/
  PLANEJAMENTO.md
  README.md
  frontend/
  backend/
    RotinaFlow.Domain/
    RotinaFlow.Infrastructure/
    RotinaFlow.Api/
    RotinaFlow.Tests/
```

Variáveis, nunca no frontend:

- `Gemini:ApiKey`, `Gemini:Model`
- `Notion:Token`
- `Google:ClientId`, `Google:ClientSecret`
- `WhatsApp:Token`, `WhatsApp:PhoneNumberId`, `WhatsApp:VerifyToken`, `WhatsApp:AppSecret`
- `App:FusoHorario` (`America/Sao_Paulo`)

Um único usuário, criado no primeiro acesso (nome, e-mail e senha). Todas as entidades carregam `UsuarioId`.

## 4. Categorias iniciais

O seed cria as cinco. Depois a tela Categorias edita, reordena, arquiva e cria outras. Arquivar não apaga histórico.

| Nome | Foco principal | Origem no Notion | Ícone |
| --- | --- | --- | --- |
| Físico | Perder peso | Body (Lose Weight) | haltere |
| Idioma | Inglês | Language (English) | idiomas |
| Profissão | Tecnologia | Profession (Technology) | monitor |
| Organização | nenhum | Organize | pastas |
| Outros | nenhum | Other | círculo |

Cada categoria tem cor, ícone, ordem e um interruptor “foco prioritário”. No seed, Físico, Idioma e Profissão nascem com foco preenchido. Organização e Outros nascem sem foco.

## 5. Modelo de dados

```text
Usuario
Categoria
Tarefa
Ocorrencia
Evento
Ofensiva
ListaMetas
Meta
Recompensa
ResgateRecompensa
Anotacao
Mensagem
Sugestao
VinculoWhatsApp
VinculoCalendario
ImportacaoNotion
ItemImportadoNotion
FechamentoPeriodo
```

### Usuario

`Id`, `Nome`, `Email`, `SenhaHash`, `FusoHorario`, `CriadoEm`.

### Categoria

`Id`, `UsuarioId`, `Nome`, `FocoPrincipal` (nulo se não houver), `Icone`, `Cor`, `Ordem`, `FocoPrioritario`, `Arquivada`.

### Tarefa

Definição recorrente, não a execução de um dia e não um evento.

`Id`, `UsuarioId`, `CategoriaId`, `TarefaPaiId` nulo, `Titulo`, `Descricao`, `Icone`, `Tipo` (Obrigatoria, Normal, Extra), `Unidade` (Dia, Semana, Mes, Ano), `Intervalo` (inteiro ≥ 1), `Ancora`, `Pontos`, `Agrupadora`, `TambemPontua`, `Situacao` (Ativa, Futura, Arquivada), `RecorrenciaJson`, `HoraInicio`, `HoraFim`, `OfensivaLigada`, `NotionPageId`, `EditadoLocalmente`, `Ordem`.

`RecorrenciaJson` completa a unidade: dias da semana, dia do mês, mês do ano. Tarefa diária com intervalo 1 e sem dias marcados vale todos os dias. Intervalo maior que 1 sem âncora é inválido.

### Ocorrencia

Uma execução de tarefa num ciclo. Evento não gera ocorrência.

`Id`, `TarefaId`, `Inicio`, `Fim`, `Status` (NaoIniciada, EmAndamento, Concluida), `PontosComputados`, `ConcluidaEm`, `FechadaEm`.

Índice único: tarefa + início + fim.

### Evento

`Id`, `UsuarioId`, `CategoriaId` nulo, `Titulo`, `Descricao`, `Icone`, `Inicio`, `Fim` nulo, `Status` (NaoIniciada, EmAndamento, Concluida), `Arquivado`, `NotionPageId`, `EditadoLocalmente`.

`Inicio` é obrigatório. Não há campos de recorrência, pontos nem ofensiva.

### Ofensiva

Uma linha por tarefa.

`TarefaId`, `SequenciaPositiva`, `SequenciaNegativa`, `MaiorPositiva`, `MaiorNegativa`, `UltimaOcorrenciaAvaliadaId`.

Regra de consistência: no máximo uma das sequências ativas é maior que zero.

### ListaMetas

`Id`, `UsuarioId`, `CategoriaId` nulo, `Titulo`, `Descricao`, `Icone`, `Inicio` nulo, `Fim`, `Situacao` (Ativa, Futura, Concluida, Arquivada), `NotionPageId`.

`Fim` é o prazo da lista. Meta não copia esse campo.

### Meta

`Id`, `UsuarioId`, `ListaId` nulo, `TarefaId` nulo, `MetaPaiId` nulo, `CategoriaId` nulo, `Titulo`, `Descricao`, `Icone`, `Status`, `Situacao` (Ativa, Futura, Arquivada), `Ordem`, `ConcluirPaiAutomaticamente`, `ModoRecompensa` (Nenhuma, NesteNo, SoNaRaiz), `ProgressoPorOfensiva`, `TarefaOfensivaId` nulo, `AlvoOfensiva` nulo, `NotionPageId`, `EditadoLocalmente`.

Sem `Inicio` e sem `Fim`.

### Recompensa

`Id`, `UsuarioId`, `Titulo`, `Descricao`, `Icone`, `Escopo` (Diario, Semanal, Mensal, Anual, Meta, Lista), `MetaId` nulo, `ListaId` nulo, `CondicaoJson`, `Ativa`, `OrigemSugestaoId` nulo.

`CondicaoJson` lista cláusulas `{ tipo, parametros, operador }`.

### ResgateRecompensa

`Id`, `RecompensaId`, `InicioPeriodo`, `FimPeriodo`, `ResgatadoEm`. Único por recompensa e período.

### Anotacao

`Id`, `TarefaId` nulo, `MetaId` nulo, `EventoId` nulo, `OcorrenciaId` nulo, `Texto`, `CriadoEm`, `AtualizadoEm`. Pelo menos um vínculo.

### Mensagem

`Id`, `UsuarioId`, `Canal` (App, WhatsApp), `Direcao` (Usuario, Secretaria), `Texto`, `WhatsAppMessageId` nulo, `SugestaoId` nulo, `CriadoEm`.

Uma conversa por usuário. Os dois canais appendam nela.

### Sugestao

`Id`, `UsuarioId`, `Tipo` (Dica, QuebrarTarefa, PromoverFutura, NovaTarefa, NovaMeta, NovaLista, LevarMetas, Recompensa, PerguntarEvento, CriarEvento), `Titulo`, `Texto`, `PayloadJson`, `Status` (Pendente, Aceita, Recusada, Adiada), `AlvoTarefaId` nulo, `AlvoMetaId` nulo, `AlvoListaId` nulo, `FechamentoId` nulo, `CriadoEm`, `RespondidoEm`.

`PerguntarEvento` não grava evento. `CriarEvento` só grava no aceite, com título e início no payload. Recusa grava o alvo. Duas recusas seguidas do mesmo alvo, sem aceite no meio, tiram o item da fila até reabilitar.

### VinculoWhatsApp

`UsuarioId`, `TelefoneE164`, `CodigoVinculo`, `VinculadoEm` nulo, `UltimaJanelaAte` nulo.

### VinculoCalendario

`Id`, `TarefaId` nulo, `EventoId` nulo, `GoogleEventId`, `CalendarioId`, `MinutosAntes`, `UltimaSincronizacao`, `Erro`.

Exatamente um dos dois vínculos locais. Tarefa vira lembrete recorrente. Evento vira lembrete único.

### ImportacaoNotion e ItemImportadoNotion

Importação: `Id`, `IniciadaEm`, `EncerradaEm`, `Resumo`.

Item: `NotionPageId`, `TipoLocal` (Tarefa, Evento, Meta, Lista), `LocalId`, `HashVisto`, `EditadoLocalmente`, `Classificacao` (ObrigatoriaAtiva, Futura, Evento, Meta, Lista).

### FechamentoPeriodo

`Id`, `Tipo` (Dia, Semana, Mes, Ano, Lista), `Inicio`, `Fim`, `ListaId` nulo, `Pontuacao`, `PontuacaoMaxima`, `PorCategoriaJson`, `OfensivasAvaliadas`, `RitualGerado`, `FechadoEm`.

`RitualGerado` fica falso se a Secretaria não conseguiu montar a fala, para a pessoa poder pedir de novo. O fechamento em si não é desfeito.

## 6. Regras que os testes de domínio travam

Arquivo de testes obrigatório antes da interface correspondente.

1. Cinco faltas e um cumprimento viram positiva 1 e negativa 0.
2. Cinco cumprimentos seguidos viram positiva 5 e negativa 0.
3. Cumprimento com positiva já em 4 vai a 5, sem mexer na maior negativa.
4. Falta no meio de uma positiva zera a positiva e põe a negativa em 1.
5. Ocorrência ainda aberta não altera ofensiva.
6. Conclusão depois do fechamento não altera a ofensiva daquela ocorrência.
7. Extra perdida não quebra a ofensiva do dia se as obrigatórias foram cumpridas.
8. Obrigatória perdida quebra a ofensiva do dia.
9. Pontuação máxima ignora futuras e inclui extras planejadas. Evento não entra.
10. Agrupadora sem “também pontua” não soma em dobro com os filhos.
11. Agrupadora com “também pontua” soma pai e filhos.
12. Mês soma ocorrências diárias, semanais, mensais e a trimestral que cai naquele mês.
13. Categoria filtra a mesma conta.
14. Meta por ofensiva completa em 30 e reabre se a sequência cai. Padrão: reabre. A meta segue sem data.
15. Pai com três submetas, duas concluídas, fica em 2/3 e Em andamento.
16. Pai conclui quando a última filha conclui, se o automático está ligado.
17. Recompensa diária padrão não resgata se falta obrigatória, mesmo com todas as extras feitas e com o evento do dia concluído.
18. Sugestão de promoção pula a futura com duas recusas seguidas.
19. Âncora em janeiro de 2026, unidade mês, intervalo 3, gera janeiro, abril, julho e outubro, e não gera fevereiro.
20. Concluir ou perder um evento não altera ofensiva nem pontuação.
21. Evento de amanhã ao meio-dia fica fora do Hoje de hoje e dentro do Hoje de amanhã.
22. Montar uma meta com início ou fim é rejeitado. Montar uma lista sem fim é rejeitado.
23. Fechar um dia e concluir uma lista produzem, cada um, uma sugestão `PerguntarEvento`.
24. Aceitar `CriarEvento` grava um evento e não grava tarefa.
25. Mensagem com telefone diferente do vínculo não cria sugestão nem evento.

## 7. API

Prefixo `/api`. Cookie de autenticação. Respostas de erro em português, com código estável. O webhook do WhatsApp autentica pela assinatura da Meta, não pelo cookie.

| Área | Operações |
| --- | --- |
| Sessão | registrar o primeiro usuário, entrar, sair, eu |
| Categorias | listar, criar, editar, reordenar, arquivar |
| Tarefas | CRUD, arquivar, tornar futura, promover, reordenar, mover de pai |
| Ocorrências | listar por intervalo, mudar status, concluir, reabrir |
| Eventos | CRUD, arquivar, concluir, listar por intervalo |
| Hoje | o que fazer agora, rotina do dia, eventos do dia, pontuação do dia |
| Períodos | semana, mês, ano, fechar |
| Ofensivas | listar, detalhe, ligar/desligar na tarefa |
| Listas e metas | CRUD da árvore, promover, arquivar, concluir. Meta sem campos de data |
| Recompensas | CRUD, condições, elegibilidade do período, resgatar, desfazer resgate |
| Anotações | CRUD no vínculo |
| Pontuação | geral e por categoria, período aberto e fechamentos |
| Secretaria | listar a conversa, enviar texto, aceitar, recusar, adiar, reabilitar alvo |
| WhatsApp | pedir vínculo, confirmar, desvincular, status, webhook de verificação e de mensagem |
| Calendário | URL de conexão, callback, status, ligar lembrete, desligar, sincronizar |
| Notion | status do token, pré-visualizar Schedule, confirmar importação, importar seleção |
| Ajustes | fuso, modelo Gemini, reabilitar futuras recusadas |

Aceitar sugestão aplica o `PayloadJson` já validado no domínio: criar tarefas filhas, promover futura, criar meta, criar lista, levar metas, criar recompensa ou criar evento. Aceitar uma dica ou um “não” na pergunta de evento só marca a sugestão.

## 8. Telas

Navegação inferior no celular e lateral no desktop:

Hoje, Semana, Metas, Secretaria, Mais.

O botão da Secretaria fica também flutuante nas outras telas e abre a conversa.

Mais abre: Mês, Ano, Eventos, Futuras, Recompensas, Ofensivas, Calendário, Notion, Categorias, Ajustes.

Ícone em categoria, tipo, status, ofensiva, recompensa, meta, evento e ação principal. Toque grande para concluir. Evento usa ícone de marcação única. Tarefa usa ícone de repetição.

| Tela | Conteúdo |
| --- | --- |
| Entrar | Primeiro acesso cria a conta. Depois, e-mail e senha |
| Hoje | Cartão “agora”, eventos de hoje, pontuação do dia geral e por categoria, obrigatórias, normais, extras, sem horário, ofensivas em risco, recompensa do dia |
| Semana | Grade dos sete dias com a rotina e os eventos do dia, tarefas semanais, pontuação, ofensiva da semana, recompensa |
| Mês | Idem no mês, incluindo o acumulado e os eventos |
| Ano | Idem no ano |
| Tarefa | Cabeçalho, recorrência, status, pontos, ofensiva, subtarefas, metas ligadas, anotações, histórico, lembrete |
| Evento | Data, hora, status, categoria, anotação, lembrete. Sem recorrência e sem ponto |
| Eventos | Próximos e concluídos, criar e editar |
| Futuras | Tarefas e metas futuras, promover na mão, ver por que a Secretaria ainda não sugeriu |
| Listas de metas | Cartões com prazo e progresso |
| Árvore da lista | Metas sem data, status, recompensa do nó, vínculo com ofensiva |
| Recompensas | Definições e resgates do período atual |
| Ofensivas | Positivas e negativas separadas, nunca um número líquido |
| Calendário | Conectar Google, escolher quais tarefas e eventos geram lembrete, minutos antes |
| Notion | Prévia classificada em tarefa, evento, meta ou lista, confirmar, importar de novo |
| Categorias | CRUD, foco, ícone, cor, ordem |
| Secretaria | Conversa única. Nome Secretaria Pessoal. Campo de texto fixo. Cartões aceitar, recusar e adiar dentro da fala dela. Aviso do canal WhatsApp |
| Ajustes | Fuso, modelo, conta, vínculo do WhatsApp, reabilitar recusas |

Estados vazios em toda lista: o que é, e um botão para criar ou importar.

Formulários de criar e editar usam os mesmos campos do modelo. Não há entidade sem tela de edição, fora sessão, mensagem, fechamento e item importado, que são sistema. O formulário de meta não oferece data. O formulário de lista exige o prazo. O formulário de evento exige data e hora e não oferece repetição. O formulário de tarefa exige unidade e intervalo.

## 9. Integrações

### Secretaria Pessoal (Gemini Flash)

- Serviço no Infrastructure, interface no Domain (`ISecretariaPessoal`).
- Entrada: texto da pessoa ou ritual de fechamento, resumo de pontuação, faltas de obrigatórias, ofensivas, futuras elegíveis, recusas, focos, eventos já marcados e prazo de listas.
- Saída JSON: uma fala em português e a lista de sugestões com tipo, título, texto e payload.
- Payload de quebra: tarefas filhas propostas, ainda não salvas.
- Payload de promoção: id local da futura e a recorrência de destino.
- Payload de meta: título e lista de destino, sem data.
- Payload de lista: título e fim.
- Payload de recompensa: título, escopo e condição.
- Payload de evento: título, início e fim opcional. Sem recorrência.
- O ritual sempre inclui uma sugestão `PerguntarEvento`.
- Falha de rede ou JSON inválido vira aviso na conversa, sem desfazer o fechamento e sem duplicar sugestão.

### WhatsApp

- Cloud API oficial. Webhook de verificação e de mensagens na API.
- Vínculo: Ajustes mostra um código e o número da Secretaria. A pessoa manda esse código no WhatsApp. O webhook casa código e telefone e grava o vínculo.
- Mensagem do número vinculado entra na conversa e segue o mesmo aceite das sugestões.
- Mensagem de outro número não lê a rotina e não grava.
- `WhatsAppMessageId` único evita processar a mesma mensagem duas vezes.
- Fora da janela de 24 horas, sai só o modelo aprovado na Meta. A conversa completa permanece no app.
- Sem as chaves, o app funciona e a tela de vínculo explica o que falta, inclusive o custo da Meta.

### Google Calendar

- Escopo de lembretes no calendário primário, editável o id do calendário.
- Tarefa com lembrete cria ou atualiza um lembrete recorrente, no mesmo intervalo da rotina.
- Evento com lembrete cria ou atualiza um lembrete único, na data e hora dele.
- Editar horário ou recorrência atualiza o lembrete.
- Arquivar, cancelar ou desligar remove o lembrete.
- Concluir a ocorrência ou o evento não apaga o lembrete.
- Sem conexão, tarefa e evento funcionam igual, sem lembrete.

### Notion

Ordem fixa da primeira entrega da importação:

1. Ler a página Schedule e listar bancos e páginas filhas. Gravar em `docs/notion-schedule.md` os nomes reais das propriedades antes de mapear no código.
2. Mapa inicial, ajustado por esse apêndice:

| Notion | rotina-flow |
| --- | --- |
| Título | Título |
| Goal category | Categoria pelo mapa da seção 4 |
| Max Score | Pontos da tarefa, se for número maior que zero. Senão, 1. Evento ignora ponto |
| Status Not Started / In Progress / Done | Não iniciada / Em andamento / Concluída, se o item já estiver ativo |
| Página com prazo que contém objetivos | Lista de metas. As filhas viram metas sem data |
| Objetivo sem repetição | Meta |
| Página de rotina recorrente | Tarefa, com unidade e intervalo |
| Página de uma vez, com data | Evento |
| Marca de obrigatória, quando existir no banco | Tipo Obrigatória e situação Ativa |
| Rotina sem essa marca | Situação Futura |

3. Pré-visualização editável: a pessoa corrige o registro (tarefa, evento, meta ou lista), o tipo e a recorrência antes de gravar.
4. Confirmar grava os escolhidos e a ligação `NotionPageId`.
5. Nova importação mostra só o que ainda não entrou e o que mudou no Notion.

Enquanto o apêndice de propriedades não existir, a fase Notion não começa a gravar mapeamento definitivo.

## 10. PWA e offline

- Manifesto: nome rotina-flow, nome curto Rotina, `display: standalone`, idioma `pt-BR`, cor de tema definida no Tailwind.
- Ícones 192 e 512, mais máscara.
- Service worker gerado pelo Vite. App shell em cache.
- IndexedDB guarda o dia aberto e uma fila de mudanças de status de ocorrência e de evento.
- Sem rede, Hoje abre com a última cópia e concluir entra na fila.
- Com rede, a fila envia em ordem. Conflito de status: o mais recente no aparelho ganha se o servidor não tiver fechado o item. Se já fechou, o aparelho fica com o estado do servidor e avisa.
- Secretaria, WhatsApp, Notion e Google exigem rede e mostram isso.
- Instalável no Chrome e no Edge, celular e desktop. HTTPS fora de localhost.

## 11. Ordem de execução

Não antecipar tela de uma fase se o domínio dela ainda não tem teste. Não conectar Gemini, WhatsApp, Google ou Notion antes do núcleo local estar utilizável no Hoje.

Critério de saída de cada fase está na última tarefa da fase.

---

### Fase 0 — Fundação

- [ ] 0.1 Criar a solution `backend/RotinaFlow.sln` com Api, Domain, Infrastructure e Tests, na LTS atual do .NET.
- [ ] 0.2 Api sobe em development, SQLite em `backend/data/rotinaflow.db`, CORS só para a origem do Vite.
- [ ] 0.3 Criar `frontend` com Vue 3, TypeScript, Vite, Vue Router, Pinia, Tailwind e lucide.
- [ ] 0.4 Rota inicial vazia em português e script único na raiz do repositório para subir API e frontend juntos, documentado no README em poucas linhas de como rodar.
- [ ] 0.5 Pasta de configuração de segredos ignorada pelo Git (chave Gemini, token Notion, segredo Google, token WhatsApp). Não commitar segredo.
- [ ] 0.6 Saída: `dotnet test` verde (ainda sem regras) e o frontend abre a rota inicial.

### Fase 1 — Conta, categorias e tarefas

- [ ] 1.1 Entidades Usuario, Categoria e Tarefa, migração inicial e seed das cinco categorias da seção 4.
- [ ] 1.2 Primeiro acesso cria o usuário. Entrar e sair com cookie. Rotas da API recusam anônimo.
- [ ] 1.3 CRUD de categorias com ícone, cor, foco, ordem e arquivar.
- [ ] 1.4 CRUD de tarefas com tipo, unidade, intervalo, âncora, dias, pontos, horário, pai, agrupadora e “também pontua”. Recusar tarefa sem recorrência.
- [ ] 1.5 Telas Entrar, Categorias e lista de Tarefas com criar e editar. Ícones nos tipos e nas categorias.
- [ ] 1.6 Saída: dá para criar “Academia” toda segunda, quarta e sexta, agrupadora, com “Musculação” e “Cardio”, categoria Físico, e uma tarefa “Revisão trimestral” a cada 3 meses. As duas editam de novo.

### Fase 2 — Ocorrências e o dia

- [ ] 2.1 Gerar ocorrências do intervalo pedido a partir da recorrência e do fuso, incluindo o teste 19. Não gerar para situação Futura ou Arquivada.
- [ ] 2.2 Mudar status, concluir e reabrir. Status do pai agrupador segue os filhos.
- [ ] 2.3 Endpoint e cartão “o que fazer agora” com a ordem da seção 2, ainda só com tarefas.
- [ ] 2.4 Tela Hoje: agora, obrigatórias, normais, extras, sem horário. Concluir em um controle só.
- [ ] 2.5 Tela da tarefa: dados, recorrência, subtarefas e anotações.
- [ ] 2.6 Saída: uma quinta-feira de teste mostra a tarefa de quinta em Hoje e não mostra a tarefa futura nem a trimestral fora do mês âncora.

### Fase 3 — Eventos

- [ ] 3.1 Entidade Evento, CRUD e testes 20 e 21.
- [ ] 3.2 Incluir o evento de hoje no Hoje e no “o que fazer agora”, na ordem da seção 2.
- [ ] 3.3 Telas Evento e Eventos, com data e hora obrigatórias e sem campo de repetição.
- [ ] 3.4 Semana e, quando existirem, mês e ano, mostram o evento no dia dele.
- [ ] 3.5 Saída: “Levar a TV ao conserto” amanhã ao meio-dia não aparece no Hoje de hoje, aparece amanhã e não cria tarefa nem ofensiva.

### Fase 4 — Ofensivas

- [ ] 4.1 Serviço de domínio da ofensiva com os testes 1 a 8 da seção 6.
- [ ] 4.2 Ao fechar a ocorrência (fim do ciclo no fuso), avaliar ofensiva uma única vez. Evento não passa por esse serviço.
- [ ] 4.3 Job leve na API: ao atender qualquer leitura de Hoje, fechar o que já venceu e ainda não foi avaliado.
- [ ] 4.4 Ofensiva do período só com obrigatórias. Ofensiva por tarefa respeita `OfensivaLigada`.
- [ ] 4.5 Tela Ofensivas e o bloco de ofensiva na tarefa e no Hoje. Positiva e negativa em indicadores separados.
- [ ] 4.6 Saída: o exemplo das cinco quintas e do cumprimento seguinte passa no teste e aparece na tela como positiva 1.

### Fase 5 — Pontuação e períodos

- [ ] 5.1 Serviço de pontuação com os testes 9 a 13 e 17 no que couber sem recompensa ainda.
- [ ] 5.2 Telas Semana, Mês e Ano com pontuação geral, pontuação máxima e quebra por categoria. Eventos visíveis e fora da conta.
- [ ] 5.3 Fechamento grava `FechamentoPeriodo` sem chamar a Secretaria ainda.
- [ ] 5.4 Histórico de ocorrências na tela da tarefa.
- [ ] 5.5 Saída: um dia com obrigatória de 1 ponto, normal de 2, extra de 1 e um evento concluído mostra máxima 4. Concluir só a extra mostra pontuação 1 de 4. O mês soma esse dia e a trimestral que cair nele.

### Fase 6 — Metas e listas

- [ ] 6.1 ListaMetas com prazo obrigatório e Meta sem data, em árvore, progresso e conclusão automática. Testes 14 a 16 e 22.
- [ ] 6.2 Meta ligada a tarefa e meta ligada a ofensiva com alvo, ainda sem data na meta.
- [ ] 6.3 Telas de listas, árvore, criar, editar, aninhar, arquivar e promover futura. O formulário de meta não mostra calendário.
- [ ] 6.4 Na tela da tarefa, aba de metas ligadas àquela tarefa.
- [ ] 6.5 Situação Futura fica fora das telas ativas e dentro de Futuras.
- [ ] 6.6 Saída: dá para montar a Lista de Metas para 2027, ou a lista até 2028, com a meta de corpo, a submeta de posts e a submeta dos vídeos curtos, e concluir de baixo para cima. Nenhuma meta fica com data própria.

### Fase 7 — Recompensas

- [ ] 7.1 CRUD de recompensa e condições da seção 2, teste 17 completo.
- [ ] 7.2 Seed das quatro recompensas de horizonte, condição “todas as obrigatórias”, textos editáveis e genéricos (a pessoa troca pelo prêmio real).
- [ ] 7.3 Elegibilidade ao vivo e resgate único por período. Desfazer resgate enquanto o período não fechou.
- [ ] 7.4 Modo de recompensa da meta: nenhuma, neste nó, só na raiz. Resgate ao concluir. Recompensa opcional da lista.
- [ ] 7.5 Tela Recompensas e o selo de resgate no Hoje, Semana, Mês, Ano e na meta.
- [ ] 7.6 Saída: dia com obrigatória pendente não resgata. Dia com obrigatórias feitas resgata, mesmo com extra pendente e com evento pendente.

### Fase 8 — Secretaria Pessoal no app

- [ ] 8.1 Interface `ISecretariaPessoal` e adaptador Gemini Flash. Modelo configurável. Sem chave, a conversa explica como configurar e o resto do app segue.
- [ ] 8.2 Conversa no app: enviar texto, guardar `Mensagem`, responder com fala e sugestões. Botão flutuante e item Secretaria na navegação. O título da tela é Secretaria Pessoal.
- [ ] 8.3 Ritual no fechamento de dia, semana, mês e ano, na conclusão da lista e no prazo da lista. Teste 23. A pergunta de evento entra sempre.
- [ ] 8.4 Aceitar quebra, promoção, meta, lista, levar metas, recompensa e evento. Teste 24. Recusar e adiar não mudam dados. “Não” na pergunta de evento não cria evento.
- [ ] 8.5 Fila de recusas, teste 18, e ação de reabilitar.
- [ ] 8.6 Saída: com chave válida, fechar um dia abre a conversa com o resumo e com “Tem algum evento novo para eu agendar?”. Descrever a TV e aceitar cria o evento de amanhã ao meio-dia, e não uma tarefa. Sem chave, o fechamento conclui e avisa.

### Fase 9 — WhatsApp

- [ ] 9.1 Webhook de verificação e de mensagens. Segredos só no servidor.
- [ ] 9.2 Vínculo pelo código em Ajustes. A tela avisa o custo da Meta antes de mostrar o código.
- [ ] 9.3 Mensagem do número vinculado entra na mesma conversa e pode aceitar sugestão, inclusive criar evento. Teste 25 para número estranho.
- [ ] 9.4 Ritual também é enviado no WhatsApp quando o vínculo existe: texto livre dentro da janela de 24 horas, modelo aprovado fora dela.
- [ ] 9.5 Áudio e imagem recebem pedido para escrever. Mensagem repetida não grava duas vezes.
- [ ] 9.6 Saída: do WhatsApp vinculado, a pessoa escreve o compromisso da TV, confirma, e o evento aparece no app. Do app, a mesma conversa mostra essa troca. Número não vinculado não cria nada.

### Fase 10 — Google Calendar

- [ ] 10.1 OAuth, guardar refresh token protegido no servidor, status de conexão.
- [ ] 10.2 Ligar, atualizar e remover lembrete de tarefa recorrente e de evento único. Minutos antes editáveis. Falha de API fica em `VinculoCalendario.Erro` e não desfaz o registro local.
- [ ] 10.3 Tela Calendário e seção de lembrete na tarefa e no evento.
- [ ] 10.4 Saída: a tarefa diária com lembrete de 15 minutos e o evento da TV ao meio-dia aparecem no Google Calendar. Desligar remove cada um.

### Fase 11 — Importação do Notion

- [ ] 11.1 Cliente somente leitura. Sem token, a tela explica o que falta.
- [ ] 11.2 Ler Schedule e escrever `docs/notion-schedule.md` com bancos, propriedades e um exemplo de obrigatória, um de rotina comum, um de objetivo e, se existir, um de item único. Parar aqui se o mapa da seção 9 não bater com o banco real, ajustar o mapa e só então seguir.
- [ ] 11.3 Pré-visualização: cada página com destino proposto (tarefa, evento, meta ou lista) e correção manual.
- [ ] 11.4 Confirmar a seleção. Segunda visita não duplica `NotionPageId` e não pisa campo `EditadoLocalmente`.
- [ ] 11.5 Saída: obrigatórias escolhidas aparecem na rotina ativa. Metas entram sem data, dentro de lista quando houver prazo. Evento entra no dia marcado. O resto da rotina aparece em Futuras. Uma reimportação não duplica.

### Fase 12 — PWA, offline e acabamento

- [ ] 12.1 Manifesto, ícones, service worker e instalação.
- [ ] 12.2 Fila offline de status no Hoje, ocorrência e evento, com a regra de conflito da seção 10.
- [ ] 12.3 Revisar todas as telas da seção 8 no celular estreito e no desktop: vazio, erro de rede, carregando, formulário de edição, conversa da Secretaria.
- [ ] 12.4 Textos restantes em português. Nenhum estado interno em inglês na interface (`Not Started`, `Done`, `Score`). O nome Secretaria Pessoal aparece na conversa, no botão e nas mensagens.
- [ ] 12.5 README com: como subir, como criar a conta, onde pôr as chaves, como instalar o PWA, como conectar Google, Notion e WhatsApp.
- [ ] 12.6 Saída: Lighthouse do Vite em preview marca o app como instalável. Concluir uma tarefa e um evento com a API desligada e, ao ligar de novo, o status chega no servidor se o item ainda estava aberto.

### Fase 13 — Roteiro real de uso

Executar uma vez, na ordem, com dados de verdade ou cópia reduzida do Schedule.

- [ ] 13.1 Criar a conta e ver as cinco categorias com os focos do seed.
- [ ] 13.2 Importar o Schedule. Conferir obrigatórias ativas, metas sem data e o restante da rotina em Futuras.
- [ ] 13.3 Montar a Lista de Metas para 2027, ou a lista até 2028, com pelo menos uma meta e uma submeta, nenhuma delas com data própria.
- [ ] 13.4 Criar o evento da TV para o dia seguinte ao meio-dia e vê-lo só naquele dia.
- [ ] 13.5 Concluir o dia. Ver pontuação, pontuação máxima, categorias e ofensiva, sem o evento na conta.
- [ ] 13.6 Resgatar a recompensa diária só quando as obrigatórias estiverem feitas.
- [ ] 13.7 Fechar o dia com a Secretaria ligada. Ver a pergunta do evento. Recusar uma futura. Na fala seguinte, ver outra futura se houver mais de uma.
- [ ] 13.8 Responder na conversa com um evento novo, aceitar e editar o resultado.
- [ ] 13.9 Vincular o WhatsApp, mandar uma frase de lá e ver a mesma troca no app.
- [ ] 13.10 Ligar um lembrete de tarefa e o do evento e ver os dois no Google Calendar.
- [ ] 13.11 Instalar o PWA, abrir Hoje por ele e abrir a Secretaria pelo botão flutuante.

## 12. Definição de pronto

A aplicação está completa quando a fase 13 fecha e isto é verdade:

- Tarefa só existe com repetição: dia, semana, mês, ano ou a cada N dessas unidades.
- Evento acontece uma vez, com data e hora, fora da ofensiva e fora da pontuação.
- Meta não tem data. Lista de metas tem prazo.
- Listas, metas e submetas aninhadas funcionam, inclusive meta futura.
- Ofensiva positiva e negativa seguem o exemplo da quinta-feira, com teste automático.
- Pontuação e pontuação máxima existem no geral e por categoria.
- Obrigatórias, normais e extras têm os efeitos da seção 2.
- Recompensas dos quatro horizontes e das metas são editáveis e resgatáveis.
- A Secretaria Pessoal conversa no app e no WhatsApp, sugere, pergunta se há evento novo ao fechar período ou lista, e só grava com aceite.
- Google Calendar recebe lembrete da rotina e do evento.
- Notion entra pela prévia, obrigatórias primeiro, objetivos como meta, item único como evento, o resto da rotina como futuro, sem duplicar.
- PWA instala e o Hoje conclui offline.
- Tudo o que a pessoa configura tem tela de edição.
- A interface está em português.

## 13. Acompanhamento

Marcar `[x]` neste arquivo na tarefa concluída. Não abrir fase nova com tarefa da fase atual em aberto, exceto bloqueio externo (chave Google, token Notion, token WhatsApp ou cota do Gemini). Esse bloqueio não impede as fases anteriores.
