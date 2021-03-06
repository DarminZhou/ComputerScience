# 索引数据结构是B+树
# MyISAM和InnoDB两个存储引擎的索引实现方式
## MyISAM索引实现

MyISAM引擎使用B+Tree作为索引结构，叶结点的data域存放的是数据记录的地址。
MyISAM中索引检索的算法为首先按照B+Tree搜索算法搜索索引，如果指定的Key存在，则取出其data域的值，然后以data域的值为地址，读取相应数据记录。

MyISAM的索引方式也叫做“非聚集”的

## InnoDB索引实现
InnoDB也使用B+Tree作为索引结构
InnoDB的数据文件本身就是索引文件。从上文知道，MyISAM索引文件和数据文件是分离的，索引文件仅保存数据记录的地址。
而在InnoDB中，表数据文件本身就是按B+Tree组织的一个索引结构，这棵树的叶结点data域保存了完整的数据记录。
这个索引的key是数据表的主键，因此InnoDB表数据文件本身就是主索引。

InnoDB主索引（同时也是数据文件）的示意图，可以看到叶结点包含了完整的数据记录。这种索引叫做聚集索引。
因为InnoDB的数据文件本身要按主键聚集，所以InnoDB要求表必须有主键（MyISAM可以没有），如果没有显式指定，
则MySQL系统会自动选择一个可以唯一标识数据记录的列作为主键，如果不存在这种列，则MySQL自动为InnoDB表生成一个隐含字段作为主键，
这个字段长度为6个字节，类型为长整形。

第二个与MyISAM索引的不同是InnoDB的辅助索引data域存储相应记录主键的值而不是地址。换句话说，InnoDB的所有辅助索引都引用主键作为data域。
