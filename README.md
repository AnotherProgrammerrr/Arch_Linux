# Arch Linux
Este é um guia de instalação de Arch Linux que fiz para mim mesmo do futuro, mas que pode ser usado por outras pessoas, claro.

# Carregar Teclado
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

## Partição de boot
Acredito que um tamanho adequado para uma partição de boot seja de pelo menos uns 512MB, para fazer isso utilizamos a seguinte sequência: 
```
n     - Para criar a nova partição
Enter - Caso queira especificar o tipo de partição que deseja criar, escolha p para primária ou e para extendidade.
Enter - Usar o número de partição padrão deixa esse guia muito mais fácil, então não vamos mexer nisso.
Enter - Acredito que não há nenhum tipo de interesse em especificar o setor inicial da partição, então vamos apertar enter para pular essa parte.
+512M - Para determinar o tamanho, usamso M para mb, G para gigas e T para tera
```

## Partição swap
