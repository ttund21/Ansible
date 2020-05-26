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


