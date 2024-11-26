--- Aplicação Cliente-Servidor para Transporte Confiável de Dados ---

Disciplina: Infra. De Comunicação
Professor: Petronio Junior

Alunos:
- Paulo Portella (phcp@cesar.school)
- Gustavo Carneiro (gcic@cesar.school)

Descrição
Esta aplicação implementa um protocolo de transporte confiável na camada de aplicação, simulando características como controle de fluxo, controle de congestionamento, detecção de erros e retransmissão. Ela permite a simulação de perdas de pacotes e erros de integridade, suportando os protocolos Go-Back-N (GBN) e Selective Repeat (SR).

A aplicação utiliza sockets UDP para comunicação entre cliente e servidor, implementando mecanismos adicionais para garantir a confiabilidade da transmissão, já que o UDP é um protocolo não confiável por natureza.

Funcionalidades principais:

- Controle de fluxo: O cliente considera a janela de recepção do servidor, que pode ser atualizada dinamicamente.
- Controle de congestionamento: A janela de congestionamento é ajustada com base em ACKs (acknowledgements) e NACKs (negative acknowledgements).
- Detecção de erros: Uso de checksum para verificar a integridade dos pacotes.
- Retransmissão de pacotes: Pacotes perdidos ou com erro são retransmitidos.
- Protocolos suportados: Go-Back-N (GBN) e Selective Repeat (SR).
- Limite de tamanho de mensagem: As mensagens possuem um tamanho máximo de 100 caracteres.

--- Requisitos ---

Python 3.6 ou superior.
Sistemas operacionais: Windows, Linux ou macOS.

--- Execução ---

-Servidor 
Digite no terminal:

  python servidor.py

Interaja com o servidor através do menu exibido no terminal.

-Cliente
Digite em outro terminal:

  python cliente.py

Interaja com o cliente utilizando o menu exibido.


--- Comandos Disponíveis ---

- Servidor

  1 - Ativar/Desativar erro de integridade em ACKs
 Simula erros nos ACKs enviados ao cliente.
 
  2- Configurar pacotes para simular perdas
 Especifica quais pacotes devem ser "perdidos" (não serão respondidos).

  3- Exibir status da janela de congestionamento
Mostra o tamanho atual da janela de congestionamento.

  4- Alterar protocolo (Selective Repeat / Go-Back-N)
 Define o protocolo a ser utilizado.

  5- Sair
 Encerra o servidor.


- Cliente
  
  1- Enviar uma única mensagem
 Envia uma mensagem ao servidor.

  2- Enviar várias mensagens em sequência
 Envia múltiplas mensagens consecutivamente.

  3- Enviar pacote com erro de checksum
 Envia um pacote com erro de integridade para simulação.

  4- Configurar protocolo (Selective Repeat / Go-Back-N)
 Define o protocolo a ser utilizado pelo cliente.

  5- Alterar o tamanho da janela de recepção
 Atualiza dinamicamente o tamanho da janela de recepção do cliente.

  6- Verificar integridade do pacote
 Permite verificar a integridade de uma mensagem antes de enviá-la.

  7- Sair
 Encerra o cliente.


--- Testando Funcionalidades ---

  1- Configurar o protocolo:

No cliente, escolha a opção 4 para configurar o protocolo desejado (GBN ou SR).
O servidor será notificado da alteração.

  2- Enviar mensagens:

Utilize as opções 1 ou 2 no cliente para enviar mensagens ao servidor.
Observação: As mensagens não devem exceder 100 caracteres.

  3- Simular erros de integridade:

No cliente, use a opção 3 para enviar pacotes com erro de checksum.
No servidor, use a opção 1 para simular erros nos ACKs.

  4- Simular perdas de pacotes:

No servidor, utilize a opção 2 para configurar quais pacotes serão perdidos.

  5- Atualizar janela de recepção dinamicamente:

No cliente, escolha a opção 5 para alterar o tamanho da janela de recepção.
O cliente ajustará seu envio de pacotes de acordo com o novo tamanho.

  5- Verificar integridade de pacotes:

No cliente, use a opção 6 para verificar o checksum de uma mensagem antes de enviá-la.

  6- Verificar status da janela de congestionamento:

No servidor, use a opção 3 para visualizar o tamanho atual da janela de congestionamento.


--- Detalhes do Protocolo ---

Formato do Pacote:

  -Cabeçalho:

Número de sequência (seq_num): Identifica o pacote de forma única.
Número de reconhecimento (ack_num): Utilizado para confirmar o recebimento de pacotes.
Tamanho da janela (window_size): Indica o tamanho atual da janela de recepção.
Flags (flags): Indicam o tipo de pacote (DATA, ACK, NACK, CONFIG).
Checksum (checksum): Usado para verificar a integridade do pacote.

  -Dados:

Conteúdo da mensagem (até 100 caracteres).

  -Flags:

FLAG_DATA (0b0001): Pacote de dados.
FLAG_ACK (0b0010): Acknowledgement (confirmação).
FLAG_NACK (0b0100): Negative Acknowledgement (confirmação negativa).
FLAG_CONFIG (0b1000): Configuração de protocolo.

  -Controle de Fluxo:

O cliente respeita o menor valor entre sua janela de congestionamento e a janela de recepção definida.

Controle de Congestionamento:
  -Cliente:
  
A janela de congestionamento aumenta com ACKs positivos.
Diminui com NACKs ou timeouts.

  -Servidor:
  
Ajusta sua janela de congestionamento com base nos pacotes recebidos e eventuais erros.

  -Detecção e Correção de Erros:

Utiliza checksum simples (XOR) para verificar a integridade dos pacotes.
Pacotes corrompidos são descartados, e o remetente é notificado para retransmissão.
