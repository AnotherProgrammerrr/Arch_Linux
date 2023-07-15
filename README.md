# Guia de Instalação de Arch Linux
Este é um guia de instalação de Arch Linux que fiz para mim mesmo do futuro, mas que pode ser usado por outras pessoas, claro.

# Mudar o layout do teclado
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
Até onde entendo, swap serve para evitar que o sistema morra se ficar sem RAM, para definir isso vamos fazer o seguinte:
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
Essa parte é bem simples, não tem segredo algum, para root, boot e swap respectivamente, usamos os seguintes comandos:

```
mount /dev/sda3 /mnt
mount --mkdir /dev/sda1 /mnt/boot
swapon /dev/sda2
```

# Instalar o sistema e pacotes importantes
Vamos instalar o sistema agora, junto dele também vamos acrescentar alguns pacotes, como o nano, que é um editor de texto, que vai ser importante em breve, o networkmanager e entre outros, vamos fazer isso através do comando:

```
pacstrap -K /mnt base linux linux-firmware base-devel grub efibootmgr nano networkmanager
```

# Toques finais
Apesar de instalado, o sistema não está pronto para uso, nem temos um usuário ainda, então, utilizaremos os seguintes comandos para entrar no sistema e dar os toques finais:

```
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt
```

A partir desse ponto vai ser necessário utilizar o nano algumas vezes, quando houver um _ na frente de alguma informação, considere-a como uma linha editável para escrever ou usando o nano ou no terminal.

### Ajustar o relógio
```
ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
hwclock --systohc
```

### Adicionar um hostname
```
nano /etc/hostname
_hostname-desejado
```

### Adicionar senha para o usuário root
```
passwd
_senha-desejada
_senha-desejada
```

### Adicionar usuário
```
useradd -m -G wheel -s /bin/bash _nome-de-usuario-desejado
```

### Adicionar senha para o usuário
```
passwd _nome-de-usuario-desejado 
_senha-desejada
_senha-desejada
```

### Permitir que o usuário execute comandos de usuário root
Essa parte requer uma explicação, o motivo de ao adicionar o usuário termos escrito ```-G wheel``` é que queremos adicioná-lo ao grupo ```wheel``` para deixar essa etapa mais fácil, utilizando o nano, vamos abrir um arquivo e editá-lo para permitir que todos os usuários neste grupo sejam capazes de executar comandos como super usuários/ usuários root da seguinte forma:

```
EDITOR=nano visudo
```

Se necessário, substitua nano pelo outro editor de texto escolhido.

Ao fazer isso, devemos descer até a parte em que estiver escrito o seguinte: 

```
##Uncomment to allow members of group wheel to execute any command
# %wheel ALL=(ALL:ALL) ALL
```

E como se pede, vamos remover o comentário, removendo o ```#``` que está na frente de ```%wheel ALL=(ALL:ALL) ALL```, após isso, salvamos e saimos do arquivo.

### Habilitar o NetworkManager
```
systemctl enable NetworkManager 
```

### Instalar o grub
```
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

### Modificar o layout do teclado permanentemente
```
localectl set-keymap br-abnt2
```

### Sair e reiniciar
```
exit
umount -a
reboot 
```

# Final
No fim, isso é tudo, após isso o sistema deve estar corretamente instalado, processos como a instalação de um DE (desktop enviroment) não serão descritos aqui, pois a intenção do arquivo era servir de guia para instalar e o sistema, não um DE.
