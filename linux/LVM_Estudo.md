## Passo a passo para trabalhar com o LVM
Para trabalhar com o LVM, você precisa inicializar os discos para o LVM, depois criar pelo menos um grupo de Volumes, criar pelo menos um volume lógico no grupo que acabou de criar, formatar o volume com o sistema de arquivos desejado e por último montá-lo.

Resumidamente, é preciso fazer os seguintes passos:

Se for utilizar apenas uma partição de um disco, é preciso criá-la com o fdisk ou parted e mudar o seu tipo para 8e (LVM), caso contrário, basta usar todo o disco com o pvcreate;
Inicializar os volumes físicos (partições ou discos) com o comando pvcreate;
Criar um volume group com o comando vgcreate;
Ativar um volume group com o comando vgchange;
Criar um volume lógico com o comando lvcreate;
Formatar o volume lógico com o sistema de arquivos desejado com o mkfs.
Montar o volume lógico com o mount.
## Criando partições LVM
Se você não deseja utilizar todo o disco como LVM, pode usar o fdisk para criar uma ou mais partições tipo LVM. Para criar uma partição LVM com o fdisk, você cria a partição normalmente e altera o tipo dela para 8e.

Para exemplificar, imaginemos que  o sistema foi instalado no disco /dev/sda.

E para o LVM serão utilizados mais dois discos:  /dev/sdb e /dev/sdc que não estão particionados.

Antes de adicionar um disco ou partição como um volume físico do LVM é preciso inicializá-lo com o comando pvcreate.
## Caso precise formatar :
```
mkfs.ext4 /dev/sdb
```
## Inicializando volumes físicos
Para inicializar volumes físicos de discos inteiros o comando é: pvcreate e o caminho completo da partição ou disco:

```
# pvcreate /dev/sdb
```

```Physical volume “/dev/sdb” successfully created
# pvcreate /dev/sdc
```
Physical volume “/dev/sdc” successfully created
## Criando um volume group
Depois de inicializar os discos, é preciso criar um grupo de volume com os discos com o comando vgcreate:

```
# vgcreate meuvolume /dev/sdb /dev/sdc
```
  Volume group “meuvolume” successfully created
## Ativando um volume group
Após criar o volume group, é necessário ativá-lo com o comando vgchange:

```
# vgchange -a y meuvolume
```
  0 logical volume(s) in volume group “meuvolume” now active
Após o reboot do sistema é necessário ativar o volume group novamente. Então, faz-se necessário incluir esse comando nos scripts de carga do sistema.

## Criando volumes lógicos
O comando lvcreate cria volumes lógicos. No exemplo será criado  um volume lógico de 1GB chamado logico1 no volume meuvolume:

```
# lvcreate -L 1000 -n logico1 meuvolume
```
Logical volume “logico1” created
Como no nosso exmplo os discos  /dev/sdb e /dev/sdc têm 2GB cada um, é possível criar até 4 volumes de 1GB cada, ou 1 só volume lógico de 4GB, como no exemplo abaixo:

```
# lvcreate -L 4000 -n logico1 meuvolume
```
  Logical volume “logico1” created
## Ativando o volume lógico
O comando lvchange ativa / desativa o volume lógico para uso:

Para ATIVAR:

```
# lvchange -a y /dev/meuvolume/logico1
```
Para DESATIVAR:

```
# lvchange -a n /dev/meuvolume/logico1
```
## Formatando o volume lógico
Qualquer sistema de arquivos pode ser usado para formatar o volume lógico:

```
# mkfs.ext4 /dev/meuvolume/logico1
```
mke2fs 1.41.14 (22-Dec-2010)
Filesystem label=
OS type: Linux
(…)
Depois de formatar o volume lógico, é necessário montá-lo:

```
# mount /dev/meuvolume/logico1 /mnt
```
Após esses passos o volume lógico estará pronto para uso.

Você também pode usar o LVM para aumentar ou diminuir o tamanho de um volume.

## Aumentando o tamanho do volume com um disco novo
Primeiro é necessário criar o volume físico:

```
# pvcreate /dev/sdd
```
Atribuí-lo ao grupo:

```
# vgextend meugrupo /dev/sdd
```
Desmontar o volume lógico:

```
# umount /dev/meuvolume/logico1
```
Aumente o grupo de volume lógico:

```
# lvextend -L +13090M /dev/meuvolume/logico1
```
Procurar por erro e reparação do mesmo:

```
# e2fsck -f /dev/meuvolume/logico1
```
Finalmente, redimensionamos:

```
# resize2fs /dev/meuvolume/logico1
```
Agora basta montar novamente:

```
# mount /dev/meuvolume/logico1 /mnt
```