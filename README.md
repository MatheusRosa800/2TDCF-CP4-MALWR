# FIAP - 4° Checkpoint - Análise de Malware

## Introdução
Checkpoint realizado com o intuito de colocar em prática todos os conhecimentos sobre análise de malwares na matéria de Malware Analysis, ministrada pelo [Professor Charles Lomboni](https://www.linkedin.com/in/charleslomboni/).

## Case Summary

Recebemos um malware direcionado para a FIAP e precisamos que você nos ajude na análise. Não temos maiores informações sobre ele, mas gostariamos que você criasse um relatório com o que foi encontrado no artefato malicioso.

## Analysts

Análise e report feito por Matheus Rosa Colombo


## Static Analysis

Antes de tudo iniciei o processo de analise apartir da coleta de informações estaticas para obter melhor compreendimento do malware.

1.  Die

Ao jogar o malware no DIE é possivel ver que está packeado.

![image](https://github.com/user-attachments/assets/375d0f2d-e2f3-4327-9545-8559a4de2980)


### tirando com upx 

````upx.exe -d [arq]````

2. 4N4L Detector
   
Utilizei o 4n4l detector para coletar mais informações 

![image](https://github.com/user-attachments/assets/14e85bf6-67e0-4276-b962-8d4dab997134)

![image](https://github.com/user-attachments/assets/0671ff60-a4c6-4de1-9a6d-f8e0a91a919c)

![image](https://github.com/user-attachments/assets/e51811c1-a406-42ea-93d5-6bf1e75d545f)

![image](https://github.com/user-attachments/assets/5f75abd3-6e2f-4610-99b9-e5d4e87e1595)

![image](https://github.com/user-attachments/assets/4a7fc4bf-671a-4701-b7ed-e78413f784fd)



## Artefatos que chamaram minha atenção:  

1. Suspicious Functions:
   - CryptEncrypt
   - CryptDecrypt
   - IsDebuggerPresent
   - CreateFileA
   - WriteFile
   - VirtualAlloc
   - WriteProcessMemory

2. File Access:
   - Crypt32.dll
   - flag.txt
   - nslookup.exe
   
5. Intelligent String:
   - GetUserObjectInformationAUSER32.dll,
   - http://www.fiap.com.br
   - WinInet.dll
   - Winhttp.dll
   
Após pesquisar sobre as funções mais suspeitas e chamativas, tive a suspeita de se tratar de um malware que trabalha com injeção e provavelmente um infostealer local.


# Dynamic Analysis

Initial Access
Execution
Persistence
Privilege Escalation
Defense Evasion
Credential Access
Discovery
Lateral Movement
Collection

## Command and Control

tcp://123.45.6.171:1337


Exfiltration
Impact
Timeline
Diamond Model
Indicators
Detections
MITRE ATT&CK







