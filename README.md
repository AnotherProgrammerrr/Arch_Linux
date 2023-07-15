# Arch Linux
Este é um guia de instalação de Arch Linux que fiz para mim mesmo do futuro, mas que pode ser usado por outras pessoas, claro.

# Escolher o teclado
Antes de mais nada, é interessante modificar o teclado que está sendo usado, somente para evitar problemas para encontrar alguns caracteres.

```
loadkeys br-abnt2
```

# Formatar e montar patições
Para formatar as partições, usaremos o fdisk para evitar erros que poderiam ocorrer no caso do uso de outra ferramenta.

```
fdisk /dev/sda
```

De acordo com as últimas instalações e pela ausência de erros resultantes do uso disso, a gente vai criar uma tabela de partições MBR, para isso, basta digitar ``` o ```.

### Partição de boot
Acredito que um tamanho adequado para uma partição de boot seja de pelo menos uns 512MB, para fazer isso utilizamos a seguinte sequência: 
```
n     - Para criar a nova partição
Enter - Caso queira especificar o tipo de partição que deseja criar, escolha p para primária ou e para extendidade.
Enter - Usar o número de partição padrão deixa esse guia muito mais fácil, então não vamos mexer nisso.
Enter - Como não há por que especificar o setor inicial da partição, enter para usar o padrão.
+512M - Isso é basicamente para determinar o tamanho, usamos M para mb, G para gigas e T para tera
```

### Partição swap
Até onde entendo, swap server para evitar que o sistema so morra se ficar sem RAM, para definir isso vamos fazer o seguinte:
```
n 
Enter
Enter
Enter
+2G   - Geralmente é recomendado que swap tenha o dobro de RAM que a sua máquina vai utilizar, aumente se quiser.
```

### Partição root
Aqui vão ficar todos os arquivos do usuário, acho interessante usar o disco inteiro, dessa forma:
```
n 
Enter
Enter
Enter
Enter
```

# Sistemas de arquivos
É necessário designar os sistemas de arquivos dos partições corretamente para garantir o funcionamento, fazemos isso da seguinte forma:

### Sistemas de arquivos do root 
```
mkfs.ext4 /dev/sda3
```

### Sistema de arquivos do swap
```
mkswap /dev/sda2
```

### Sistema de arquivos do boot
```
mkfs.fat -F 32 /dev/sda1
```

Não vou me aprofundar muito nas explicações aqui, mas essa parte é essencial e não deve ser pulada.

# Montar as partições
