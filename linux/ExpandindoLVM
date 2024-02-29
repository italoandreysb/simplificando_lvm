## Expandindo disco LVM
Validado por mim

Para expandir um Logical Volume (LV) em um Volume Group (VG) LVM (Logical Volume Manager) no Ubuntu, siga os passos abaixo. Antes de prosseguir, é importante fazer backup dos dados importantes, pois qualquer operação de gerenciamento de volume pode ter riscos.

Verifique o espaço disponível:
Primeiro, verifique se há espaço livre suficiente no Volume Group (VG) que você deseja usar para expandir o Logical Volume (LV). Para isso, execute o comando:

```
sudo vgs
```
Isso mostrará informações sobre todos os Volume Groups e o espaço disponível em cada um.

Verifique o caminho do volume a ser expandido:
Use o comando lsblk para identificar o caminho do volume que você deseja expandir. Normalmente, o caminho é /dev/{nome_do_grupo}/{nome_do_logical_volume}.

Faça a expansão do Logical Volume:
Agora que você tem o espaço disponível e o caminho do volume a ser expandido, use o comando lvresize para aumentar o tamanho do Logical Volume.

Por exemplo, se você deseja adicionar 5 GB ao volume, o comando seria:

```
sudo lvresize -L +5G /dev/nome_do_grupo/nome_do_logical_volume
```

Isso aumentará o tamanho do Logical Volume em 5 GB. Se você quiser especificar um tamanho absoluto, em vez de um incremento, use -L seguido pelo tamanho desejado, como por exemplo:

```
sudo lvresize -L 20G /dev/nome_do_grupo/nome_do_logical_volume
```
Expanda o sistema de arquivos:
Agora que o Logical Volume foi expandido, você precisa expandir o sistema de arquivos que ele contém. O comando a ser usado depende do tipo de sistema de arquivos.
Se for um sistema de arquivos ext2, ext3 ou ext4, você pode usar o seguinte comando para redimensioná-lo para que ele use todo o espaço disponível no Logical Volume:

```
sudo resize2fs /dev/nome_do_grupo/nome_do_logical_volume
```

Para outros tipos de sistema de arquivos, como xfs, use o comando específico para redimensionamento desse sistema de arquivos.

Verifique o novo tamanho:
Para confirmar que o redimensionamento foi realizado com sucesso, use o comando df para exibir informações sobre o sistema de arquivos e o espaço usado:

```
df -h
```

Verifique se o sistema de arquivos tem o tamanho desejado e se há espaço livre suficiente.

Pronto! Agora o Logical Volume foi expandido e o sistema de arquivos foi redimensionado para usar o espaço adicional. Lembre-se sempre de tomar cuidado ao realizar operações de gerenciamento de volume e ter certeza de que você está expandindo os volumes corretos.