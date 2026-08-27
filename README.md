# Vertex-Systems
This project was developed from AI-generated fictional requirements designed to simulate real-world business scenarios and improve my programming, problem-solving, and software engineering skills.


# DEMANDA TÉCNICA #2026-001

**Projeto:** Sistema de Gestão de Tarefas Internas
**Cliente:** Prisma Contabilidade Ltda. — Sorocaba/SP
**Contato do cliente:** Márcia Toledo (sócia-administradora)
**Desenvolvedor responsável:** Renan Barros
**Tech Lead:** (mentoria)
**Data de abertura:** 27/08/2026
**Prioridade:** Alta
**Status:** Aguardando início da Entrega 1

---

## 1. E-mail original do cliente

> **De:** marcia.toledo@prismacontabil.com.br
> **Assunto:** Sistema pra organizar as tarefas do escritório
>
> Oi, tudo bem?
>
> Fui indicada pra você pelo pessoal do Rotary. A gente tem um escritório de contabilidade com 8 funcionários e a bagunça aqui tá insustentável.
>
> Hoje funciona assim: eu passo as demandas no WhatsApp, o pessoal anota em post-it e caderninho, e no fim do mês sempre some alguma coisa. Mês passado esquecemos uma entrega de obrigação acessória de um cliente e tomamos multa. Não dá mais.
>
> Eu já testei Trello, mas o pessoal aqui é mais velho, achou complicado, ninguém usou. Eu quero uma coisa **simples**, que abra no navegador, que a menina da recepção consiga usar sem treinamento.
>
> O que eu preciso: cadastrar a tarefa, dizer de que área ela é (fiscal, contábil, pessoal, etc), colocar o prazo, e ir dando baixa conforme faz. E eu preciso enxergar rapidinho o que tá atrasado, porque atraso aqui vira multa.
>
> Ah, e não pode perder as informações quando fechar o navegador. Isso é básico né?
>
> Não tenho orçamento pra servidor nem essas coisas agora. Se der pra rodar no computador mesmo, tá ótimo pra começar.
>
> Quanto tempo você precisa?
>
> Abraço,
> Márcia

---

## 2. Reunião de alinhamento — perguntas e respostas

Como o briefing veio incompleto (como sempre vem), foram levantados os seguintes pontos com a cliente:

| # | Pergunta feita ao cliente | Resposta da Márcia |
|---|---|---|
| 1 | Cada funcionário terá login próprio? | Não nessa primeira versão. O computador da sala é compartilhado. Login fica pra depois. |
| 2 | Precisa saber **quem** criou a tarefa? | Por enquanto não. Só o que é a tarefa e pra quando. |
| 3 | Uma tarefa pode ter mais de uma categoria? | Não. Uma categoria só, senão confunde. |
| 4 | Toda tarefa precisa ter prazo? | Não. Tem coisa que é "quando der". Mas a maioria tem. |
| 5 | O que exatamente é "atrasado"? | Prazo já passou e a tarefa não foi concluída. |
| 6 | Tarefa concluída some da tela? | Não! Ela quer poder consultar o que já foi feito. |
| 7 | Precisa apagar tarefa? | Sim, "quando alguém digita errado ou o cliente cancela o serviço". |
| 8 | Precisa editar tarefa? | Sim, "porque prazo de cliente muda toda hora". |
| 9 | Quantas tarefas por dia, mais ou menos? | "Umas 30, 40 no mês fechado." |
| 10 | Precisa relatório / exportação? | Não agora. Ela quer "ver na tela e pronto". |

**Conclusão do levantamento:** escopo pequeno, uso local, sem backend, sem autenticação. Persistência no próprio navegador é suficiente e atende a restrição orçamentária.

---

## 3. Objetivo do projeto

Substituir o controle por post-it e WhatsApp por uma aplicação web local que permita cadastrar, acompanhar e concluir tarefas do escritório, com **destaque visual imediato para tarefas atrasadas**, de forma que nenhuma obrigação com prazo seja perdida novamente.

**Métrica de sucesso do cliente:** zero prazos perdidos por esquecimento nos próximos 3 meses.

---

## 4. Usuários do sistema

- **Márcia (administradora):** cadastra a maior parte das tarefas, quer visão geral e prazos.
- **Equipe (7 pessoas, 35 a 58 anos):** consultam o que precisam fazer e dão baixa. Baixa familiaridade com tecnologia.

**Implicação direta no design:** interface óbvia, textos grandes, poucos cliques, nenhum ícone sem rótulo, nenhuma ação destrutiva sem confirmação.

