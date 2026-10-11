# Sistema web único na Vercel com Postgres no Neon, em São Paulo

O sistema é um único projeto web em Next.js com TypeScript, hospedado na Vercel (plano Pro, exigido para uso comercial, com as funções rodando em São Paulo). O banco é PostgreSQL no Neon, na região de São Paulo (aws-sa-east-1), e o login fica dentro do próprio sistema (better-auth, e-mail e senha). Escolhemos assim porque ninguém da clínica precisa manter servidor, os dados ficam no Brasil e o custo estimado é de R$ 135 a 160 por mês.

## Opções consideradas

- **Servidor alugado único**: de R$ 40 a 80 por mês, mas alguém teria de cuidar de atualizações de segurança e de cópias de segurança de dados de pacientes.
- **Planilhas com painel** (Google Planilhas e Looker Studio): quase sem custo, mas frágil para repasse, choque de sala e permissões.
- **Supabase no lugar do Neon**: cerca de R$ 240 por mês, pagando por um login pronto que não precisamos.
