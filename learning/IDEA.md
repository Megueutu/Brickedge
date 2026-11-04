# A ideia

O Brickade é um **firewall de saída** escrito em Rust, feito para estudar Rust e redes construindo algo que resolve um problema de verdade. Não é um produto: não substitui nenhuma ferramenta que já existe. O valor está no que se aprende ao construí-lo.

## O problema

Segredos (chaves de API, tokens, conteúdo de um `.env`) saem da máquina de quem desenvolve sem ninguém perceber:

- colados em ferramentas de IA ou em serviços de colar texto;
- enviados por um script de `postinstall` de uma dependência maliciosa;
- vazados por uma requisição de depuração esquecida no código.

Firewalls comuns olham **para onde** a conexão vai (IP e porta). Ferramentas como o `gitleaks` olham **repositórios**, não o tráfego. A ideia do Brickade é um ponto simples no meio: um proxy local que olha o que está **saindo** e bloqueia quando algo parece um segredo indo para onde não deveria.

## O que é um firewall, por dentro

Um firewall é um **ponto de controle** no caminho do tráfego. Para cada conexão ou pacote, ele compara com uma lista de **regras** e executa uma **ação** (permitir ou bloquear), registrando a decisão.

Conceitos que aparecem em qualquer firewall:

- **Regra:** uma condição (de onde vem, para onde vai, porta, protocolo, conteúdo) mais uma ação.
- **Ação:** `allow` (permitir) ou `deny` (bloquear).
- **Política padrão:** o que acontece quando nenhuma regra casa. Bloquear por padrão (*default deny*) é mais seguro do que permitir por padrão.
- **Ordem:** em geral vale a primeira regra que casa, então a ordem importa.
- **Direção:** *ingress* (o que entra) e *egress* (o que sai). O Brickade cuida do que sai.
- **Log:** toda decisão deixa registro, para que se possa entender por que algo foi bloqueado.

Os tipos mais comuns, do mais simples ao mais esperto:

| Tipo | O que enxerga | Exemplo de regra |
| --- | --- | --- |
| Filtro de pacotes | Cabeçalhos de IP e TCP/UDP, pacote a pacote | bloquear a porta 23 |
| Stateful | Também o estado da conexão (iniciada de dentro? já estabelecida?) | só aceitar respostas de conexões que partiram de dentro |
| Proxy / camada de aplicação | O protocolo em si, como HTTP | bloquear requisições a um domínio |
| Firewall de nova geração | Aplicação, usuário, reputação, inspeção profunda | bloquear um tipo de aplicativo |

O Brickade é um **proxy de camada de aplicação**: o programa cliente se conecta a ele, e ele decide se repassa a conexão adiante.

## Dentro do escopo

- Proxy local de saída para HTTP e `CONNECT` (túnel para HTTPS).
- Regras por domínio, com bloqueio por padrão.
- Detecção de segredos em tráfego HTTP sem criptografia.
- Logs claros de cada decisão.

## Fora do escopo

- Filtragem de pacotes no nível do kernel, NAT, VPN e detecção de intrusão completa.
- Desempenho ou segurança de nível de produção.
- Suporte a todos os protocolos além de HTTP.
- Interceptação de HTTPS: só como etapa opcional e avançada (ver [APPROACH.md](APPROACH.md)).

## Como aprender com isso

Cada conceito acima vira uma etapa do [ROADMAP.md](ROADMAP.md), com uma entrega no projeto e um checkpoint em que você explica com as suas palavras. As regras de trabalho estão em [AGENTS.md](AGENTS.md).