---

## 5. Escopo funcional

Cada requisito abaixo tem **critérios de aceite**. Um requisito só é considerado entregue quando **todos** os critérios passam.

### RF-01 — Estrutura de dados e renderização da lista

**Descrição:** o sistema deve manter as tarefas em memória e renderizá-las na tela a partir dessa estrutura.

**Comportamento esperado:**
- A lista exibida na tela é sempre um reflexo fiel do estado em memória.
- Nenhuma tarefa é escrita manualmente no HTML.

**Critérios de aceite:**
- [ ] Existe uma única fonte de verdade com as tarefas.
- [ ] Existe uma função responsável por renderizar a lista completa.
- [ ] A função limpa o container antes de desenhar, sem duplicar itens ao ser chamada várias vezes.
- [ ] Cada tarefa possui um identificador único que **não** depende da posição no array.
- [ ] O `<ul>` (ou container equivalente) nasce vazio no HTML.

---

### RF-02 — Cadastro de tarefa

**Descrição:** o usuário deve conseguir cadastrar uma nova tarefa pela interface.

**Campos do formulário:**

| Campo     |  Tipo   | Obrigatório | Regras             |
|---|---|---|---|
| Descrição | texto   | Sim         | 3 a 120 caracteres |
| Categoria | seleção | Sim         | Fiscal, Contábil, Pessoal, Societário, Financeiro, Outros |
| Prazo     | data    | Não         | Se preenchido, não pode ser anterior a hoje |

**Critérios de aceite:**
- [ ] A tarefa aparece na lista imediatamente após o cadastro, sem recarregar a página.
- [ ] O formulário é limpo após o cadastro bem-sucedido.
- [ ] O foco retorna ao campo de descrição após o cadastro.
- [ ] A página **não recarrega** ao enviar o formulário.
- [ ] Toda tarefa nasce com status "pendente".
- [ ] Toda tarefa registra a data/hora de criação.

---

### RF-03 — Validações de cadastro

**Descrição:** entradas inválidas devem ser bloqueadas com feedback claro.

**Critérios de aceite:**
- [ ] Descrição vazia ou só com espaços é rejeitada.
- [ ] Descrição com menos de 3 caracteres é rejeitada.
- [ ] Descrição com mais de 120 caracteres é rejeitada.
- [ ] Categoria não selecionada é rejeitada.
- [ ] Prazo no passado é rejeitado.
- [ ] A mensagem de erro é exibida **na tela**, próxima ao campo. Uso de `alert()` é reprovado.
- [ ] A mensagem some quando o usuário corrige o campo.
- [ ] Espaços em excesso no início e no fim da descrição são removidos antes de salvar.

---

### RF-04 — Concluir / reabrir tarefa

**Descrição:** o usuário marca a tarefa como concluída e pode desfazer.

**Critérios de aceite:**
- [ ] Um clique alterna entre pendente e concluída.
- [ ] Tarefa concluída recebe destaque visual distinto (ex.: texto riscado e opacidade reduzida).
- [ ] Tarefa concluída **permanece** na lista.
- [ ] Tarefa concluída registra a data/hora da conclusão.
- [ ] Tarefa concluída **nunca** é exibida como atrasada, mesmo com prazo vencido.

---

### RF-05 — Excluir tarefa

**Critérios de aceite:**
- [ ] Existe ação de exclusão em cada tarefa.
- [ ] A exclusão exige confirmação explícita do usuário.
- [ ] Após excluir, a lista e os contadores são atualizados.
- [ ] A exclusão remove a tarefa correta mesmo com filtro ou busca ativos.

---

### RF-06 — Editar tarefa

**Descrição:** prazos e descrições mudam com frequência; o usuário precisa corrigir sem recadastrar.

**Critérios de aceite:**
- [ ] É possível editar descrição, categoria e prazo.
- [ ] As mesmas validações do RF-03 se aplicam à edição.
- [ ] É possível cancelar a edição sem alterar os dados.
- [ ] O identificador e a data de criação da tarefa **não** mudam após a edição.

---

### RF-07 — Contadores

**Descrição:** painel com números-resumo no topo da tela.

**Critérios de aceite:**
- [ ] Exibe total de tarefas.
- [ ] Exibe total de pendentes.
- [ ] Exibe total de concluídas.
- [ ] Exibe total de atrasadas.
- [ ] Os contadores refletem **todas** as tarefas, independentemente do filtro ou da busca ativos.
- [ ] Atualizam automaticamente após qualquer operação.

