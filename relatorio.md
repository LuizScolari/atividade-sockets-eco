# Relatório — Atividade Prática: Sockets TCP

**Disciplina:** Sistemas Distribuídos
**Aluno:** Luiz Roberto Pereira Scolari
**Data:** 21/11/2026

---

## Nível 1 — Inspecionar o código

### 1.1 Servidor (`servidor.py`)

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | Cria um socket TCP (IPv4). |
| `servidor_socket.bind((HOST, PORTA))` | Associa o socket à porta 5000. |
| `servidor_socket.listen(5)` | Começa a escutar conexões (fila de até 5). |
| `servidor_socket.accept()` | Espera e aceita a conexão de um cliente. |
| `conn.recv(TAMANHO_BUFFER)` | Recebe a mensagem do cliente. |
| `conn.sendall(resposta.encode("utf-8"))` | Envia toda a resposta ao cliente (o `send()` pode enviar só uma parte). |
| `conn.close()` | Fecha a conexão. |

### 1.2 Cliente (`cliente.py`)

| Chamada | O que ela faz? |
|---------|---------------|
| `socket.socket(AF_INET, SOCK_STREAM)` | Cria um socket TCP (IPv4). |
| `cliente_socket.connect((HOST_SERVIDOR, PORTA_SERVIDOR))` | Conecta ao servidor. |
| `cliente_socket.sendall(mensagem.encode("utf-8"))` | Envia a mensagem ao servidor. |
| `cliente_socket.recv(TAMANHO_BUFFER)` | Recebe o eco do servidor. |

### 1.3 Rede e contêineres

Por que o cliente usa o hostname `"servidor"` em vez de `"localhost"`
para se conectar? O que aconteceria se usasse `"localhost"`?

> _Resposta:_ Cada contêiner tem sua própria rede, então `localhost` aponta
> para o próprio contêiner do cliente, onde não há servidor. Usando
> `"localhost"` daria erro de conexão recusada. O nome `"servidor"` é
> resolvido pelo Docker para o IP do contêiner do servidor na rede `rede-eco`.

---

## Nível 2 — Modificar o servidor

1. Devolver a mensagem **em maiúsculas**:

   > _O que foi alterado e por quê:_ Troquei `resposta = mensagem` por
   > `resposta = mensagem.upper()`, que converte o texto para maiúsculas
   > antes de enviar.

2. Exibir no log o **contador de mensagens recebidas** acumulado:

   > _O que foi alterado e por quê:_ Adicionei `total_mensagens += 1` a cada
   > mensagem recebida e mostrei o valor no log
   > (`Mensagem #{total_mensagens} recebida`).

Após a modificação, você precisou rodar `docker compose up --build`.
Por que o `--build` é necessário aqui?

> _Resposta:_ O código é copiado para dentro da imagem no `Dockerfile`.
> Sem `--build`, o Docker usaria a imagem antiga, sem as mudanças.


