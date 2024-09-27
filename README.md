# FIAP - 4° Checkpoint - Análise de Malware

## Introdução
Checkpoint realizado com o intuito de colocar em prática todos os conhecimentos sobre análise de malwares na matéria de Malware Analysis, ministrada pelo [Professor Charles Lomboni](https://www.linkedin.com/in/charleslomboni/).

## Case Summary

Recebemos um malware direcionado para a FIAP e precisamos que você nos ajude na análise. Não temos maiores informações sobre ele, mas gostariamos que você criasse um relatório com o que foi encontrado no artefato malicioso.

## Analysts

Análise e report feito por Matheus Rosa Colombo


## Static Analysis

Antes de tudo iniciei o processo de analise apartir da coleta de informações estaticas para obter melhor compreendimento do malware.

# 1.  Die

Ao jogar o malware no DIE é possivel ver que está packeado.

![image](https://github.com/user-attachments/assets/375d0f2d-e2f3-4327-9545-8559a4de2980)


### tirando com upx 

````upx.exe -d [arq]````

# 2. 4N4L Detector
   
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
   
Após pesquisar sobre as funções mais suspeitas e chamativas, tive a suspeita de se tratar de um malware que trabalha com **injeção** e provavelmente um **infostealer** da máquina local.


# Dynamic Analysis

# 1. Procmon

Para começar com a análise dinâmica iniciei o procmon e passei um filtro do .exe.

![image](https://github.com/user-attachments/assets/8afc4e25-3c73-46b9-b975-9d27dbe67141)

![image](https://github.com/user-attachments/assets/04e51916-63b6-4ce2-bcd4-d3d38d8f290d)

![image](https://github.com/user-attachments/assets/b9e55e1e-4885-475f-943d-f79e67d3ee53)


Dali, parti para pesquisa de algumas chaves de registro e outras informações que apareciam.

# 2. Wireshark

executei novamente o malware e filtrei no wireshark para ver algumas requisicoes, e tem apenas comunicações DNS com a URL do site da fiap.

![image](https://github.com/user-attachments/assets/848c1952-2634-44af-88fc-0547e2dcd9f8)


# 3. IDA

Algumas funcoes encontradas no IDA. 

![image](https://github.com/user-attachments/assets/b75885e9-9305-40f5-abbc-7dddecf722db)

![image](https://github.com/user-attachments/assets/fbff2e64-60e5-4750-9119-bec564ca4de5)

![image](https://github.com/user-attachments/assets/884aec79-6634-4e05-9d12-822a962bb616)

### WriteProcessMemory:
Escreve dados em um processo especificado na memória. Normalmente é usado para manipular a memória de outro processo.

### HeapAlloc:
Aloca um bloco de memória do heap. Este heap pode ser criado pelo processo que o utiliza, e a função retorna um ponteiro para o bloco alocado.

### VirtualAllocEx:
Aloca ou reserva um espaço de memória no contexto de outro processo (geralmente usado para injeção de código ou manipulação da memória de outro processo).

### VirtualFreeEx:
Libera ou desaloca memória que foi previamente reservada ou alocada com VirtualAllocEx. Também opera no contexto de outro processo.

### GetProcessHeap:
Obtém o identificador (handle) do heap do processo chamador. O heap é onde a memória dinâmica é armazenada durante a execução de um programa.

### CreateProcess:
Cria um novo processo e um novo thread, o que permite a execução de programas no sistema.

### GetCurrentProcess:
Retorna um pseudo-handle para o processo atual. Esse pseudo-handle não precisa ser fechado, pois se refere ao próprio processo que chama a função.

### IsProcessorFeaturePresent:
Verifica se uma determinada funcionalidade do processador está disponível no sistema atual, como suporte a instruções específicas.

### FindResource:
Localiza um recurso (como uma string, imagem, etc.) dentro de um módulo carregado.

### SizeofResource:
Retorna o tamanho de um recurso em bytes após ele ter sido localizado com FindResource.

### GetCurrentProcessId:
Obtém o ID (identificador) do processo atual.

Algumas funções destas encontrada são utilizada para **Injeção de processos**


# 4. XDBG

Alem de utilizar o IDA, procurei tambem em outro debugger.

## Requisição de API 

Nas strings encontrei esse link de api que parecia estar incompleto. 

Também encontrei nas STRINGS que há um "key="

O palpite que tive é que ele incrementa essa api com a chave e o id.

![image](https://github.com/user-attachments/assets/ed1a7410-8a08-4e71-8d2a-03c1701410c0)
![image](https://github.com/user-attachments/assets/e50845e3-c69c-4049-b7a0-274cdcbbb9fe)


# Bypass

Não estava conseguindo achar mais nada então decidi partir para uma estratégia mais bruta. Comecei a bypassar tudo.

![image](https://github.com/user-attachments/assets/b2f9db2c-fead-4a25-8360-dfd7e8f77318)



# Credential Access
GetUserObjectInformationAUSER32.dll

A função GetUserObjectInformationA na biblioteca USER32.dll é utilizada para obter informações sobre objetos do Windows que pertencem à interface gráfica do usuário (GUI), como janelas e desktops.

## Command and Control

Possível c2: tcp://123.45.6.171:1337


# Conclusão 

O malware analisado se trata de um infostealer com propriedades de injeção de código e locação de memória. 


