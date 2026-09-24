## Bloqueio de acesso via su

## Data

23/08/2026

## Sintoma

Durante a configuração no arquivo utilizado pelo PAM para controlar o comportamento do comando `su`, devido a um erro que cometi quando criei a regra de bloqueio para impedir o usuário voyager de realizar elevação de privilégios para root através do comando `su`, acabei impossibilitando o uso do comando para acessar qualquer usuário.

## Diagnóstico

Como meu ambiente é uma VPS hospedada em IaaS e possui o recurso de VNC, utilizei esse recurso para acessar o servidor e logar diretamente como root e então buscar uma solução para o problema.
 
Depois de analisar os logs do servidor e a configuração que foi feita por mim nos arquivos do PAM, identifiquei que a falha ocorreu porque, quando fui configurar a linha de comando no PAM, devido à falta de conhecimento, não defini um comportamento para o caso de a regra não ser atendida.
 
Dessa forma, o PAM não sabia o que fazer e acabou bloqueando o usuário voyager de utilizar o comando `su` para acessar o usuário octavio também, e não apenas o root.
 
Como apenas o usuário voyager pode realizar login via SSH e eu precisaria acessar o usuário octavio para então poder virar root, acabei ficando preso no usuário voyager sem privilégios.

### Comando executado:

```bash
tail -f /var/log/secure | grep pam
```

## Correção 

Após estudar um pouco mais sobre o comportamento do PAM e realizar alguns testes, descobri como corrigir o problema.

Adicionei uma configuração para que, caso a regra que eu criei não fosse atendida, ela fosse ignorada.

Com isso, o comportamento do PAM passou a agir como o esperado, bloqueando apenas quando o usuário voyager tentasse acessar o root através do comando su e permitindo o acesso aos demais usuários do sistema, no caso o octavio.

## Evidências

As imagens abaixo demonstram o estado do servidor durante o problema e após a resolução.

### Sintoma

<img width="422" height="69" alt="image" src="https://github.com/user-attachments/assets/6a2cb978-546e-4aed-a8d2-e79b76e4ba08" />

### Diagnóstico

<img width="1551" height="46" alt="image" src="https://github.com/user-attachments/assets/4f00dab7-f685-42e6-a3c0-91115b1f8368" />

### Correção

<img width="1213" height="480" alt="image" src="https://github.com/user-attachments/assets/474aca3d-2348-46b6-abd7-c83acdcd8c96" />

