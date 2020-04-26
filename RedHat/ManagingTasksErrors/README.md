# Registro Handling Task Failure

## Anatoções

### Ignore Errors

+ Por padrão, se uma task falha, toda execução é abortada;
+ Mas temos uma alternativa usando a palavra-chave **ignore_errors**, aonde tasks com erro serão ignoradas;
+ Exemplo:

  ```ignoreErrors
  - name: Um comando inexistente
    shell: blablabla
    ignore_errors: true
  ```

### Force Handlers

+ Por padrão, após uma execução falhar ela é abortada e os handlers não são executadas;
+ Uma alternativa a isso é usar a palavra-chave **force_handlers**, para que os handlers sejam executados mesmo após às falhas;
+ Exemplo:

  ```forceHandlers
  tasks:
    - name: Task para notificar o handler
      shell: /bin/true
      notify:

    - name: Uma task com falha
      shell: blablabla

  handlers:
    - name: handler
      debug:
        msg: "Howdy"
  ```

### Failed When

+ A palavra-chave **failed_when** pode ser utilizada para indicar uma condição de falha para a task;
+ Exemplo:

  ```failedWhen
  - name: Uma task com falha que vai ser pulada
    shell: blablabla
    register: command_output
    failed_when: "'FAILED!' in command_output.stderr"
  ```

### Changed When

+ A palavra-chave **changed_when** é usada para controlar quando uma task deve reportar o status *changed*;
+ Exemplo:

  ```changedWhen
  - name: Essa task sempre vai dar status ok
    shell: echo 'ping' > /home/vagrant/ping.txt
    changed_when: false
  ```

### Ansible blocks and Error handling 

+ No playbook a palavra-chave **block** são um agrupamento de tasks, que podem ser usadas com condicionais para controlar quando aquele bloco vai ser utilizado;
+ Ele também permite o controle de error usando as palavras-chave **rescue** e **always**;
+ Significados:
  + **block**: Define as tasks principais;
  + **rescue**: Define as tasks para ser executada se uma task definida no block falhar;
  + **always**: Define as tasks quem sempre vão ser executadas independente do sucesso ou falhas no block ou no rescue.
+ Exemplo:
  ```blockRescueAlways
      - name: Preparing OS
        block:
          - name: Packages
            apt:
              name: "{{ item }}"
              state: latest
            loop: "{{ package }}"

          - name: Purposeful error
            shell: blablabla

        rescue:
          - name: Revert package
            apt:
              name: "{{ item }}"
              state: absent
            loop: "{{ package }}"

        always:
          - name: Always Block
            debug:
              msg: "Howdy"
        when: ansible_facts.distribution == "Ubuntu"
  ```
