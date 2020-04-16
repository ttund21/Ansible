# Atividade

## Objetivo

+ Criar um usuário via playbook, mas armazenar as variaveis *username e password* em um arquivo encriptado via **ansible-vault**;

### Alguns comandos

+ ansible-vault \[encrypt | decrypt | edit | remove\] nomeDoArquivo;
  + encriptar um arquivo;

+ ansible all -i localhost, -m debug -a "msg={{ 'senha' | password_hash('sha512', 'titulo') }}"; 
  + encriptar uma string;
