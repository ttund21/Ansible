# Registro Managing Files

## Anotações

### Modulos de arquivos comuns

|     Nome     |                                                Descrição do Modulo                                                                 |
|     ---      |                                                         ---                                                                        |
| file         | Usado para gerenciar arquivos e suas propriedades. Esse modulo é o principal para gerenciar permissões, atalhos e etc.             |
| copy         | Cópia um arquivo da máquina local ou da máquina remota para um local no host gerenciado.                                           |
| fetch        | Cópia um arquivo do host gerenciado para à máquina local.                                                                          |
| synchronize  | Copia o arquivo e mantem as informções dentro dele. Semelhante ao rsync, mas não o substitui.                                      |
| stat         | Traz as informções de um determinado arquivo, similar ao comando stat do linux.                                                    |
| lineinfile   | Garante que aquele linha particular esteja no arquivo, ou sobrepõe uma linha existente usando regex.                               |
| blockinfile  | Este módulo irá inserir / atualizar / remover um bloco de texto de várias linhas cercado por linhas de marcador personalizáveis.   |

### Exemplos

#### File
  
  ```file
  - name: Touch a file a set permissions
    file:
      path: /home/vagrant/file.txt
      owner: vagrant
      group: vagrant
      mode: 0660
      state: touch # like a touch /home/vagrant/file.txt
  ``` 

+ Cria um arquivo chamado file.txt dentro da pasta home do usuario vagrant;
+ Seta as permissões de write e read paro o dono e o grupo, parecido com o chmod do linux;
+ Altera o dono e o grupo para vagrant, parecido com o comando chown do linux.

#### Copy

  ```copy
  - name: Copy a file copy.txt to node1
    copy:
      src: copy.txt
      dest: /home/vagrant/copy.txt
      owner: vagrant
      group: vagrant
      force: false # Não irá forçar a sobreposição
  ```

+ Cópia o arquivo copy.txt para a pasta home do usuário vagrant;
+ Altera o dono e o grupo para vagrant, parecido com o comando chown do linux.

#### Fetch

  ```fetch
  - name: Retrieve node1.file from node1
    fetch:
      src: "/home/vagrant/node1.file"
      dest: "/home/vagrant/ansible/RedHat/ManageFile"
  ```

+ Copia o arquivo node.file do host remoto para a máquina local.

#### Lineinfile

  ```lineinfile
  - name: Add additional lines to a file.txt
    lineinfile:
      path: /home/vagrant/file.txt
      line: 'Adicionando uma linha'
      state: present
  ```

+ Vai adicionar a linha "Adicionando uma linha" no arquivo file.txt;
+ Se a linha ja existir no arquivo ele retornará um ok.

#### Blockinfile

  ```blockinfile
  - name: Add additional lines to a file.txt
    blockinfile:
      path: /home/vagrant/file.txt
      block: | # Usado para quebrar linhas
        Uma linha
        Duas linhas
        Três linhas
      state: present
  ```

+ Vai adicionar um bloco de texto no arquivo file.txt;
+ Se o bloco ja existir no arquivo ele retornará um ok.

#### Stat

  ```stat
  - name: Verify the checksum of a file
    block:
      - name: Using stat
        stat:
          path: /home/vagrant/file.txt
          checksum_algorithm: md5
        register: statResult

      - name: Checksum
        debug:
          msg: "The checksum of the file is {{ statResult.stat.checksum }}"
  ```

+ Vai retornar o checksum do arquivo file.txt.

#### Synchronize

  ```synchronize
  - name: Synchronize local file to remote host
    synchronize:
      src: synchronize.txt
      dest: /home/vagrant/synchronize.txt
  ```

+ Copia e mantém o conteúdo do arquivo synchronize.txt para pasta home do usuário vagrant.
