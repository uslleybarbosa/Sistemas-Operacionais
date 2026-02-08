Para fixar o conteúdo da Unidade 02, vou compartilhar conhecimentos sobre **processos e threads**.

### O que são processos e threads?

- **Processo** é um programa em execução, com seu próprio espaço de memória e recursos.  
- **Thread** é uma linha de execução dentro de um processo. Um processo pode ter várias threads, que compartilham a mesma memória e recursos, mas cada uma executa de forma independente.

Isso traz vantagens de desempenho, pois criar uma thread é mais rápido do que criar um novo processo (que exige alocação de memória e inicialização completa).  

As threads compartilham o espaço de memória do processo, mas cada uma mantém sua própria pilha e registradores. Assim, podem executar tarefas diferentes sem interferir diretamente umas nas outras, mesmo trabalhando dentro do mesmo processo.

Quando um processo é interrompido e depois retomado, o sistema usa o PCB (Process Control Block) para restaurar seu estado e continuar a execução. Da mesma forma, cada thread possui seu próprio TCB (Thread Control Block), que guarda as informações necessárias para que ela possa ser pausada e retomada corretamente. 

Precisamos tomar cuidado porque apenas **um processo ou thread por vez** pode acessar a **região crítica**.  
A região crítica é o trecho de código que acessa **recursos compartilhados** (como variáveis globais, arquivos ou dispositivos).  

Se o controle de acesso não for feito corretamente, dois processos podem entrar ao mesmo tempo e isso gera **falhas**, como corrupção de dados ou resultados inconsistentes.

**Exclusão mútua**: garante que apenas um processo ou thread por vez possa acessar a **região crítica**, evitando corrupção de dados.  

Se não houver exclusão mútua, dois processos podem entrar ao mesmo tempo na região crítica e modificar recursos compartilhados de forma incorreta.  

Já o **deadlock** é outro problema: ocorre quando processos ficam bloqueados esperando recursos que nunca serão liberados, impedindo a continuidade da execução.

Sincronização de condicional: é um mecanismo que garante que um processo ou thread só entre na região crítica quando uma condição específica for satisfeita. 

**Variável de impedimento (lock variable)**: é uma técnica em que um processo só acessa a **região crítica** se a variável `lock` estiver livre (por exemplo, `lock = 0`).  

O problema é que essa verificação e atualização não são **atômicas**. Assim, dois processos podem ver `lock = 0` ao mesmo tempo e entrar juntos na região crítica, causando **condição de corrida** e corrupção de recursos.  

Por isso, a variável de impedimento isolada não garante exclusão mútua de forma confiável.

**Alternância obrigatória**: é um algoritmo de exclusão mútua que funciona apenas para dois processos.  
Ele utiliza uma variável chamada `turn`, que indica de quem é a vez de acessar a região crítica.  

Exemplo:  
- Se `turn = 0`, o processo A pode entrar.  
- Se `turn = 1`, o processo B pode entrar.  

O problema é que, se o processo B não quiser entrar na região crítica, o processo A fica bloqueado esperando sua vez, mesmo sem necessidade. Isso gera **espera desnecessária** e torna o algoritmo ineficiente.

**Solução de Peterson**: funciona de forma semelhante à alternância obrigatória, mas adiciona uma variável `flag[]` para indicar a intenção de cada processo de entrar na região crítica.  

- A variável `turn` continua existindo para indicar de quem é a vez.  
- A `flag[]` resolve o problema da espera desnecessária, pois um processo só entra se o outro não estiver interessado ou se for realmente sua vez.  

Assim, o algoritmo garante exclusão mútua, evita corrupção de dados e corrige a limitação da alternância obrigatória.

**TSL (Test-and-Set Lock)**: é uma instrução de hardware que combina em uma única operação atômica o teste e a atualização de uma variável de lock.  

Ela garante que apenas um processo por vez consiga alterar o lock de 0 para 1 e entrar na região crítica.  
Assim, mesmo que dois processos tentem acessar ao mesmo tempo, apenas um terá sucesso, evitando condição de corrida e garantindo exclusão mútua.

Observe que nos algoritmos clássicos de exclusão mútua, mesmo quando há operações atômicas, os processos ainda ficam em **espera ocupada (busy waiting)**, consumindo CPU de forma desnecessária.  

Nos sistemas atuais, utilizamos mecanismos como **sleep/wakeup**: quando um processo não pode acessar a região crítica, ele é colocado em estado de **bloqueado** (dormir) e não consome CPU. Quando o recurso é liberado, o sistema operacional envia um sinal de **wakeup** para acordar o processo e permitir que ele continue a execução.

**Escalonamento de processos**: é a função do sistema operacional responsável por organizar a execução dos processos, decidindo **qual processo terá acesso à CPU** (ou outro recurso) e por quanto tempo.

Critérios de escalonamento:

- **Throughput**: número de processos concluídos em um intervalo de tempo.  
- **Utilização da CPU**: porcentagem de tempo em que a CPU está ocupada executando processos.  
- **Waiting time (tempo de espera)**: tempo total que um processo passa esperando na fila de pronto antes de ser executado.  
- **Turnaround time (tempo de retorno)**: tempo total desde a submissão do processo até sua conclusão.  
- **Response time (tempo de resposta)**: tempo entre a submissão de um processo e a produção da primeira resposta.

**Políticas de escalonamento:**

- **Não preemptivo**: o processo que entra na CPU só sai quando termina ou bloqueia.  
- **Preemptivo**: o processo pode ser interrompido antes de terminar, permitindo alternância.  
- **FIFO/FCFS (First Come, First Served)**: o primeiro processo que chega é o primeiro a ser executado.  
- **SJF (Shortest Job First)**: o processo com menor tempo de execução tem prioridade.  
- **Round Robin (RR)**: cada processo recebe um quantum fixo de tempo na CPU, em ciclos.  
- **HRRN (Highest Response Ratio Next)**: calcula prioridade pelo tempo de espera e execução, evitando starvation.

**Threads a nível de usuário (User-level threads)**: gerenciadas por bibliotecas no espaço do usuário. São leves e rápidas, mas o kernel não sabe que existem várias threads. Se uma thread bloquear, todo o processo fica bloqueado.  

**Threads a nível de kernel (Kernel-level threads)**: gerenciadas diretamente pelo sistema operacional. São mais pesadas, mas permitem que o kernel controle e escalone cada thread individualmente, aproveitando melhor múltiplos núcleos e evitando bloqueios globais.

**Modelos de mapeamento de threads:**

- **Many-to-One**: várias threads de usuário são vistas como uma única thread de kernel.  
- **One-to-One**: cada thread de usuário é espelhada em uma thread de kernel.  
- **Many-to-Many**: várias threads de usuário são mapeadas para várias threads de kernel, de forma dinâmica.  
- **Two-Level Model**: híbrido; algumas threads de usuário são mapeadas diretamente para threads de kernel, outras são gerenciadas em grupo.

Finalizamos aqui a Unidade 02. Espero que este conteúdo tenha contribuído para o seu aprendizado e fortalecido sua compreensão sobre processos e threads. Bons estudos e até a próxima!
