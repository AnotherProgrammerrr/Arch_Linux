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
