---
title: Termos de Uso da API
effective_date: 2025-08-22
---

# Termos de Uso da API

**Última atualização:** 22/08/2025

Bem-vindo! Estes Termos regulam o acesso e o uso da nossa **API Lotes**. Ao utilizar a API, você concorda com as condições abaixo.

## 1. Definições
- **API**: Interface de programação disponibilizada para consulta de indicadores.
- **Token JWT**: Token de autenticação no padrão *JSON Web Token*.
- **Chamada**: Uma requisição HTTP feita a qualquer endpoint exposto.
- **Ambiente de Testes**: Instância destinada a validação e homologação.
- **Ambiente de Produção**: Instância destinada a uso operacional.

## 2. Autenticação e Autorização
- O acesso é realizado via endpoint `/api/login`, mediante **credenciais** do usuário cadastrado.
- Em sucesso, é retornado um **token JWT** com prazo de expiração (`expiresIn`).
- As chamadas autenticadas devem enviar o header:
  ```http
  Authorization: Bearer {token}
  ```
- O endpoint `/api/logout` permite a **invalidação** antecipada do token.
- O usuário é responsável por proteger suas credenciais e tokens de acesso.

## 3. Rate Limiting
- Os endpoints de indicadores são protegidos por **rate limit de 5 requisições por minuto** por token/IP.
- Em excesso de chamadas, será retornado **HTTP 429** (*Too Many Requests*).
- São expostos os cabeçalhos de controle:
  - `X-RateLimit-Limit` — limite de requisições na janela atual;
  - `X-RateLimit-Remaining` — requisições restantes;
  - `X-RateLimit-Reset` — tempo (segundos) para reinício da janela.
- Práticas para convivência com o limite:
  - implemente **retentativa com *backoff* exponencial**;
  - **respeite cache** quando aplicável;
  - **evite *burst*** de tráfego desnecessário.

## 4. Versionamento
- O parâmetro de query opcional `v` pode ser enviado para **solicitar versões de resposta** futuras.
- **No momento**, este parâmetro **não altera** o payload retornado e é reservado para **retrocompatibilidade**.
- Mudanças incompatíveis poderão ser introduzidas apenas em novas versões, com aviso prévio.

## 5. Ambientes
- **Homologação / Testes**: `http://localhost:8080`
- **Produção**: `http://dt-d3yxt53:8080`
- Recomenda-se validar integrações **primeiro** no ambiente de testes antes de usar produção.

## 6. Disponibilidade e Suporte
- A API é disponibilizada em regime de **melhor esforço** (*best effort*).
- Janelas de manutenção podem ocorrer com notificação prévia quando possível.
- Canais de suporte e SLA, quando aplicável, são acordados em contrato/comercial.

## 7. Segurança e Boas Práticas
- Transmita tokens **apenas via HTTPS** em produção.
- Revogue tokens comprometidos utilizando `/api/logout`.
- Armazene mínimas informações pessoais necessárias (**princípio de minimização**).
- Não compartilhe tokens em repositórios públicos, issues, *logs* ou capturas de tela.

## 8. Uso Aceitável
É **vedado**:
- utilizar a API para atividades ilícitas;
- tentar explorar vulnerabilidades, burlar autenticação ou rate limiting;
- realizar *scraping* ou *flood* de requisições intencionalmente;
- redistribuir, *mirrorar* ou revender o acesso sem autorização.
Reservamo-nos o direito de **suspender** o acesso em caso de abuso.

## 9. Dados e Privacidade
- Logs de acesso podem ser coletados para auditoria, segurança e métricas.
- Dados pessoais tratados seguirão as leis aplicáveis (ex.: **LGPD**).
- Consulte nossa Política de Privacidade quando aplicável.

## 10. Alterações nos Termos
- Podemos atualizar estes Termos periodicamente.
- A continuidade de uso após a publicação de alterações implica **aceite** das novas condições.

## 11. Responsabilidades
- **Cliente/Integrador**: uso correto da API, gestão de credenciais, implementação de *retry* e *backoff*, e aderência aos limites.
- **Fornecedor da API**: disponibilização dos endpoints documentados, evolução versionada e comunicação de mudanças relevantes.

## 12. Contato
Dúvidas, solicitações ou incidentes de segurança: **contato@exemplo.com**.

---

### Referências Rápidas (trechos úteis para copy/paste)

**Header de Autenticação**
```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

**Exemplo de Resposta de Login (200)**
```json
{
  "token": "JWT...",
  "expiresIn": 1735689600
}
```

**Erro por Rate Limit (429)**
```json
{
  "erro": "Too Many Requests",
  "detalhe": "Limite excedido, tente novamente em alguns segundos."
}
```