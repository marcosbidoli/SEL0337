## Prática 5: Configuração do SystemD para Personalização de Serviços de Inicialização de S.O. em Linux Embarcado, Controle de Versão e Repositório de Códigos com Git e GitHub

### Objetivos
O principal objetivo dessa prática é automatizar uma ação/aplicação gerada por um script em python ao realizar a inicialização (boot) do sistema
operacional. Em outras palavras, é adicionar uma unidade de serviço localizada (systemd service unit) capaz de gerenciar a execução e inicialização
de determinado script no linux embarcado na SBC Raspberry Pi 3.

### Resumo teórico SystemD

O SystemD é um sistema de inicialização e gerenciamento de serviços para sistemas operacionais Linux. Ele substitui o tradicional sistema de
inicialização SysV e é responsável por gerenciar a inicialização do sistema, a execução de serviços e a supervisão de processos. O SystemD permite
que os serviços sejam iniciados de forma paralela, melhorando a eficiência do boot, e fornece ferramentas para gerenciar serviços, como o systemctl,
que permite iniciar, parar e monitorar serviços de forma simples e eficaz.

Os serviços no SystemD são definidos por arquivos de unidade, que descrevem como um serviço deve ser iniciado, parado e gerenciado. Esses arquivos
contêm informações sobre dependências, comandos de inicialização e outras configurações necessárias. Para isso, ele gerencia as dependências entre
serviços, garantindo que um serviço só seja iniciado quando todos os serviços dos quais ele depende estiverem prontos. O comando systemctl é a
interface principal para interagir com o SystemD. Ele permite iniciar, parar, reiniciar e verificar o status dos serviços, além de habilitar ou
desabilitar serviços para inicialização automática no boot.

### Prática realizada

Para a prática, os conceitos apresentados até então foram utilizados para criar um sistema capaz de compilar um programa em python, nesse caso,
capaz de fazer um LED oscilar a cada 1s, desde a ininicialização/boot da Raspberry Pi, sem a necessidade de alguém executá-lo via terminal.

Dessa maneira, o primeiro passo foi montar o circuito na protoboard com o LED, conectando o terminal negativo no GND do dispositivo e o positivo no
terminal GPIO18, ao qual, foi baseado para criar o script que faz o LED piscar ao inicializar a SBC e o script que cessa seu funcionamento ao 
desligá-la, conforme visto na Figura a seguir:

<div align="center">
Figura 1 - Circuito montado sobre a protoboard para parte 1
  
![Pratica5](https://github.com/user-attachments/assets/15fded69-dadd-40e9-8bdf-524c03fb723e)

</div>

Nesse contexto, foi necessário configurar o systemd do dispositivo juntamente do controle de serviços com systemctl. Para isso, seguiu-se os
seguintes passos:
* Foram criados os dois scripts em python para o funcionamento do projeto, o primeiro que faz o LED piscar a cada 1 segundo e o segundo que cessa
seu funcionamento (Os dois arquivos estão no repositório do GITHUB).
* Em seguida, foi criado o arquivo unit file, responsável por colocar um determinado serviço que à disposição do systemd, ou seja, os serviço
executar os dois arquivos em python, um ao ligar o dispositivo e outro ao desligá-lo.
* O próximo passo foi copiar o arquivo “blink.service” para o diretório do padrão de serviços do systemd, para que seja reconhecido
e esteja à disposição no momento de inicialização do sistema.
* Assim, o programa foi testado utilizando o comando "sudo systemctl start blink".
* Por fim, para habilitar o serviço e o LED comece a piscar já na inicialização da Raspberry Pi, foi utilizado o seguinte comando:
"sudo systemctl enable blink".

Ao reiniciar a Raspberry, foi possível visualizar o comportamento de oscilação do LED na bancada, assim como previsto. Além disso, ao desligar o 
dispositivo, o LED para de piscar, pois o segundo script (blinkstop.py) foi executado pelo systemD.


### Observação

A parte 2 do trabalho foi realizada em um horário extra-aula, através de um computador próprio que possui sistema Linux instalado, por isso
o arquivo obtido através do comando "git log" não consta com o nome da raspberry pi do laboratório
