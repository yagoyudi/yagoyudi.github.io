# **Trabalho Prático de Redes de Computadores 2**

## **Como foi feito**

O código está disponível no repositório: [github.com/yagoyudi/gobench-tcp-udp](https://github.com/yagoyudi/gobench-tcp-udp).

Links diretos:
-   [Cliente e servidor TCP](https://github.com/yagoyudi/gobench-tcp-udp/blob/main/internal/benchmark/tcp.go)
-   [Cliente e servidor UDP](https://github.com/yagoyudi/gobench-tcp-udp/blob/main/internal/benchmark/udp.go)

A estrutura do projeto é a seguinte:

```
.
├── cmd
│   └── gobench
│       └── main.go
├── internal
│   ├── benchmark
│   │   ├── tcp.go
│   │   └── udp.go
│   └── cmd
│       ├── const.go
│       ├── gobench.go
│       ├── tcp_client.go
│       ├── tcp.go
│       ├── tcp_server.go
│       ├── udp_client.go
│       ├── udp.go
│       └── udp_server.go
├── logs
│   ├── tcp-client.txt
│   ├── tcp-server.txt
│   ├── udp-client.txt
│   └── udp-server.txt
├── Makefile
└── README.md
```

### **Descrição Geral**

O trabalho consistiu no desenvolvimento de um sistema que mede o desempenho da transferência de dados usando os protocolos **TCP** e **UDP**. A implementação possui:

- **Cliente e Servidor TCP:** Realizam conexões confiáveis, medindo o tempo de transferência de grandes volumes de dados.
- **Cliente e Servidor UDP:** Analisam o tempo de envio em um protocolo não orientado à conexão, permitindo avaliar o impacto de perdas e variações na rede.
- Os logs de execução são armazenados na pasta `logs`, com arquivos separados para cada protocolo e papel (cliente/servidor).

### **Estrutura do Projeto**

- **`cmd/gobench`**: Contém o ponto de entrada do programa (`main.go`).
- **`internal/cmd`**: Abriga a lógica da interface de linha de comando (CLI), incluindo o gerenciamento de argumentos e opções.
- **`internal/benchmark`**: Implementa a lógica central para os clientes e servidores TCP e UDP.
- **`logs`**: Diretório para os registros de execução, permitindo análises detalhadas.

---

## **Como Compilar e Executar**

### **Compilação**

Execute o comando abaixo para compilar o programa:

```
make
```

O binário gerado será salvo em `./bin/gobench`.

### **Uso**

Para detalhes sobre como usar o programa, execute:

```
./bin/gobench --help
```

#### **Exemplos**

1. **Iniciar um servidor TCP** no endereço `127.0.0.1:8080`:

   ```
   ./bin/gobench tcp server 127.0.0.1:8080
   ```

2. **Criar um cliente TCP** que envia 1 GB de dados para o servidor em `127.0.0.1:8080`:

   ```
   ./bin/gobench tcp client 127.0.0.1:8080 --total 1gb
   ```

3. **Iniciar um servidor UDP** no endereço `127.0.0.1:9090`:

   ```
   ./bin/gobench udp server 127.0.0.1:9090
   ```

4. **Criar um cliente UDP** que envia 500 MB de dados para o servidor em `127.0.0.1:9090`:

   ```
   ./bin/gobench udp client 127.0.0.1:9090 --total 500mb
   ```

> **Nota:** Caso o parâmetro `--total` não seja especificado, o programa utiliza um valor padrão de 250 MB.

---

## **Resultados Obtidos**

### **Análise de Desempenho**

Os testes foram realizados em uma máquina local com **[detalhes do hardware e sistema operacional]**. Os resultados medem o tempo médio de transferência e o throughput para diferentes tamanhos de dados. Seguem os principais pontos observados:

1. **Protocolo TCP:**
   - Transferência confiável sem perda de pacotes.
   - Desempenho consistente, mas levemente impactado por overheads de garantia de entrega.

2. **Protocolo UDP:**
   - Apresentou maior throughput em redes estáveis.
   - Sensível a condições adversas da rede, com possíveis perdas de pacotes, especialmente para volumes maiores.

### **Resumo dos Resultados**

| Protocolo | Tamanho Total (MB) | Tempo Médio (s) | Throughput (MB/s) |
|-----------|---------------------|------------------|-------------------|
| TCP       | 500                 | X.XX             | XX.XX             |
| TCP       | 1000                | X.XX             | XX.XX             |
| UDP       | 500                 | X.XX             | XX.XX             |
| UDP       | 1000                | X.XX             | XX.XX             |

---

## **Conclusão**

