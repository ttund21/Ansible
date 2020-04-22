# Atividade

## Objetivo

+ Criar um *custom ansible fact* e exportar para o node;
+ Usar os *facts* criados em um playbook.

### Algumas informações

+ O *custom ansible fact* é armazenado no node que vai ser usado;
+ O ansible por default procura ele no seguinte caminho */etc/ansible/facts.d/*;
+ O arquivo tem que ser criado com o final *.fact* e escrito ou em *JSON* ou em *INI*.
