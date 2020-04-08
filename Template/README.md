# Laboratórios

## O que é o [Vagrant](https://www.vagrantup.com/)?
+ Resumidamente o vagrant é um automatizador de criação de VMs, através de código.

## COMANDOS UTEIS

#### 1. Iniciando o VagrantFile.
+ $ vagrant up [nome da maquina] --provision --> Vai executar o Vagrantfile e o **--provision** vai executar o shell script que está dentro do Vagrantfile, uso quando vou executar o Vagrantfile pela primeira vez;
+ $ vagrant up [nome da maquina] --no-provision --> Vai executar o Vagrantfile e o **--no-provision** não vai executar o shell script que está dentro do Vagrantfile, uso quando o o status da máquina está em **poweroff**.

#### 2. Desligar.
+ $ vagrant halt [nome da maquina]

#### 3. Destruir.
+ $ vagrant destroy [nome da maquina]

#### 4. Provisionar o shell script.
+ $ vagrant provision [nome da maquina]

#### 5. Atualizar alterações no Vagrantfile.
+  $ vagrant reload [nome da maquina]

#### 6. Ver o status da máquina.
+ $ vagrant status [nome da maquina]

#### Obs:
+ Caso não especifique a maquina depois do comando, ele vai executar o comando em todas as máquinas.
