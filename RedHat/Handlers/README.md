# Registro Handlers

## Handlers

+ Basicamente *handlers* são tasks que quando *notificadas* por outras tasks ela é executada;
+ Cada handler tem que ter seu nome único;
+ **Geralmente** o handler é usado para dar reboot e restartar serviços;
+ Para declarar um handler é necessário:
  + Declarar **handlers:**, após as "tasks:", para começar a criar as tasks;
  + Usar o **notify** no fim de uma task, seguido do nome do handler.
+ Exemplo:

  ```handler
    tasks:

      - name: Apache Installed
        apt:
          name: apache2
        notify: restart apache

      - name: Apache Activated
        service:
          name: apache2
          state: started
          enabled: true

      - name: Apache main page changed
        template:
          src: /home/vagrant/ansible/playbook/Handlers/index.html
          dest: /var/www/html/index.html
        notify: restart apache

      - name: Ping
        ping:

    handlers:

      - name: restart apache
        service:
          name: apache2
          state: restarted
  ```

### Informações

1. Handlers vai ser executado na ordem que foi declarado na sessão handlers, não na ordem que ele foi notificado;
2. Handler é executado após todas as tasks;
3. Sem dois handlers tiverem o mesmo nome, apenas um vai ser executado;
4. Se mais de uma task notificar um handler, o handler só será executa uma vez;
5. Se uma tasks não tiver o status changed na usa execução, o handler não vai ser executado.
