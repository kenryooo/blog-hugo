+++
title = "Ubuntu 14.04 LTS で 2TB 超の HDD を使う"
date = "2017-01-09T16:39:33+09:00"

+++

年末年始のテレビ番組録画に備えて、録画マシンに HDD を増設したときのメモ。  
2TB 超のディスクを扱うときは GNU Parted を使う。

### 環境

OS: Ubuntu 14.04 LTS  
HDD: Seagate ST4000DM000


### 手順

GNU Parted インストール。

```bash
$ sudo apt-get install gparted
```

パーティションを作成する。

```bash
$ ll /dev/disk/by-id/
lrwxrwxrwx 1 root root   9 12月 23 17:09 ata-ST4000DM000-2AE166_WDH0YZJ4 -> ../../sda  # パーティションを作成する先を確認

$ sudo parted /dev/sda
GNU Parted 2.3
/dev/sda を使用
GNU Parted へようこそ！ コマンド一覧を見るには 'help' と入力してください

(parted) mklabel gpt
(parted) unit GB
(parted) mkpart
パーティションの名前?  []?                                      
ファイルシステムの種類?  [ext2]? ext4
開始? 0
終了? 4001
(parted) print
モデル: ATA ST4000DM000-2AE1 (scsi)
ディスク /dev/sda: 4001GB
セクタサイズ (論理/物理): 512B/4096B
パーティションテーブル: gpt

番号  開始    終了    サイズ  ファイルシステム  名前  フラグ
 1    0.00GB  4001GB  4001GB
(parted) quit
通知: 必要であれば /etc/fstab を更新するのを忘れないようにしてください。

$ ll /dev/disk/by-id/ | grep sda
lrwxrwxrwx 1 root root   9 12月 23 20:25 ata-ST4000DM000-2AE166_WDH0YZJ4 -> ../../sda
lrwxrwxrwx 1 root root  10 12月 23 20:25 ata-ST4000DM000-2AE166_WDH0YZJ4-part1 -> ../../sda1
lrwxrwxrwx 1 root root   9 12月 23 20:25 wwn-0x5000c5009d4b7807 -> ../../sda
lrwxrwxrwx 1 root root  10 12月 23 20:25 wwn-0x5000c5009d4b7807-part1 -> ../../sda1
```

ファイルシステムを作成する。

```bash
$ sudo mkfs -t ext4 /dev/sda1
mke2fs 1.42.9 (4-Feb-2014)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
244195328 inodes, 976754176 blocks
48837708 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
29809 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks: 
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208, 
	4096000, 7962624, 11239424, 20480000, 23887872, 71663616, 78675968, 
	102400000, 214990848, 512000000, 550731776, 644972544

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done 
```

Parted が最後に教えてくれたとおりに fstab に設定を追加する。  

```bash
$ sudo blkid /dev/sda1 
/dev/sda1: UUID="<uuid>" TYPE="ext4"

$ vi /etc/fstab  # 以下の1行を追加
UUID=<uuid>       /media/video/volume2    ext4    defaults        1       2
```

再起動して自動マウントを確認。

```bash
$ df -h | grep sda1
/dev/sda1                    3.6T   68M  3.4T   1% /media/video/volume2
```

