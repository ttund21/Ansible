# Registro Conditional

## Conditional

+ Usado para validar uma expressão, usando operadores lógicos booleanos;
+ A condicional é representada pela palavra-chave **when** seguida da expressão booleana;
+ Exemplos de usos:
  
  ```when
  vars:
    distros:
      - Ubuntu
      - Debian

  - name: Teste
    apt:
      name: "{{ web }}"
    when: web is defined

  - name: ping
    ping:
    when: ansible_facts.distribution in distros

  - name: Usando condicional and no modo lista
    debug:
      msg: "hello world"
    when:
      - true
      - false

  - name: Usando condicionais agrupadas
    debug:
      msg: "Agrupadas"
    when: ( true and true ) or ( true and false )
  ```

+ Operadores Lógicos:

| Operator        | Significado                                                  | Exemplo                                   |
|-----------------|--------------------------------------------------------------|-------------------------------------------|
| <=              | Menor ou igual que                                           | 5 <= 5, true                              |
| >=              | Maior ou igual que                                           | 6 >= 5, true                              |
| >               | Maior que                                                    | 10 > 9, true                              |
| <               | Menor que                                                    | 10 < 9, false                             |
| ==              | Igual a                                                      | 1 == 1, true                              |
| !=              | Diferente de                                                 | 1 != 1, false                             |
| is definied     | Variável existe                                              | var is definied                           |
| is not definied | Variável não existe                                          | var is not definied                       |
| true            | valor booleano para verdadeiro                               |                                           |
| false           | valor booleano para falso                                    |                                           |
| in              | Dentro de                                                    | Ubuntu in varList                         |
| not             | Inverte um valor                                             | not true, false                           |
| and             | Agrupa duas expressões, aonde as duas tem que ser verdadeira | true and true, true true and false, false |
| or              | Agrupa duas expressões, aonde uma tem que ser verdadeira     | true or false, true false or false, false |
