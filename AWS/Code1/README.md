# Codigo N. 1

## Objetivo do Playbook

+ Provisionar uma estrutura básica com ansible; 
+ Diagrama da estrutura que será provisionada:

![Diagrama](Diagrama.jpg)

## Explicando o código

+ Neste tópico será explicado o código e cada módulo ansible será referenciado sua página na documentação;
+ A explicação será dividida em blocos;
+ Cada bloco terá linhas marcadas por **\<\>**, que serão explicadas abaixo do código.

### Código

#### 1º Bloco:

  ```1
  ---
    - name: AWS <1>
      hosts: localhost 
      gather_facts: false  
      vars: 
        - profile: aws <2>
        - region: us-east-1
        - user: vagrant
        - group: vagrant
      vars_prompt: <3>
        - name: state 
          prompt: "Escreva absent para destruir"
          private: false
  ```

+ **<1>**: Esse inicio é necessário seguir o passo a passo do [Amazon Web Services Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html) e instalar as dependências;
+ **<2>**: O profile é uma referência ao arquivo [.aws/credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) da AWS;
+ **<3>**: Os [prompts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_prompts.html) são utilizados para receber informações do usuário e armazenar essa informação na variável declarada na linha *name:*, no caso variável *state*. 