---

### RF-08 — Filtros por status

**Critérios de aceite:**
- [ ] Filtros disponíveis: Todas, Pendentes, Concluídas, Atrasadas.
- [ ] O filtro ativo é visualmente identificável.
- [ ] O filtro **não** apaga tarefas — apenas altera o que é exibido.
- [ ] Filtro e busca funcionam de forma combinada (aplicados simultaneamente).
- [ ] Quando nenhum resultado é encontrado, exibe mensagem de estado vazio.

---

### RF-09 — Busca por texto

**Critérios de aceite:**
- [ ] A busca ocorre conforme o usuário digita.
- [ ] A busca ignora diferença entre maiúsculas e minúsculas.
- [ ] A busca ignora acentuação (buscar "obrigacao" encontra "obrigação").
- [ ] Espaços no início e fim do termo são desconsiderados.
- [ ] A busca não interfere nos contadores.

---

### RF-10 — Categorias

**Critérios de aceite:**
- [ ] A categoria é exibida em cada tarefa como etiqueta visual.
- [ ] Cada categoria tem cor própria e consistente.
- [ ] É possível filtrar por categoria, combinando com status e busca.
- [ ] A lista de categorias é definida em um único lugar no código, não repetida no HTML e no JS.

---

### RF-11 — Prazos e alertas visuais

**Regras de exibição (avaliadas apenas para tarefas pendentes):**

| Situação | Exibição |
|---|---|
| Prazo já passou | "Atrasada há X dia(s)" — destaque vermelho |
| Prazo é hoje | "Vence hoje" — destaque laranja |
| Prazo em até 3 dias | "Vence em X dia(s)" — destaque amarelo |
| Prazo acima de 3 dias | Data formatada em DD/MM/AAAA — neutro |
| Sem prazo | "Sem prazo" — neutro |

**Critérios de aceite:**
- [ ] A comparação de datas ignora horas, minutos e segundos.
- [ ] Uma tarefa com prazo hoje **não** é classificada como atrasada.
- [ ] Datas são exibidas no formato brasileiro DD/MM/AAAA.
- [ ] Tarefas concluídas não exibem alerta de atraso.

---

### RF-12 — Ordenação

**Critérios de aceite:**
- [ ] Ordenações disponíveis: mais recentes primeiro, prazo mais próximo primeiro, ordem alfabética.
- [ ] Tarefas sem prazo aparecem **por último** na ordenação por prazo.
- [ ] A ordenação não altera permanentemente a estrutura de dados original de forma destrutiva e imprevisível.
- [ ] Ordenação funciona em conjunto com filtros e busca.

---

### RF-13 — Persistência

**Descrição:** requisito explícito do cliente ("não pode perder as informações quando fechar o navegador").

**Critérios de aceite:**
- [ ] Os dados sobrevivem ao fechamento e reabertura do navegador.
- [ ] Ao abrir a aplicação pela primeira vez (sem dados salvos), o sistema não quebra.
- [ ] Se os dados salvos estiverem corrompidos, o sistema trata o erro e não trava a tela.
- [ ] O salvamento ocorre automaticamente, sem botão "salvar".
- [ ] A leitura e a escrita dos dados estão isoladas em funções próprias, não espalhadas pelo código.

---

## 6. Regras de negócio

- **RN-01:** toda tarefa nasce com status pendente.
- **RN-02:** uma tarefa pertence a exatamente uma categoria.
- **RN-03:** prazo é opcional; quando informado, não pode ser retroativo no cadastro.
- **RN-04:** tarefa concluída nunca é considerada atrasada.
- **RN-05:** "atrasada" = pendente **e** prazo anterior à data de hoje.
- **RN-06:** exclusão é permanente e exige confirmação.
- **RN-07:** o identificador da tarefa é imutável durante todo o ciclo de vida.
- **RN-08:** contadores consideram o universo total de tarefas, nunca o conjunto filtrado.

---

## 7. Requisitos não funcionais

- **RNF-01 — Usabilidade:** utilizável sem treinamento por pessoa com baixa familiaridade digital.
- **RNF-02 — Acessibilidade:** todos os campos com `<label>` associado; navegação por teclado funcional; contraste adequado.
- **RNF-03 — Responsividade:** funcional em desktop (1366px+) e em celular (360px+). A Márcia vai abrir no celular, mesmo tendo dito que não.
- **RNF-04 — Performance:** a interface não pode travar com 500 tarefas cadastradas.
- **RNF-05 — Robustez:** nenhum erro não tratado no console em uso normal.
- **RNF-06 — Idioma:** toda a interface em português do Brasil.

