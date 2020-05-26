# Introdução ao Jinja2

## O que é?

+ Jinja é uma linguagem para criação de templates para o Python;
+ O ansible pode usar o *jinja2* para criar templates de arquivos;
+ O ansible o utiliza para referenciar variáveis no playbook.

## Usando delimitadores

+ Variáveis e expressões lógicas são alocadas entre tags, ou delimitadores.
+ Por exemplo, o template Jinja usa **{% Expressão %}** para expressões (por exemplo uma condicional ou um loop); 
+ Enquanto **{{ variável }}** é usada para exibir o valor de uma variável;
+ E **{# Comentário #}** usado para fazer comentários no seu arquivo;
+ Exemplo prático:
  
  ```jinja2exmaple
  {# Template #}
  {% for name in names %}
    Hello, {{ name }}
  {% endfor %}

  # Yaml file
  names:
    - Anna
    - Thiago
    - Damocles

  # Output
  
    Hello,Anna

    Hello,Thiago

    Hello,Damocles
  ```

## Criando um template Jinja2

+ Um template jinja é composto por vários elementos como: data, variáveis e expressões;
+ Essas variáveis e expressões serão substituidas pelos seus valores reais quando o template for renderizado;
+ As variáveis do template podem ser especificadas pelo playbook;
+ É possivel usar as variáveis **facts** que o ansible gera no template;
+ Um template não precisa carregar a extensão jinja (*.j2*), mas é uma boa prática por a extensão para facilitar a identificação do template. 

### Dando deploy nos templates jinja

+ Após a criação do template, com ansible podemos usar o o modulo *template*, para dar o deploy para o host gerenciado;
+ O exemplo a seguir irá mostrar um template simples e o deploy dele.

#### Exemplo

+ Jinja2 template:
  
  ```jinjaTemplate
  {# Jinja2 Template #}
  Ola, {{ usuario }},
 
  O senhor está usando o sistema operacional: {{ ansible_facts.distribution }}. {# Variavel do ansible facts #}
  ```

+ Playbook:

  ```playbookTemplate
  ---       
    - name: Renderizando Template Jinja
      hosts: node1
      become: True
      vars:
        - usuario: "João" # Variavel que está no template
      tasks:
          
        - name: Template Renderizado
          template:
            src: template.j2
            dest: /home/vagrant/HelloWorld.txt
  ...
  ```

+ Output:

  ```templateOutput
  Ola, João,

  O senhor está usando o sistema operacional: Ubuntu.
  ```

## Ansible Managed

+ Para evitar que alguém modifique um template implantado pelo ansible é uma boa prática por um comentário no inicio do arquivo, indicando que não é para ser editado manualmente;
+ Um jeito para fazer isso é usar o "Ansible Managed" que é uma string que pode ser armazenada em uma variável no arquivo **ansible.cfg**;
+ Descomente ou adicione a linha a abaixo no arquivo ansible.cfg:

  ```ansibleManaged
  ansible_managed = Ansible managed
  ```

+ E no inicio do seu template adicione a linha:

  ```ansibleManagedTemplate
  # {{ ansible_managed }}
  ```

+ Exemplo da saída:

  ```templateAnsibleManaged
  # Ansible managed
  Ola, João,
 
  O senhor está usando o sistema operacional: Ubuntu.
  ```

## Estruturas de controle

+ Com o Jinja pode-se usar estruturas de controles como loop e condicionais.

### Loops

+ Jinja2 usa o loop **for** para estruturas de repetição;
+ Exemplo:
  
  ```jinjaLoopFor
  # JINJA2
  {% for produto in compras %}
    {{ produto }}
  {% endfor %}

  # YAML FILE
  compras:
    - Maçã
    - Banana
    - Laranja

  # RENDER
    Maçã
    Banana
    Laranja
  ```

+ No exemplo acima o *loop for* irá pecorrer todos os itens da variável, do tipo lista, *compras*;
+ E irá exibir todos os valares através da variável *produto*.

### Condicionais

+ O jinja2 usa o **if** para testar uma condição, que lhe dar a possibilidade de adicionar x linha, caso a condição seja verdadeira;
+ Exemplo:

  ```jinjaIf
  # JINJA
  {% if numero == 0 %}
    A condição é verdadeira.
  {% endif %}

  # YAML FILE
  numero: 0

  # RENDER
    A condição é verdadeira.
  ```

+ No exemplo acima atraves do *if* o jinja testou que se a variável numero for igual a 0 exiba "A condição é verdadeira.";
+ Então numero é igual a 0 e foi exibida a frase.

## Filtros de variáveis
