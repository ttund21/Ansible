# Registro Loops

## Loop

+ Loop pode ser usado para executar o mesmo módulo sem ter que criá-lo novamente;
+ O loop é representado pela variável **item** e pela chave **loop**;
+ Exemplo de uso:

  ```loop
      - name: Instalando pacotes usando loop
        apt:
          name: "{{ item }}"
        loop: "{{ packages }}"
 
      - name: Usando loop composto  
        apt:
          name: "{{ item.name }}"
          state: "{{ item.state }}"
        loop:
          - name: nano
            state: absent
          - name: vim 
            state: present

  ```
