# CryptoPay — Status do Projeto (em construção)

Este pacote contém o que já foi implementado do monorepo CryptoPay.
**Este NÃO é ainda o projeto completo** — é o progresso até a Fase 9 (Pricing)
de um total de 26 fases planejadas. Veja o checklist abaixo.

## ✅ Implementado até agora

### Infraestrutura do monorepo
- `package.json` raiz com npm workspaces
- `tsconfig.json` com path aliases (`@cryptopay/*`)
- `.gitignore` e `.env.example` (somente placeholders, nenhum valor real)

### `packages/database`
- Schema Prisma completo: User, UserProfile, RefreshToken, KycVerification,
  Wallet, WalletAsset, BlockchainTransaction, Quote, SellOrder, PixPayment,
  WebhookEvent, Integration, AuditLog, Notification
- Client singleton (`src/index.ts`)
- Seed script (cria apenas registros de integração inativos — sem dados
  financeiros fictícios)

### `packages/types`
- Tipos de domínio compartilhados (Balance, Price, Quote, PixPayment, etc.)

### `packages/blockchain`
- Interface `BlockchainProvider` (contrato comum, padrão Adapter)
- `BlockCypherProvider` — Bitcoin, via API real e documentada da BlockCypher
- `AlchemyProvider` — Ethereum/EVM, via JSON-RPC real da Alchemy
- `RpcProvider` — fallback genérico para redes EVM adicionais
- Factory que seleciona o provider certo por rede

### `packages/pricing`
- Interface `PricingProvider`
- `CoinGeckoProvider` — cotação real via API pública da CoinGecko

### `packages/wallet`
- Validação de endereços por rede (Bitcoin legacy/P2SH/Bech32, EVM)
- Parser de QR Code (endereço simples e URIs `bitcoin:`/`ethereum:`)

### `packages/kyc`, `packages/payments` (off-ramp), `packages/pix`
- Interfaces conforme especificação (`KycProvider`, `OffRampProvider`, `PixProvider`)
- Adapters **template** (REST + webhook HMAC) — pontos de extensão para quando
  um provedor real for contratado
- Factories que **lançam erro explícito** se as credenciais reais não
  estiverem configuradas no ambiente (nunca operam com dados fictícios)

## ⏳ Ainda não implementado (próximas fases)

- `packages/config` — validação centralizada de variáveis de ambiente (Zod)
- `apps/api` — Backend NestJS completo:
  - Módulo de autenticação (cadastro, login, JWT, refresh, RBAC)
  - WalletService, QuoteService, SellOrderService
  - Controllers REST + documentação Swagger/OpenAPI
  - Webhooks (`/webhooks/pix`, `/webhooks/offramp`, `/webhooks/kyc`)
  - Máquina de estados de transação
  - Painel administrativo (`/admin`)
- `apps/web` — Frontend Next.js (todas as telas do fluxo)
- `apps/mobile` — Empacotamento Android via Capacitor
- Testes (Vitest, Jest, Playwright E2E)
- `docker-compose.yml` e `Dockerfile`
- GitHub Actions (`.github/workflows/ci.yml`, `deploy.yml`)
- `netlify.toml`
- `docs/` (architecture.md, api.md, security.md, etc.)

## Como continuar

Este projeto está sendo construído fase a fase, conforme a especificação
original determina (documento não deve ser gerado inteiro de uma vez).
Peça para eu continuar e seguirei para o backend NestJS (`apps/api`),
que é o próximo bloco.

## Importante — regras de segurança já seguidas

- Nenhuma seed phrase ou chave privada é solicitada ou armazenada em
  nenhum lugar do código.
- Nenhuma credencial real está presente — apenas placeholders em `.env.example`.
- Os módulos de KYC, Off-Ramp e Pix falham explicitamente (erro claro) se
  não configurados com credenciais reais — não há simulação de aprovação
  ou pagamento fictício em nenhum desses módulos.
