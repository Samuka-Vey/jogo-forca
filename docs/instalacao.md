# Documentação de instalação

## 1. Visão geral

Este projeto implementa uma aplicação cliente-servidor em JavaScript/Node.js, usando sockets para trocar mensagens entre um cliente e um servidor. O repositório contém os arquivos `server.js`, `cliente.js`, `protocolo.js` e `config.js`.

> **Observação:** confirme no código e no `package.json` qual transporte está habilitado na versão que você recebeu (TCP ou UDP). Os comandos abaixo correspondem à execução direta dos arquivos JavaScript.

## 2. Pré-requisitos

- Node.js instalado (recomenda-se uma versão LTS recente).
- npm, normalmente instalado junto com o Node.js.
- Terminal/Prompt de Comando.
- Para testar em dois computadores, ambos devem conseguir se comunicar pela rede local; o firewall pode precisar permitir a porta usada pelo servidor.

Verifique as instalações:

```bash
node --version
npm --version
```

Se os comandos não forem reconhecidos, instale o Node.js pelo site oficial: https://nodejs.org/

## 3. Obter e extrair o projeto

1. Extraia `batalha-naval.rar` para uma pasta.
2. Abra um terminal dentro da pasta extraída, onde ficam `server.js` e `cliente.js`.
3. Confira os arquivos:

```text
server.js
cliente.js
config.js
protocolo.js
package.json
```

## 4. Instalar dependências

Execute:

```bash
npm install
```

Se o projeto não declarar dependências externas, o comando pode terminar sem instalar pacotes adicionais. Não é necessário instalar bibliotecas manualmente sem que o `package.json` ou o código indiquem essa necessidade.

## 5. Iniciar o servidor

Na pasta do projeto, execute:

```bash
node server.js
```

Mantenha esse terminal aberto. O servidor precisa estar em execução para receber conexões ou datagramas do cliente.

## 6. Iniciar o cliente

Abra **outro terminal** na mesma pasta e execute:

```bash
node cliente.js
```

Se o cliente aceitar endereço IP e porta como argumentos, você também pode informar explicitamente os valores configurados no projeto, por exemplo:

```bash
node cliente.js 127.0.0.1 5000
```

Use a porta definida em `config.js` e confirme no código se a versão atual lê esses argumentos.

## 7. Testar em dois computadores

1. Execute o servidor no computador servidor.
2. Descubra o endereço IPv4 desse computador na rede local.
3. No cliente, informe o IP do servidor e a porta configurada.
4. Permita a comunicação pela porta no firewall, se necessário.
5. Não use `0.0.0.0` como endereço de destino no cliente: esse endereço serve para o servidor escutar em todas as interfaces locais. O cliente deve usar o IP real do computador servidor.

## 8. Problemas comuns

- **`ECONNREFUSED` (TCP):** geralmente indica que não há serviço escutando no IP/porta de destino ou que a conexão foi recusada.
- **Timeout:** pode indicar IP/porta incorretos, firewall, servidor indisponível ou perda de pacotes. Em UDP, não há estabelecimento de conexão nem garantia de entrega; a ausência de resposta pode apenas causar timeout na aplicação.
- **Porta ocupada:** outro processo pode estar usando a mesma porta. Encerre o processo conflitante ou altere a configuração.
- **Cliente não encontra o servidor:** confira IP, porta, firewall e se o servidor está ativo.
- **Erro ao usar `npm`:** confira se o terminal está na pasta que contém `package.json`.

## 9. Encerramento

Para encerrar, use `Ctrl + C` no terminal do servidor e, depois, no terminal do cliente. Em UDP, fechar o processo do servidor não gera necessariamente uma mensagem automática de erro ao cliente; isso depende da aplicação e do sistema operacional.
