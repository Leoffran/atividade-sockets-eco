# Relatório — Atividade Prática: Sockets TCP

**Disciplina:** Sistemas Distribuídos
**Dupla:** [Rodrigo Martins dos Santos] / [Leonardo Fonseca Franchini]
**Data:** 25/06/26

---

## Nível 1 — Inspecionar o código

### 1.1 Servidor (`servidor.py`)

Descreva com suas palavras o que cada chamada abaixo faz e por que ela
é necessária:

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | Cria um socket de rede usando IPv4 (AF_INET) e o protocolo TCP (SOCK_STREAM). Esse socket será o ponto de comunicação do servidor com os clientes. |
| `servidor_socket.bind((HOST, PORTA))` | Associa o socket do servidor a um endereço IP e a uma porta específicos. Isso é necessário para que o sistema operacional saiba em qual endereço o servidor ficará aguardando conexões. |
| `servidor_socket.listen(5)` | Coloca o socket em modo de escuta, permitindo que ele aceite conexões de clientes. O valor 5 indica o tamanho máximo da fila de conexões pendentes enquanto o servidor ainda não as aceitou. |
| `servidor_socket.accept()` | Aguarda a chegada de uma conexão de cliente. Quando isso acontece, retorna um novo socket dedicado à comunicação com aquele cliente e também o endereço do cliente conectado. |
| `conn.recv(TAMANHO_BUFFER)` | Recebe os dados enviados pelo cliente, lendo até TAMANHO_BUFFER bytes por vez. É por meio dessa chamada que o servidor obtém a mensagem enviada. |
| `conn.sendall(resposta.encode("utf-8"))` | Envia a resposta ao cliente em formato de bytes. O método sendall garante que todos os bytes da mensagem sejam transmitidos antes de retornar. O encode("utf-8") converte a string em bytes para que ela possa ser enviada pela rede. |
| `conn.close()` | Fecha a conexão com o cliente, liberando os recursos associados a esse socket após o término da comunicação. |

### 1.2 Cliente (`cliente.py`)

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | Cria um socket de rede usando IPv4 e TCP, que será utilizado pelo cliente para se comunicar com o servidor. |
| `cliente_socket.connect((HOST_SERVIDOR, PORTA_SERVIDOR))` | Inicia a conexão com o servidor, informando o endereço e a porta onde ele está escutando. Sem essa etapa, o cliente não consegue estabelecer comunicação. |
| `cliente_socket.sendall(mensagem.encode("utf-8"))` | Envia a mensagem ao servidor em formato de bytes. O sendall garante o envio completo da mensagem, e o encode("utf-8") faz a conversão da string para bytes. |
| `cliente_socket.recv(TAMANHO_BUFFER)` | Recebe a resposta enviada pelo servidor, lendo até TAMANHO_BUFFER bytes do socket. |

### 1.3 Rede e contêineres

Por que o cliente usa o hostname `"servidor"` em vez de `"localhost"`
para se conectar? O que aconteceria se usasse `"localhost"`?

> _Resposta:_
O cliente usa o hostname "servidor" porque, em uma rede com contêineres Docker, esse nome identifica o contêiner onde o servidor está rodando. Já "localhost" se refere ao próprio contêiner do cliente, ou seja, a ele mesmo. Se o cliente usasse "localhost", tentaria se conectar ao seu próprio contêiner em vez do servidor, e a conexão provavelmente falharia porque não haveria nenhum serviço escutando nessa porta.
---

## Nível 2 — Modificar o servidor

Descreva as mudanças que você fez no `servidor.py` para:

1. Devolver a mensagem **em maiúsculas**:

   > _O que foi alterado e por quê:_
   Colocamos um .upper() na resposta antes de devolve-la ao cliente, isso faz o servidor retornar o eco em maiúsculo para o cliente (resposta = mensagem.upper()).

2. Exibir no log o **contador de mensagens recebidas** acumulado:

   > _O que foi alterado e por quê:_
   Somamos a variável total_mensagens sempre que há dados. Após fechar a conexão, o servidor exibe o número total de mensagens.

Após a modificação, você precisou rodar `docker compose up --build`.
Por que o `--build` é necessário aqui?

> _Resposta:_
O --build é necessário pois houve uma mudança no código da aplicação dentro do contêiner. Ao executar sem o --build após atualizar o código, o docker pode rodar uma versão antiga do programa
---

## Observações livres

Anote aqui qualquer comportamento inesperado que observaram, erros que
apareceram e como resolveram, ou dúvidas que ainda têm:

>

---

## Dúvida para a próxima aula

Escreva **uma pergunta** que surgiu durante a atividade e que vocês
gostariam de ver respondida na aula seguinte:

>
