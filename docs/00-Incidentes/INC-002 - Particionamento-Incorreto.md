## Particionamento Incorreto

## Data

22/10/2026 

## Sintoma

Durante a instalacao do Rocky Linux, SO do data center, eu nao me atendei em planejar a arquitetura de armazenamento e por esse descuido, acabei gerando um problema
que veio mais a frente.

Por padrao, o rocky linux criou duas particoes durante a instalacao, Sda1 contendo os arquivos do /bot, e Sda2 contendo os arquivos do /raiz, swap e /home.

Isso se tornou um problema, pois meu objetivo e ter o storage pool do incus isolado do resto do sistema, para facilitar os processos de backup, administracao dos containers, etc.

## Diagnóstico

Analisando mais a fundo como as partições foram criadas, observei que a Sda2 foi criada no formato LVM o que pode ser conveniente para mim, pois esse formato permite criar
volumes lógicos dentro dele e funcionam de forma isolada parecido com uma partição em si, o que poderia resolver meu problema, poderia apenas criar um volume logico dentro
do Sda2 dedicado ao Incus.

Mas então me veio o outro problema, observei que por padrão durante a instalação do SO, ele distribuiu todo o espaço de armazenamento da partição sda2 para os volumes logicos
que ele criou durante a instalação, /raiz, swap e /home. Sendo assim, não sobrou nada para eu utilizar na criação no volume logico dedicado ao Incus, e como eles foram criados
no formato xfs por padrão, não seria possível reduzir o tamanho deles, os volumes lógicos foram criados por padrão usando todo o espaço da partição, e desse espaço 
dedicado, eles são usavam quase nada.

### Comandos executados:

```bash
lsblk -f
```
```bash
pvs
```
```bash
vgs
```
```bash
lvs
```

## Correção 

Após um período de pensamento, tomei a decisão de reinstalar o sistema do data center, e durante a instalação, criar as partições da forma correta, usando o planejamento que
elaborei. Como não existem arquivos relevantes que não possam ser excluídos, dessa forma, perco menos tempo com backups desnecessários. 

Como outra possível solução, eu poderia apenas remover o volume logico do /home, faria o backup dele que que apenas de 32 kybtes, após a exclusão utilizaria o espaço dedicado
a ele para criar o volume logico do Incus, que seria de 70.5G, o que seria mais que suficiente para o laboratorio. Mas, como o meu objetivo e o aprendizado e ainda estou bem
no inicio do laboratorio, prefiro refazer da forma correta.

Nao considero minha decisao uma derrota tecnica, foi uma ótima experiencia de aprendizado, aprendi sobre analise de armazenamento, planejamento prévio das partições 
do servidor, formatos de particionamento, etc.

## Evidências

As imagens abaixo demonstram o estado do servidor durante o problema e após a resolução.

### Sintoma

<img width="725" height="190" alt="image" src="https://github.com/user-attachments/assets/384ef20e-fb8b-4b9c-92d3-251d157399ff" />

### Diagnóstico

<img width="1258" height="524" alt="image" src="https://github.com/user-attachments/assets/7bc4dcbc-fdf5-469d-b59f-af193042e016" />

### Correção

