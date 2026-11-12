# SPEC — Gui Finance (instruções de construção)

Estas instruções são para o Muse que vai construir o Gui Finance na conta de quem pediu. Construa o aplicativo exatamente como descrito aqui, como um espaço/aplicativo com banco de dados interno, na barra lateral, com o nome **Gui Finance** e o slug `gui-finance`.

## Produto

- Aplicativo de finanças pessoais para uso no Japão, moeda única: iene (¥).
- Interface em português, limpa e direta, sem textos explicativos sobre o próprio aplicativo (nada de "prévia", "somente ¥", "vale para todas as abas" ou frases de ajuda na tela). Menos é mais.
- O banco de dados começa **vazio**: nenhum lançamento de exemplo, nenhum dado de outra pessoa.
- Três abas numa barra inferior: **Lançar**, **Lançamentos** e **Estatísticas**.

## Modelo de dados

Tabela interna de lançamentos, um registro por lançamento:

- `id`: identificador gerado pelo app.
- `date`: data no formato AAAA-MM-DD (data real, não texto livre).
- `type`: `receita` ou `despesa`.
- `description`: lugar ou descrição curta (ex.: Sukiya, Combini, Mercado).
- `amount_yen`: valor inteiro em ienes, maior que zero.
- `category`: categoria curta (ex.: Alimentação, Supermercado, Salário, Impostos, Transporte, Saúde, Lazer, Compras, Outros).
- `payment_method`: forma de pagamento (lista abaixo).

## Ações que o app publica

- `addtransaction`: recebe date, type, description, amount_yen, category e payment_method; salva um lançamento e devolve o id. É a ação que a skill de chat usa.
- `listtransactions`: devolve todos os lançamentos salvos, do mais novo para o mais antigo.
- `deletetransaction`: recebe um id e apaga esse lançamento.

## Aba Lançar (entrada principal: voz)

- Um botão de microfone grande no centro é a entrada principal.
- Ao tocar, o app usa o reconhecimento de voz do navegador em português (pt-BR) e mostra o estado "Ouvindo…".
- A fala é interpretada: valor em dígitos ou por extenso, lugar virando descrição e categoria sugerida, e a forma de pagamento quando dita ("no débito" = Cartão de Débito, "no crédito" = Cartão de Crédito, "no PayPay" = Carteira Digital).
- Antes de salvar, o app mostra um cartão com o lançamento interpretado e dois caminhos: **Confirmar** (salva) ou **Corrigir** (volta ao formulário manual já preenchido). Nada é salvo sem confirmação.
- Se o reconhecimento falhar, o erro aparece escrito na tela com o motivo (microfone bloqueado, sem conexão, nada ouvido) — nunca falhar em silêncio. Como caminho alternativo, o campo de texto do formulário manual aceita o ditado do teclado do telefone (microfone do próprio teclado), e o texto ditado passa pelo mesmo interpretador.
- Logo abaixo, separado por um divisor discreto, fica o formulário manual: data (hoje por padrão), tipo (despesa por padrão), descrição, valor, categoria e forma de pagamento com **Dinheiro pré-selecionado**.
- Ao salvar (voz confirmada ou manual), o lançamento entra no topo da lista e os totais e o saldo se atualizam na hora.

## Formas de pagamento

- Dinheiro (padrão), Cartão de Débito, Cartão de Crédito, Carteira Digital, Transferência Bancária, Cashback, Voucher, Pontos, Outros.
- Nada de Pix ou Boleto: é um livro só em ienes, do Japão.

## Aba Lançamentos

- Lista de todos os lançamentos salvos, do mais novo para o mais antigo, com data, descrição, categoria, forma e valor (despesa com sinal de menos, receita com sinal de mais).
- Busca por texto na descrição ou categoria.
- Totais do mês atual: entradas, saídas e saldo do período.
- Excluir um lançamento pela lista, com a lista e os totais atualizando em seguida.

## Aba Estatísticas

- Gráficos calculados somente dos lançamentos salvos (nunca de dados de exemplo):
  - Despesas por categoria (barras).
  - Entradas, saídas e saldo do fluxo.
  - Evolução mensal dos últimos meses.
  - Despesas por forma de pagamento.
- Sem lançamentos, a aba mostra o estado vazio, sem números inventados.

## Olho de privacidade (global)

- Um botão de olho no cabeçalho controla **um estado único** para as três abas: bloqueou ou desbloqueou em qualquer aba, vale para todas.
- O app abre com os valores **ocultos** (¥•••).
- O bloqueio cobre todos os números da interface: saldos, totais, itens da lista **e os valores e rótulos dos gráficos** das Estatísticas. Nada vaza com o olho fechado.

## Tema

- Botão de sol/lua no cabeçalho alterna tema **noite** (escuro) e **dia** (claro), nas três abas.

## Skill de lançamento pelo chat (instalar junto)

- A pasta `skill/gui_finance/` deste repositório contém uma skill (SKILL.md e skill.yaml).
- Ao construir o app, salve essa skill nas skills personalizadas da conta, para o dono poder lançar falando com o Muse no chat ("gastei X em Y, paguei de tal forma"), como reserva quando a voz do app falhar.
- A skill lança usando a ação `addtransaction` deste app, confere duplicata por data + valor + descrição antes de lançar, rele a lista depois de lançar e só confirma o que estiver salvo.
- Fluxo da skill: interpretação clara lança direto e confirma resumido ("Lancei no seu Gui Finance: ¥…, vai lá conferir"); interpretação ambígua mostra o entendimento e pergunta antes de lançar.

## Conferência antes de entregar

- Lançamento manual salva, aparece no topo da lista e mexe nos totais.
- Excluir remove o item e os totais voltam.
- Olho fechado esconde valores nas três abas, incluindo os gráficos; abrir o app já começa oculto.
- Voz: o cartão de confirmação mostra o lançamento interpretado e só salva no Confirmar; erro de voz mostra o motivo na tela.
- Estatísticas mostram números somente dos lançamentos salvos.
- Banco começa vazio e a skill `gui_finance` aparece na busca de skills da conta.
