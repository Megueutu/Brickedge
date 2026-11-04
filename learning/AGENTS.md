# AGENTS.md

Regras para quem trabalha neste repositório, pessoa ou ferramenta.

## Contexto

O Brickade é um projeto de estudo: aprender Rust e redes construindo um firewall de saída. O objetivo principal é **entender**, não entregar rápido. Leia [IDEA.md](IDEA.md), [APPROACH.md](APPROACH.md) e [ROADMAP.md](ROADMAP.md) antes de começar.

## Commits

- Commits vão **direto na `main`**, sem branch e sem pull request. Depois de commitar, faça push.
- Formato obrigatório: `tipo: mensagem`.
  - Certo: `feat: add tcp forwarder`, `fix: close socket on read error`, `docs: add roadmap`.
  - Errado: `feat(proxy): add forwarder`, `Feat: Add forwarder`, `added forwarder`.
- Tipos aceitos: `feat`, `fix`, `refactor`, `perf`, `test`, `docs`, `style`, `build`, `ci`, `chore`, `revert`.
- **Sem escopo** entre parênteses.
- Sempre em **inglês** e **todo em minúsculo**, incluindo o tipo e a primeira palavra.
- Mensagem curta, no imperativo.
- **Atômicos**: um commit por unidade lógica. Nunca agrupe mudanças sem relação e nunca faça commits redundantes.
- **Nenhum rastro de IA**: sem `Co-Authored-By`, sem trailers, sem menção a ferramentas de IA na mensagem, no código ou nos comentários. O commit reflete só a identidade git local de quem é dono do repositório.

## Idioma

- Nomes de arquivos, pastas, código, comentários e mensagens de commit: **inglês**.
- Documentação (`.md` de estudo e notas): **português**, com termos técnicos em inglês (`socket`, `handshake`, `proxy`).
- O `README.md` da raiz fica em inglês.

## Fronteira da IA

A IA é uma parceira de estudo, não a autora do núcleo do projeto.

**Pode fazer:**
- estrutura do projeto, `Cargo.toml`, CI e configuração;
- **testes unitários** e de integração;
- documentação, formatação e organização;
- explicar conceitos e mensagens de erro do compilador;
- revisar o código escrito e apontar problemas, sem reescrevê-lo.

**Quem estuda escreve:**
- a lógica central de cada etapa: o loop do proxy, o parser, o motor de regras e a detecção de segredos.

**Quando houver dificuldade:** a IA dá dicas e faz perguntas. Só mostra uma solução completa se for pedido explicitamente.

## Checkpoints

Cada etapa do [ROADMAP.md](ROADMAP.md) termina com um checkpoint do tipo "explique de volta":

1. Escreva em `learning/notes/` uma explicação com as suas palavras, sem copiar.
2. Quem revisa faz perguntas de aprofundamento até o entendimento ficar sólido.
3. A etapa só conta como concluída quando o checkpoint passa **e** a entrega funciona.

Arquivos de nota: `NN-topic.md` (nome em inglês, conteúdo em português), por exemplo `00-terrain.md`.

## Estrutura

```
.
├── AGENTS.md            aponta para este arquivo
├── README.md
├── learning/
│   ├── AGENTS.md        este arquivo
│   ├── IDEA.md          a ideia e o que é um firewall
│   ├── APPROACH.md      a abordagem técnica
│   ├── ROADMAP.md       etapas, entregas e checkpoints
│   └── notes/           explicações escritas por quem estuda
└── (código do firewall: workspace Cargo na raiz, a partir da etapa 1)
```
