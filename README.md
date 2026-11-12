# Gui Finance

Aplicativo de finanças pessoais para quem mora no Japão: somente ienes (¥), lançamento por voz como entrada principal, formulário manual logo abaixo, banco de dados interno e estatísticas calculadas dos lançamentos salvos.

Este repositório **não é um app para baixar e instalar** como numa loja. Ele é o pacote de construção do Gui Finance para o **Muse**: quem recebe o link pede ao próprio Muse para criar o aplicativo na sua conta, e o Muse reconstrói o app a partir destas instruções, com o banco de dados **vazio**, pronto para os dados de quem está instalando.

## Como "instalar" (para o dono do app)

1. Abra o **seu Muse** (app no telefone ou muse.ai) e comece uma conversa nova.
2. Cole o link deste repositório e peça, com as suas palavras, algo como:

   > Cria o aplicativo Gui Finance para mim a partir deste repositório: segue o SPEC.md para construir o app e salva a skill da pasta `skill/` para eu poder lançar pelo chat também.

3. O seu Muse constrói o aplicativo na **sua** conta (na barra lateral), cria o banco interno **sem nenhum lançamento** e salva a skill de lançamento pelo chat.
4. A partir daí você usa de três jeitos:
   - **Voz dentro do app**: toque no microfone e fale o gasto ("Sukiya, 980 ienes, no débito"); confira o cartão de confirmação e salve.
   - **Formulário manual** logo abaixo da voz, dentro do app.
   - **Pelo chat com o Muse** (reserva, caso a voz falhe): diga "gastei novecentos e oitenta ienes no Sukiya, no débito" e o Muse lança no app e confirma aqui.

## O que tem neste repositório

- `SPEC.md` — a especificação completa do aplicativo: telas, comportamento, modelo de dados, ações e a lista de conferência para a construção sair fiel.
- `skill/gui_finance/` — a skill que ensina o Muse a lançar pelo chat (frase livre vira lançamento no app; confirmação resumida depois de lançar; pergunta antes só quando a interpretação for ambígua).
- `referencia/gui-finance-referencia.html` — uma implementação de referência em um único arquivo HTML (dados no navegador, só para consulta visual e de comportamento). Ela **não** substitui o app de verdade: o app construído pelo Muse usa banco de dados interno próprio, conforme o SPEC.md.

## Privacidade

Este repositório é público e **não contém nenhum dado pessoal nem nenhum lançamento real**: nenhum valor, nome ou gasto de ninguém. Os lançamentos de cada pessoa ficam somente no aplicativo da conta dela.

## Resumo do aplicativo

- Três abas: **Lançar**, **Lançamentos** e **Estatísticas**.
- Moeda única: iene (¥). País: Japão.
- Olho de privacidade **global**: um estado só para as três abas, valores ocultos ao abrir o app, e o bloqueio vale também para os gráficos.
- Tema **dia/noite** no cabeçalho.
- Forma de pagamento padrão: **Dinheiro**.
- Interface limpa, sem textos explicativos sobre o próprio app: menos é mais.
