# LVM (Logical Volume Manager) no Linux

O **LVM (Logical Volume Manager)** no Linux é uma tecnologia que permite o gerenciamento flexível de espaço de armazenamento em discos rígidos. Ele oferece uma maneira avançada de organizar, gerenciar e utilizar discos físicos, proporcionando vantagens como redimensionamento dinâmico de volumes e a criação de grupos de discos que podem ser tratados como um único disco lógico.

## Principais Conceitos do LVM

### Physical Volume (PV)
Corresponde a uma partição ou disco físico no sistema, que é utilizado para o gerenciamento LVM. Um disco inteiro ou uma partição específica pode ser transformada em um **PV**.

### Volume Group (VG)
É um conjunto de **Physical Volumes** agrupados para formar um único volume lógico. Um **VG** pode ser visto como um "pool" de armazenamento que pode ser distribuído entre vários **Logical Volumes**.

### Logical Volume (LV)
Dentro de um **Volume Group**, você pode criar **Logical Volumes**, que funcionam como partições lógicas onde você pode criar sistemas de arquivos, montar diretórios, etc. A vantagem é que você pode redimensionar dinamicamente esses volumes sem afetar os dados.

### Extents
Cada **VG** é dividido em unidades menores chamadas **extents**, que são a menor unidade de armazenamento no LVM. O tamanho de cada extent é configurável, e os **Logical Volumes** são compostos por várias extents.

## Vantagens do LVM
- **Flexibilidade**: Você pode redimensionar, adicionar ou remover discos de um volume lógico sem interrupções ou perda de dados.
- **Snapshotting**: LVM permite criar "snapshots" de volumes lógicos, que são capturas instantâneas do estado de um volume em um determinado momento.
- **Gerenciamento de Espaço**: É possível adicionar mais discos a um **VG** para aumentar o espaço disponível, sem necessidade de reformatar ou alterar a estrutura dos dados.
