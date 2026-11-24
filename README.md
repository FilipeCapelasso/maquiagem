# Projeto Maquiagem

Site de agendamento para estúdio de maquiagem: vitrine para as clientes e área da gerência, num único `index.html`.

- **Vitrine:** abra `index.html`
- **Gerência:** abra `index.html#/admin`. No primeiro acesso, crie a senha.

## Banco de dados

Usa o mesmo projeto Supabase do Honda (`vendashonda`), mas **totalmente separado**:

- Tudo fica no schema próprio `maquiagem` (config, dias_semana, horario_semana, excecoes, servicos, agendamentos, cursos, portfolio, depoimentos, sessoes, login_falhas).
- O schema não é exposto na API. RLS está ligado e não há políticas, então ninguém lê as tabelas diretamente.
- O site só conversa com o banco pelas funções `public.mq_*`. Elas só leem e escrevem no schema `maquiagem`.
- A gerência tem senha própria (bcrypt). Não usa o login do Supabase/Honda.
- Nenhuma tabela ou função do Honda é tocada.

Migrations: `maquiagem_01_schema_isolado` a `maquiagem_05_search_path`.

## Regras de horário

- Cada turno da semana é a faixa em que um atendimento pode **começar**. Isso pode ser trocado em Horários, em "O fim do turno é".
- Padrão: segunda a sábado, das 08:00 às 13:00 e das 15:00 às 16:00, com horários a cada 30 min.
- Durações: Básica 1h30, Completa 2h30, Noiva/Casamento 2h30.
- Nunca há sobreposição de atendimentos: a verificação é feita no banco, com trava contra duas reservas ao mesmo tempo.
- Na gerência dá para fechar um dia, bloquear um horário, abrir um horário especial e aplicar folga em vários dias de uma vez.
