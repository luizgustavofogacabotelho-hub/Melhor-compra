# Melhor Compra v2 — base de loja conectada

Esta versão evolui o MVP com:
- Next.js App Router + TypeScript
- Supabase Auth e PostgreSQL
- Catálogo consultado no banco
- Área administrativa com verificação de usuário/role no servidor
- API de criação de preferência de pagamento Mercado Pago (esqueleto seguro)
- Webhook de pagamento com validação de assinatura configurável
- SQL com tabelas, RLS e políticas iniciais
- Variáveis de ambiente documentadas

## Pré-requisitos
- Node.js LTS
- Conta Supabase
- Conta de vendedor Mercado Pago para testar pagamentos
- Hospedagem HTTPS para publicar

## Instalação
```bash
npm install
cp .env.example .env.local
npm run dev
```
Configure `.env.local` com suas credenciais. Nunca compartilhe ou publique `.env.local`.

## Supabase
1. Crie um projeto no Supabase.
2. Abra SQL Editor e execute `supabase/migrations/001_initial.sql`.
3. Crie seu usuário em Authentication > Users.
4. Promova seu usuário a administrador usando o SQL abaixo, substituindo pelo UUID real:
```sql
update public.profiles set role = 'admin' where id = 'UUID_DO_SEU_USUARIO';
```
Apenas faça isso para sua própria conta de administrador.

## Mercado Pago
Configure `MERCADOPAGO_ACCESS_TOKEN` e `MERCADOPAGO_WEBHOOK_SECRET` no ambiente do servidor. Use credenciais de teste primeiro. O token nunca deve aparecer em código do navegador.
O endpoint de preferência recebe `items` como IDs e quantidades, consulta preços no banco e calcula o total no servidor. Antes de produção, configure no painel do Mercado Pago a URL pública do webhook e confirme o formato de assinatura vigente na documentação da sua conta.

## Scripts
```bash
npm run dev
npm run build
npm run start
```

## Importante antes de produção
- Preencha os dados legais/contato e políticas da loja.
- Configure domínio HTTPS, backups e monitoramento.
- Teste RLS com contas de cliente e administrador diferentes.
- Faça testes de pagamento em sandbox.
- Ajuste frete, impostos, estoque, devoluções, LGPD e fluxo de fulfillment.
- O webhook precisa ser testado contra o formato de assinatura vigente do Mercado Pago antes de habilitar pagamentos reais.
- O projeto é um scaffold técnico, não uma loja já publicada nem uma conta de pagamento configurada.
