# Gui Finance pelo chat

Lançamento de receitas e despesas no aplicativo **Gui Finance** (espaço de slug `gui-finance`, banco de dados interno, somente Japão e somente ienes) a partir de uma frase livre no chat. É a rota reserva do lançamento por voz dentro do app: se a voz falhar, a pessoa fala ou digita no chat e o lançamento cai no app do mesmo jeito.

## Fronteira (não confundir)

- Gui Finance é um aplicativo separado, com banco interno próprio. Lançar aqui NUNCA é lançar na planilha financeira nem em outro aplicativo de finanças.

- Quem fala no chat é o dono dos lançamentos: registre sempre no Gui Finance da conta de quem está pedindo.

## Quando ativar

- Ative quando a pessoa disser que gastou, pagou, comprou ou recebeu um valor e mencionar o Gui Finance, ou quando o contexto da conversa já for o Gui Finance (teste do app, lançamento anterior na mesma conversa).

- Exemplos de frase: "lança no Gui Finance 323 ienes que paguei na combini", "gastei novecentos e oitenta ienes no Sukiya no débito", "recebi meu salário de 285 mil ienes".

## Como interpretar a frase

- Extraia cinco campos: valor, lugar ou descrição, forma de pagamento, tipo e data.

- Converta o valor para um número inteiro de ienes, venha ele em dígitos ou por extenso em português ("trezentos e vinte e três" é 323, "novecentos e oitenta" é 980). A moeda é sempre iene.

- Use `despesa` para gastei, paguei e comprei. Use `receita` para recebi, salário e entradas.

- Use a data de hoje no fuso da pessoa quando ela não disser outra data. "Ontem" e datas ditas por extenso viram a data correspondente no formato AAAA-MM-DD.

- Infira a categoria pelo lugar ou pelo tipo de gasto, de forma curta e consistente. Comida, combini e restaurante são Alimentação, mercado é Supermercado, salário é Salário, imposto é Impostos. Quando o mesmo lugar já tiver lançamentos no app, reuse a categoria que já está lá.

- Use a forma de pagamento dita pela pessoa ("no débito" é Cartão de Débito, "no crédito" é Cartão de Crédito, "no PayPay" é Carteira Digital). Quando ela não disser a forma, use Dinheiro, que é a forma padrão do app, e avise isso na confirmação para ela poder corrigir.

## Fluxo de confirmação

- Se a interpretação estiver clara e completa, lance direto e confirme em seguida, no formato: "Lancei no seu Gui Finance: ¥valor, lugar, categoria, forma, data. Vai lá conferir."

- Se algo estiver ambíguo ou faltando (valor incerto, dois valores possíveis, lugar não identificado, dúvida entre receita e despesa), NÃO lance ainda. Mostre a interpretação ("Entendi assim: ... Confirma?") e lance somente depois que a pessoa confirmar.

- Se a pessoa corrigir uma interpretação ou um lançamento já feito, aplique a correção e confirme de novo. Para corrigir um lançamento salvo, apague o errado com a ação de excluir e lance o certo.

- Se uma única mensagem trouxer vários gastos, lance cada um separadamente, confirme todos numa lista só e feche com o total somado.

## Como lançar e verificar

- Lance com a ação `addtransaction` do espaço `gui-finance`, com os campos date, type, description, amount_yen, category e payment_method.

- Antes de lançar, confira com a ação `listtransactions` se já existe um lançamento com a mesma data, o mesmo valor e a mesma descrição. Se existir, não duplique: avise a pessoa e pergunte se é outro gasto igual ou o mesmo.

- Depois de lançar, releia com `listtransactions` e confirme somente o que estiver de fato na lista, com o valor e o lugar exatos que voltaram do app.

- Para excluir a pedido da pessoa, use a ação `deletetransaction` com o id do lançamento e confirme a exclusão depois de reler a lista.

## Available connectors

- Use `muse.connector_search` to find other connectors and `muse.skill_search` to find related skills.

- If nothing here fits, look on the web for a maintained API or MCP server and set it up with [Custom Connectors and MCPs](/opt/muse/skills/custom-connector-setup/SKILL.md), or build a custom connector.