---

## 8. Restrições técnicas (obrigatórias)

- HTML5, CSS3 e JavaScript (ES6+) **puros**.
- Proibido: React, Vue, jQuery, Bootstrap, Tailwind ou qualquer biblioteca externa.
- Proibido: `alert()`, `confirm()` e `prompt()` como interface final de produção.
- Proibido: `innerHTML` com concatenação de dados digitados pelo usuário (risco de XSS — será cobrado no review).
- Proibido: variáveis com `var`.
- Obrigatório: separação em `index.html`, `style.css` e `script.js`.
- Obrigatório: código e nomes de variáveis/funções em português **ou** inglês — mas com **padrão único** no projeto inteiro.

---

## 9. Fora do escopo desta versão

- Login e controle de usuários
- Backend, API e banco de dados
- Atribuição de tarefas a funcionários específicos
- Notificações por e-mail ou WhatsApp
- Relatórios e exportação
- Anexos de arquivos
- Subtarefas e comentários

> Registrado para negociação futura como **v2**.

---

## 10. Entregáveis

1. Código-fonte em três arquivos (`index.html`, `style.css`, `script.js`).
2. `README.md` explicando o que é o projeto, como rodar e quais funcionalidades existem.
3. Repositório no GitHub com commits descritivos (um commit por entrega, no mínimo).
4. Deploy público funcionando (GitHub Pages ou Vercel).

---

## 11. Cronograma de entregas

Cada entrega passa por code review antes de liberar a próxima. **Não se avança com requisito reprovado.**

| Entrega | Requisitos | Tema |
|---|---|---|
| 1 | RF-01 | Estrutura de dados e renderização |
| 2 | RF-02, RF-03 | Cadastro e validações |
| 3 | RF-04, RF-05 | Concluir e excluir |
| 4 | RF-06 | Edição |
| 5 | RF-07 | Contadores |
| 6 | RF-08, RF-09 | Filtros e busca |
| 7 | RF-10 | Categorias |
| 8 | RF-11 | Prazos e alertas |
| 9 | RF-12 | Ordenação |
| 10 | RF-13 | Persistência |
| 11 | — | Refatoração final, README e deploy |

---

## 12. Definition of Done

Uma entrega só é aceita quando:

1. Todos os critérios de aceite do requisito estão marcados.
2. Nenhum erro aparece no console do navegador.
3. Não há código duplicado que poderia ser uma função.
4. Não há código comentado esquecido nem `console.log` de depuração.
5. Nomes de variáveis e funções são autoexplicativos.
6. Funções fazem uma coisa só.
7. A entrega não quebrou nada que já funcionava.

---

## 13. O que será avaliado no code review

- Separação entre **dados**, **lógica de negócio** e **manipulação do DOM**
- Uso correto de `const` / `let`
- Uso de métodos de array (`map`, `filter`, `find`, `some`, `reduce`) no lugar de loops manuais quando apropriado
- Imutabilidade onde faz sentido
- Delegação de eventos em vez de listener por item
- Tratamento de erros
- Segurança contra XSS
- Semântica do HTML
- Organização e legibilidade do CSS
- Ausência de duplicação (DRY)
- Funções pequenas e com responsabilidade única (SRP)

---

## 14. ENTREGA ATUAL — Entrega 1 (RF-01)

**Situação:** você acabou de assinar o contrato. Nada existe ainda.

**O que precisa ser feito agora:**

1. Criar a estrutura de arquivos do projeto.
2. Modelar como uma tarefa é representada em memória — decidindo os campos considerando **todos** os requisitos deste documento, não só o RF-01.
3. Criar um array com 3 tarefas de exemplo para validar a renderização.
4. Escrever a função de renderização da lista.
5. Aplicar um CSS mínimo, apenas para legibilidade.

**Decisões que você precisa tomar e justificar antes de codar:**

- Tarefa é string ou objeto? Por quê?
- Quais campos a tarefa precisa ter **agora**, sabendo o que vem nas entregas 2 a 10?
- Como gerar o identificador único? Por que o índice do array é uma péssima escolha?
- Onde a lista de tarefas deve viver no código?
- Por que a função de renderização precisa limpar o container antes de desenhar?

**Entregar ao Tech Lead:** os três arquivos + as respostas às cinco perguntas acima.

---

**Documento gerado para fins de mentoria técnica. Cliente e demanda fictícios, formato baseado em briefings reais de mercado.**