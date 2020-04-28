# Codigo N. 1

## Objetivo do Playbook

+ Provisionar uma estrutura básica com ansible; 
+ Diagrama da estrutura que será provisionada:

![Diagrama](Diagrama.jpg)

## Explicando o código

+ Neste tópico será explicado o código e cada módulo ansible será referenciado sua página na documentação;
+ É necessário saber um pouco de AWS para entender o que está sendo criado;
+ A explicação será dividida em blocos;
+ Cada bloco terá linhas marcadas por **\<\>**, que serão explicadas abaixo do código.

### Código

#### 1º Bloco:

  ```1
  ---
    - name: AWS #<1>
      hosts: localhost 
      gather_facts: false  
      vars: 
        - profile: aws #<2>
        - region: us-east-1
        - user: vagrant
        - group: vagrant
      vars_prompt: #<3>
        - name: state 
          prompt: "Escreva absent para destruir"
          private: false
  ```

+ **<1>**: Esse inicio é necessário seguir o passo a passo do [Amazon Web Services Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_aws.html) e instalar as dependências;
+ **<2>**: O profile é uma referência ao arquivo [.aws/credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) da AWS;
+ **<3>**: Os [prompts](https://docs.ansible.com/ansible/latest/user_guide/playbooks_prompts.html) são utilizados para receber informações do usuário e armazenar essa informação na variável declarada na linha *name:*, no caso variável *state*. 

#### 2º Bloco

  ```2
      tasks:

        - name: VPC Created
          ec2_vpc_net: #<1>
            name: AnsibleVPC
            cidr_block: 172.30.0.0/16
            region: "{{ region }}"
            profile: "{{ profile }}"
            tags:
              Name: AnsibleVPC
              Organization: AnsPlay
          register: vpcReg #<2>
  ```

+ **<1>**: Será criada uma rede virtual(VPC) na AWS, utilizando o módulo [ec2\_vpc\_net](https://docs.ansible.com/ansible/latest/modules/ec2_vpc_net_module.html);
+ **<2>**: O [register](https://docs.ansible.com/ansible/latest/user_guide/playbooks_variables.html#registering-variables) é utilizado para armazenar a saída do comando em variável. 

#### 3º Bloco:

  ```3
        - name: IGW Created 
          ec2_vpc_igw: #<1>
            vpc_id: "{{ vpcReg.vpc.id }}" #<2>
            region: "{{ region }}"
            profile: "{{ profile }}"
            tags:
              Name: AnsibleIGW
              Organization: AnsPlay
          register: igwReg
  ```

+ **<1>**: Será criado uma Internet Gateway para permitir a comunicação à internet, foi usado o módulo [ec2\_vpc\_igw](https://docs.ansible.com/ansible/latest/modules/ec2_vpc_igw_module.html);
+ **<2>**: Aqui será referenciado um atributo da variável de retorno [vpcReg](https://docs.ansible.com/ansible/latest/modules/ec2_vpc_net_module.html#return-values), a maioria dos módulos tem seu retorno de valores na sua documentação, lembrando que os módulos sempre retorna um conteúdo em JSON. 

#### 4º Bloco:

  ```4
        - name: Subnet Created
          ec2_vpc_subnet: #<1>
            vpc_id: "{{ vpcReg.vpc.id }}"
            cidr: 172.30.1.0/24
            map_public: true 
            region: "{{ region }}"
            profile: "{{ profile }}"
            az: us-east-1a
            tags:
              Name: AnsibleSubnet
              Organization: AnsPlay
          register: subReg
  ```

+ **<1>**: Será criado uma sub-rede, utilizando o módulo [ec2\_vpc\_subnet](https://docs.ansible.com/ansible/latest/modules/ec2_vpc_subnet_module.html).
