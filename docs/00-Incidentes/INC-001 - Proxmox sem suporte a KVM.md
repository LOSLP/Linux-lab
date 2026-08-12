### INC-001 - Proxmox sem suporte a KVM

## Data

12/08/2026

## Sintoma

Inicialmente eu iria elaborar uma infraestrurua utilizando a aplicacao Proxmox VE, atraves da sua ISO instalada em uma VP, no entanto, me deparei com algumas dificuldades
apos a instalacao. 
I
Ao criar uma VM:

TASK ERROR: KVM virtualisation configured, but not available.

## Diagnóstico

Comando executado:

```bash
egrep -c '(vmx|svm)' /proc/cpuinfo
