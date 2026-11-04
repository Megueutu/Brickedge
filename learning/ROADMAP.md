# Roadmap

Cada etapa tem um **conceito** para entender, uma **entrega** no firewall e um **checkpoint** "explique de volta" em [notes/](notes/). Uma etapa só termina quando o checkpoint passa **e** a entrega funciona. Não há prazos: o que importa é entender. Regras de trabalho em [AGENTS.md](AGENTS.md).

Profundidade: vale ir fundo em ownership e borrowing, erros, traits, `async`, TCP, HTTP, TLS (conceito) e hashing. Vale só conhecer por cima `unsafe`, macros, assembly, o kernel por dentro e detalhes de cifras do TLS.

Tamanho: **P** (pequena), **M** (média), **G** (grande).

## Etapa 0: o terreno (P)

- [ ] **Conceito:** o que é um firewall por dentro (tipos, regras, bloqueio por padrão, ordem), o modelo TCP/IP em camadas, IP, porta e socket.
- [ ] **Entrega:** nenhuma de código. Reler [IDEA.md](IDEA.md) e [APPROACH.md](APPROACH.md) e ajustar o que não fizer sentido.
- [ ] **Checkpoint:** `notes/00-terrain.md`.

## Etapa 1: Rust base (G)

- [ ] **Conceito:** `cargo`, tipos, ownership e borrowing, `struct`, `enum` e `match`, `Result` e tratamento de erro.
- [ ] **Entrega:** um CLI mínimo que carrega uma lista de regras permitir/bloquear de um arquivo, com testes.
- [ ] **Checkpoint:** `notes/01-rust-basics.md`.

## Etapa 2: TCP na prática (M)

- [ ] **Conceito:** sockets, handshake do TCP, ler e escrever bytes, threads. Observar o tráfego com `tcpdump` e `curl`.
- [ ] **Entrega:** um proxy que só repassa conexões, sem regras, com uma thread por conexão.
- [ ] **Checkpoint:** `notes/02-tcp.md`.

## Etapa 3: HTTP e `CONNECT` (M)

- [ ] **Conceito:** formato do HTTP/1.1, o que é um proxy HTTP, o método `CONNECT`, traits e generics.
- [ ] **Entrega:** **v1**, filtro por domínio com bloqueio por padrão e log de cada decisão.
- [ ] **Checkpoint:** `notes/03-http-connect.md`.

## Etapa 4: `async` com `tokio` (G)

- [ ] **Conceito:** por que existe `async`, o que é uma task, concorrência versus paralelismo.
- [ ] **Entrega:** o proxy migrado de threads para `async`.
- [ ] **Checkpoint:** `notes/04-async.md`.

## Etapa 5: TLS e o limite do firewall (P)

- [ ] **Conceito:** o que o TLS protege, o que o proxy enxerga em HTTPS (domínio, SNI) e o que não enxerga, por que isso limita a inspeção.
- [ ] **Entrega:** nenhuma de código novo. Atualizar o [APPROACH.md](APPROACH.md) se o entendimento mudou.
- [ ] **Checkpoint:** `notes/05-tls-limits.md`.

## Etapa 6: inspeção de HTTP puro (G)

- [ ] **Conceito:** parser de requisição (cabeçalhos e corpo), hashing, falsos positivos e falsos negativos.
- [ ] **Decisão:** escolher como detectar segredos (ver [APPROACH.md](APPROACH.md)) e registrar a escolha e o motivo.
- [ ] **Entrega:** **v2**, bloqueio de vazamento de segredos em HTTP sem criptografia.
- [ ] **Checkpoint:** `notes/06-secret-detection.md`.

## Etapa 7: robustez (M)

- [ ] **Conceito:** configuração, logs úteis, tratamento de erros, limites de tamanho e de tempo.
- [ ] **Entrega:** configuração por arquivo, log estruturado, testes de integração e documentação de uso no `README.md`.
- [ ] **Checkpoint:** `notes/07-robustness.md`.

## Etapa 8 (opcional): interceptação de TLS (G)

- [ ] **Conceito:** certificados, CA local, cadeia de confiança, riscos de confiar em uma CA própria.
- [ ] **Entrega:** **v3**, inspeção de HTTPS com uma CA local.
- [ ] **Checkpoint:** `notes/08-tls-interception.md`.
