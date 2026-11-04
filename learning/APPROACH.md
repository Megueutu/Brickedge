# Abordagem

Como o Brickade ataca o problema descrito em [IDEA.md](IDEA.md): impedir que segredos saiam da máquina para destinos não autorizados.

## Visão geral

O Brickade é um **proxy local**. Os programas são configurados para usá-lo (por exemplo, com a variável `HTTP_PROXY`), e ele fica entre eles e a internet:

```
programa  ──►  Brickade (127.0.0.1:porta)  ──►  internet
                 │
                 ├─ lê a requisição
                 ├─ aplica as regras
                 └─ repassa ou bloqueia, e registra a decisão
```

Fluxo de cada conexão:

1. Aceita a conexão do programa.
2. Lê o início da requisição (HTTP ou `CONNECT`).
3. Decide: `allow` ou `deny`, segundo as regras e a política padrão (bloquear).
4. Se permitir, abre a conexão com o destino e repassa os bytes nos dois sentidos. Se bloquear, responde com um erro e fecha.
5. Registra a decisão no log.

## Versões

### v1: só host

Aceita `CONNECT host:porta` (e requisições HTTP simples) e decide **pelo domínio**, com uma lista de permitidos e bloqueados. Não lê conteúdo nenhum. Já é útil: sem a lista, nada sai.

### v2: inspeção de HTTP sem criptografia

Para requisições HTTP puras, lê cabeçalhos e corpo e procura **segredos**. Se achar, bloqueia a requisição e registra o motivo.

### v3 (opcional): interceptação de TLS

O proxy gera certificados próprios, assinados por uma CA local que a máquina passa a confiar, e assim decifra o HTTPS para inspecionar. É difícil e delicado (mexe em confiança de certificados), por isso só será feita se a v2 estiver sólida e houver vontade de aprofundar.

## Por que o HTTPS limita um firewall

Quase todo o tráfego de hoje é HTTPS, ou seja, **criptografado de ponta a ponta** entre o programa e o servidor. O que o proxy consegue ver:

- no `CONNECT`, o **domínio e a porta** que o programa quer alcançar;
- no começo do handshake TLS, em geral o nome do servidor (o campo SNI), que pode vir escondido em alguns casos;
- depois disso, só bytes cifrados que ele não consegue ler.

Consequência importante: na v1 e na v2 o proxy **não vê segredos dentro de HTTPS**. A v2 só inspeciona HTTP puro, que é uma parte pequena do tráfego real. Isso não é um defeito do plano, é o conceito central que o projeto ensina, e é o motivo de a v3 existir como etapa opcional.

## Como detectar um segredo (decisão em aberto)

Esta decisão será tomada na etapa 6 do [ROADMAP.md](ROADMAP.md), depois de entender o problema na prática. As ideias em discussão:

- **Formatos conhecidos:** padrões de chaves da AWS, tokens do GitHub e chaves privadas PEM, com expressões regulares.
- **Entropia:** strings longas e aparentemente aleatórias costumam ser segredos, mas geram falsos positivos.
- **Impressão digital dos próprios segredos:** ler os valores do `.env` uma vez e guardar só o **hash** deles, nunca o texto, bloqueando qualquer requisição que contenha um valor que bata.

## Limites assumidos

- Um programa que ignora o proxy não passa por ele.
- Um segredo codificado, comprimido ou dividido em partes pode escapar.
- Detecção gera falsos positivos e falsos negativos; o objetivo é aprender a lidar com isso, não eliminá-lo.
- Este projeto não oferece garantia de segurança e não deve ser usado como única proteção.
