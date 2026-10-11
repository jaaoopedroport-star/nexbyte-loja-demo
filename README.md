# NexByte Loja Demo

Loja demonstrativa e responsiva de eletrônicos, em português brasileiro, com catálogo fictício, busca, filtros, detalhe de produto, sacola persistente, cálculo do subtotal e Checkout Pro do Mercado Pago. O fluxo de checkout pode oferecer Pix e outros meios disponíveis na conta. A chave Pix do lojista não é usada como credencial da API.

## Rodar localmente

- Node.js 18 ou superior
- Uma aplicação criada em Mercado Pago Developers para obter credenciais de teste: https://www.mercadopago.com.br/developers/panel/app

1. Instale as dependências com npm install.
2. Copie .env.example para .env.
3. No painel Developers, abra a aplicação e copie o Access Token de teste para MP_ACCESS_TOKEN. O token de teste começa com APP_USR; não use o token de produção.
4. Inicie com npm start e abra http://localhost:4242.

Sem o token de teste, catálogo, filtros, produto, sacola e subtotal funcionam, mas a criação do checkout fica bloqueada com uma mensagem clara.

## Testar o checkout e Pix

1. No painel do Mercado Pago Developers, crie uma aplicação e encontre as credenciais em Credenciais de teste. O token de teste é uma credencial privada da aplicação; mantenha-o apenas no .env.
2. Crie uma conta de teste comprador no mesmo país da conta vendedora. O Checkout Pro usa o login dessa conta de teste para simular a compra.
3. Com o servidor em execução, adicione um produto, confira o subtotal e clique em Continuar para pagamento. O servidor cria a preferência a partir dos preços do catálogo no backend e redireciona ao Mercado Pago.
4. Use uma janela anônima e entre com o comprador de teste. No checkout, escolha um meio disponível. Segundo a documentação do Mercado Pago, o teste de Pix pode terminar com pagamento pendente, pois é um meio offline; cartões de teste permitem simular aprovação ou recusa.

Os pagamentos com credenciais de teste não disparam webhooks de pagamento automaticamente. Para validar a recepção de webhooks, configure uma URL HTTPS pública que encaminhe para /api/webhook, habilite o evento de pagamentos na aplicação do Mercado Pago e use a função Simular do painel. Adicione a URL em MP_WEBHOOK_URL e o segredo gerado em MP_WEBHOOK_SECRET. O servidor valida a assinatura e consulta o pagamento na API antes de registrar seu estado. Webhooks locais não ficam acessíveis ao Mercado Pago sem um túnel HTTPS.

Os itens, preços, avaliações, comunidade e frete são fictícios. Não use credenciais de produção nem a demonstração para vendas reais. Nenhum dado de cartão é recebido ou armazenado pela loja.

## Endpoints

- GET /api/products: catálogo demonstrativo.
- POST /api/checkout: valida produtos e quantidades no servidor e cria uma preferência de pagamento no Mercado Pago.
- POST /api/webhook: valida a assinatura Mercado Pago e consulta o status atual do pagamento.

Segredos ficam somente no .env, ignorado pelo Git. Nunca coloque tokens privados no JavaScript do navegador ou envie-os pelo chat.
