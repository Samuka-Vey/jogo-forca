# Guia de uso

## 1. Iniciar uma sessão

1. Abra um terminal na pasta do projeto.
2. Inicie o servidor:

   ```bash
   node server.js
   ```

3. Abra um segundo terminal e inicie o cliente:

   ```bash
   node cliente.js
   ```

4. Se estiver usando outro computador, passe ao cliente o IP do servidor e a porta correta, conforme suportado pela versão do código.

## 2. Interagir com a aplicação

O protocolo da aplicação utiliza comandos textuais. Na versão documentada, os comandos previstos incluem:

| Comando | Finalidade |
|---|---|
| `ENTRAR <nome>` | Solicita entrada na partida/sessão com o nome informado. |
| `LETRA <x>` | Envia uma letra como tentativa no jogo de palavras. |
| `SAIR` | Solicita sair da sessão. |

Se a implementação disponibilizar atalhos, mensagens adicionais ou validações diferentes, siga as instruções exibidas pelo próprio cliente e consulte `protocolo.js`.

## 3. Entender as mensagens

As mensagens trocadas entre cliente e servidor representam solicitações, respostas e atualizações de estado. O arquivo `protocolo.js` centraliza as regras de codificação/decodificação e o tratamento das mensagens.

Exemplos conceituais:

- O cliente envia um comando para entrar.
- O servidor valida a solicitação e responde com o resultado.
- Durante o jogo, o cliente envia tentativas.
- O servidor atualiza o estado e envia uma resposta ao cliente.

Os textos exatos das respostas dependem da implementação. Não considere que uma mensagem foi recebida pelo servidor apenas porque o cliente conseguiu enviá-la — isso é especialmente importante em UDP.

## 4. Executar testes básicos

Faça os testes nesta ordem:

1. **Servidor ativo:** inicie o servidor e confira se ele informa que está pronto.
2. **Cliente local:** execute o cliente no mesmo computador.
3. **Entrada:** envie `ENTRAR <nome>` e observe a resposta.
4. **Interação:** envie uma letra usando `LETRA <x>`.
5. **Saída:** envie `SAIR` e confira o encerramento.
6. **Servidor desligado:** encerre o servidor e observe como o cliente lida com a falta de resposta.

Ao testar o servidor desligado, diferencie os comportamentos dos transportes:
- **TCP:** a conexão pode ser recusada ou encerrada, dependendo do momento em que o servidor é desligado.
- **UDP:** o envio de um datagrama não confirma que o processo remoto está ativo. O cliente pode esperar uma resposta até seu timeout; em certas condições, o sistema operacional pode reportar um erro de rede, mas isso não é garantido.

## 5. Testar a codificação de uma mensagem

Para observar como uma mensagem textual é convertida em bytes no Node.js, execute no terminal:

```bash
node -e "const mensagem = 'LETRA s\\n'; console.log(Buffer.from(mensagem, 'utf8'))"
```

O `Buffer` mostra os bytes enviados. O caractere `\n` pode ser usado como delimitador de mensagem em protocolos baseados em linhas; confirme se ele é exigido pelo transporte e pela versão atual do protocolo.

## 6. Encerrar corretamente

Use o comando `SAIR` no cliente, se disponível, e encerre os processos com `Ctrl + C` quando necessário. Em seguida, confira os logs do servidor para entender quais mensagens foram processadas.
