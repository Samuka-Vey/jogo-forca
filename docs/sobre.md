# Sobre o projeto

## 1. O que é?

O projeto é uma aplicação de comunicação em rede, escrita em JavaScript com Node.js, organizada em cliente e servidor. O cliente envia comandos e recebe respostas; o servidor interpreta as mensagens, executa as regras da aplicação e devolve atualizações.

O repositório contém:

- `server.js`: lógica do servidor e processamento das mensagens recebidas.
- `cliente.js`: lógica do cliente e interação com o servidor.
- `protocolo.js`: formato das mensagens e funções auxiliares para codificação/decodificação.
- `config.js`: parâmetros de configuração, como endereço e porta.
- `package.json`: metadados do projeto e possíveis scripts/dependências.
- `wire/`: evidências de testes, capturas ou imagens relacionadas à comunicação.
- `assets/`: recursos visuais/documentação do projeto.

## 2. Objetivo

O objetivo técnico é demonstrar como processos em computadores (ou no mesmo computador) podem trocar mensagens usando sockets. A aplicação permite estudar a separação entre cliente e servidor, o formato de um protocolo próprio e o comportamento da comunicação em situações normais e de falha.

## 3. O que é um socket?

Um **socket** é uma interface de comunicação usada por um programa para enviar e receber dados pela rede. Ele associa a aplicação a um mecanismo de transporte e, conforme o tipo de socket, a um endereço e uma porta.

O socket não é, por si só, o protocolo completo do jogo: as regras de mensagens e o significado de comandos como `ENTRAR`, `LETRA` e `SAIR` são definidos pela aplicação.

## 4. Cliente, servidor, IP e porta

- **Cliente:** inicia a comunicação e envia solicitações.
- **Servidor:** aguarda mensagens e responde conforme a lógica do programa.
- **IP:** identifica uma interface/dispositivo na rede.
- **Porta:** identifica o serviço ou ponto de comunicação dentro do dispositivo.
- **Protocolo da aplicação:** define como representar comandos, estados e respostas.

O endereço `0.0.0.0` costuma ser usado pelo servidor para escutar em todas as interfaces IPv4 locais. Para conectar-se a ele, o cliente deve usar um endereço de destino específico, como `127.0.0.1` em testes locais ou o IPv4 do servidor em uma rede.

## 5. TCP e UDP: diferença essencial

| Característica | TCP | UDP |
|---|---|---|
| Estabelece conexão de transporte | Sim | Não |
| Entrega e ordenação confiáveis | Sim, para o fluxo de bytes | Não garantidas |
| Mantém limites entre mensagens da aplicação | Não; a aplicação precisa definir o enquadramento | Cada datagrama preserva seu limite |
| Confirma que o programa remoto processou a mensagem | Não, por si só | Não |
| Tratamento de falhas | A pilha TCP detecta várias falhas de conexão | A aplicação geralmente precisa tratar timeout e ausência de resposta |

Em TCP, a aplicação precisa delimitar as mensagens, por exemplo usando `\\n`, tamanho prefixado ou outro formato. Em UDP, cada datagrama é uma unidade separada, mas ainda é necessário definir como interpretar seu conteúdo.

## 6. O que acontece quando o servidor é desligado?

O resultado depende do transporte e do estado da comunicação:

- Em TCP, uma tentativa de conexão pode receber uma recusa; uma conexão existente pode ser encerrada ou apresentar erro.
- Em UDP, não existe handshake para confirmar que o servidor está disponível. O cliente pode enviar um datagrama sem receber resposta. Um timeout é normalmente uma decisão da aplicação, não uma confirmação universal de que o servidor caiu.
- Mensagens de erro ICMP podem ser geradas em algumas situações, como porta UDP sem serviço escutando, mas a entrega desse erro ao programa não é garantida em todos os sistemas e cenários.

Por isso, testes devem observar tanto os logs do cliente quanto os do servidor e distinguir o que o protocolo de transporte garante do que a aplicação implementa.

## 7. Estrutura do protocolo da aplicação

O arquivo `protocolo.js` é o ponto principal para consultar o formato das mensagens. Ele deve ser lido junto com `cliente.js` e `server.js` para compreender:

1. quais comandos o cliente pode enviar;
2. como as mensagens são separadas e interpretadas;
3. quais respostas o servidor produz;
4. como estados e erros são representados;
5. como mensagens inválidas ou repetidas são tratadas.

## 8. Limitações e cuidados

- Não assumir que enviar uma mensagem significa que ela foi recebida ou processada.
- Não usar `0.0.0.0` como IP de destino do cliente.
- Não confundir porta de destino do serviço com porta efêmera local usada pelo sistema operacional.
- Não supor que UDP detectará automaticamente um servidor desligado.
- Em testes em rede local, conferir firewall, endereço IP, porta e configuração do transporte.

## 9. Como explorar o código

Uma ordem recomendada de leitura é:

1. `config.js` — identificar os parâmetros.
2. `protocolo.js` — entender os comandos e o formato das mensagens.
3. `server.js` — acompanhar a recepção e o processamento.
4. `cliente.js` — acompanhar a criação e o envio das mensagens.
5. `wire/` — comparar o comportamento observado com os testes registrados.
