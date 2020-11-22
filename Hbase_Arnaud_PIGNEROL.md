# Apache HBase

# 1) HBase CLI

# 1.1 ) First commands
## 1.1.1 ) Basic commands
### Connect to HADOOP cluster using SSH.
```CMD
-sh-4.2$ ssh apignerol@hadoop-edge01.efrei.online
Welcome to EFREI Hadoop Cluster

Last login: Tue Nov 17 08:52:01 2020 from 65-101-97-185.ftth.cust.kwaoo.net
```

### Initialize a Kerberos ticket.
```CMD
-sh-4.2$ kinit
Password for apignerol@EFREI.ONLINE:
```

### Type the command hbase shell in the terminal prompt.
```CMD
-sh-4.2$ hbase shell

SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/hdp/3.1.5.0-152/hadoop/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/hdp/3.1.5.0-152/hbase/lib/client-facing-thirdparty/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
HBase Shell
Use "help" to get list of supported commands.
Use "exit" to quit this interactive shell.
For Reference, please visit: http://hbase.apache.org/2.0/book.html#shell
Version 2.1.6.3.1.5.0-152, rUnknown, Thu Dec 12 20:16:57 UTC 2019
Took 0.0038 seconds
hbase(main):001:0>
```

### Type these commands:
#### status
```CMD
hbase(main):001:0> status
1 active master, 1 backup masters, 3 servers, 0 dead, 3.3333 average load
Took 0.3347 seconds
```
#### version
```CMD
hbase(main):002:0> version
2.1.6.3.1.5.0-152, rUnknown, Thu Dec 12 20:16:57 UTC 2019
Took 0.0007 seconds
```
#### whoami
```CMD
hbase(main):003:0> whoami
apignerol@EFREI.ONLINE (auth:KERBEROS)
    groups: apignerol, hadoop-users
Took 0.0112 seconds
```
#### list
```CMD
hbase(main):004:0> list
TABLE
ATLAS_ENTITY_AUDIT_EVENTS
atlas_titan
ccarayon:library
vserena:library
4 row(s)
Took 0.0254 seconds
=> ["ATLAS_ENTITY_AUDIT_EVENTS", "atlas_titan", "ccarayon:library", "vserena:library"]
```
#### exit
```CMD
hbase(main):005:0> exit
```


## 1.1.2 ) Create your own namespace
```CMD
hbase(main):001:0> create_namespace 'apignerol'
Took 0.5838 seconds
```

## 1.1.3 ) Creating a table
### Create a table in your namespace.
### The tables must be called: library, and have thoses columns families:
#### - author with 2 versions.
#### - book with 3 versions.
```CMD
hbase(main):002:0> create 'apignerol:library', {NAME => 'author', VERSIONS => 2}, {NAME => 'book', VERSIONS => 3}

Created table apignerol:library
Took 1.5614 seconds
=> Hbase::Table - apignerol:library
```

### Describe the table structure.
```CMD
hbase(main):003:0> describe 'apignerol:library'

Table apignerol:library is ENABLED
apignerol:library
COLUMN FAMILIES DESCRIPTION
{NAME => 'author', VERSIONS => '2', EVICT_BLOCKS_ON_CLOSE => 'false', NEW_VERSION_BEHAVIOR => 'false', KEEP_DELETED_CELLS => 'FALSE', CA
CHE_DATA_ON_WRITE => 'false', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', MIN_VERSIONS => '0', REPLICATION_SCOPE => '0', BLOOMFILTE
R => 'ROW', CACHE_INDEX_ON_WRITE => 'false', IN_MEMORY => 'false', CACHE_BLOOMS_ON_WRITE => 'false', PREFETCH_BLOCKS_ON_OPEN => 'false',
 COMPRESSION => 'NONE', BLOCKCACHE => 'true', BLOCKSIZE => '65536'}
{NAME => 'book', VERSIONS => '3', EVICT_BLOCKS_ON_CLOSE => 'false', NEW_VERSION_BEHAVIOR => 'false', KEEP_DELETED_CELLS => 'FALSE', CACH
E_DATA_ON_WRITE => 'false', DATA_BLOCK_ENCODING => 'NONE', TTL => 'FOREVER', MIN_VERSIONS => '0', REPLICATION_SCOPE => '0', BLOOMFILTER
=> 'ROW', CACHE_INDEX_ON_WRITE => 'false', IN_MEMORY => 'false', CACHE_BLOOMS_ON_WRITE => 'false', PREFETCH_BLOCKS_ON_OPEN => 'false', C
OMPRESSION => 'NONE', BLOCKCACHE => 'true', BLOCKSIZE => '65536'}
2 row(s)
Took 0.1209 seconds
```



## 1.1.4 ) Adding Values
```CMD
hbase(main):001:0> put 'apignerol:library', 'vhugo', 'author:lastname', 'Hugo'
Took 0.4259 seconds
hbase(main):002:0> put 'apignerol:library', 'vhugo', 'author:firstname', 'Victor'
Took 0.0100 seconds
hbase(main):003:0> put 'apignerol:library', 'vhugo', 'book:title', 'La légende des siècles'
Took 0.0108 seconds
hbase(main):004:0> put 'apignerol:library', 'vhugo', 'book:category', 'Poèmes'
Took 0.0083 seconds
hbase(main):005:0> put 'apignerol:library', 'vhugo', 'book:year', '1855'
Took 0.0077 seconds
hbase(main):006:0> put 'apignerol:library', 'vhugo', 'book:year', '1877'
Took 0.0191 seconds
hbase(main):007:0> put 'apignerol:library', 'vhugo', 'book:year', '1883'
Took 0.0074 seconds
hbase(main):008:0> put 'apignerol:library', 'jverne', 'author:lastname', 'Jules'
Took 0.0108 seconds
hbase(main):009:0> put 'apignerol:library', 'jverne', 'author:firstname', 'Verne'
Took 0.0077 seconds
hbase(main):006:0> put 'apignerol:library', 'jverne', 'book:publisher', 'Hetzel'
Took 0.0304 seconds
hbase(main):010:0> put 'apignerol:library', 'jverne', 'book:title', 'Face au drapeau'
Took 0.0080 seconds
hbase(main):011:0> put 'apignerol:library', 'jverne', 'book:year', '1896'
Took 0.0118 seconds
```
#### There is an error, Verne is not the firstname but the lastname. However, it was asked like this in the instructions so I didn't change it


## 1.1.5 ) Counting Values
```CMD
hbase(main):001:0> count 'apignerol:library', CACHE => 1000
2 row(s)
Took 0.3788 seconds
=> 2
```


## 1.1.6 ) Retrieving Values

#### get namespace:library, ’vhugo’
```CMD
hbase(main):001:0> get 'apignerol:library', 'vhugo'
COLUMN                              CELL
 author:firstname                   timestamp=1605602231066, value=Victor
 author:lastname                    timestamp=1605602187660, value=Hugo
 book:category                      timestamp=1605602295320, value=Po\xC3\xA8mes
 book:title                         timestamp=1605602266861, value=La l\xC3\xA9gende des si\xC3\xA8cles
 book:year                          timestamp=1605602335314, value=1883
1 row(s)
Took 0.3969 seconds
```
#### get namespace:library, ’vhugo’, ’author’
```CMD
hbase(main):002:0> get 'apignerol:library', 'vhugo', 'author'
COLUMN                              CELL
 author:firstname                   timestamp=1605602231066, value=Victor
 author:lastname                    timestamp=1605602187660, value=Hugo
1 row(s)
Took 0.0207 seconds
```
#### get namespace:library, ’vhugo’, ’author:firstname’
```CMD
hbase(main):003:0> get 'apignerol:library', 'vhugo', 'author:firstname'
COLUMN                              CELL
 author:firstname                   timestamp=1605602231066, value=Victor
1 row(s)
Took 0.0086 seconds
```
#### get namespace:library, ’jverne’, COLUMN=>’book’
```CMD
hbase(main):007:0> get 'apignerol:library', 'jverne', COLUMN => 'book'
COLUMN                              CELL
 book:publisher                     timestamp=1605602988724, value=Hetzel
 book:title                         timestamp=1605602439823, value=Face au drapeau
 book:year                          timestamp=1605602464553, value=1896
1 row(s)
Took 0.0137 seconds
```
#### get namespace:library, ’jverne’, COLUMN=>’book:title’
```CMD
hbase(main):008:0> get 'apignerol:library', 'jverne', COLUMN => 'book:title'
COLUMN                              CELL
 book:title                         timestamp=1605602439823, value=Face au drapeau
1 row(s)
Took 0.0136 seconds
```
#### get namespace:library, ’jverne’, COLUMN=>[’book:title’, ’book:year’,’book:publisher’]
```CMD
hbase(main):009:0> get 'apignerol:library', 'jverne', COLUMN => ['book:title', 'book:publisher', 'book:year']
COLUMN                              CELL
 book:publisher                     timestamp=1605602988724, value=Hetzel
 book:title                         timestamp=1605602439823, value=Face au drapeau
 book:year                          timestamp=1605602464553, value=1896
1 row(s)
Took 0.0188 seconds
```
#### get namespace:library, ’jverne’, FILTER=>"ValueFilter(=, ’binary:Jules’)"
```CMD
hbase(main):010:0> get 'apignerol:library', 'jverne', FILTER => "ValueFilter(=, 'binary:Jules')"
COLUMN                              CELL
 author:lastname                    timestamp=1605602398920, value=Jules
1 row(s)
Took 0.0501 seconds
```


## 1.1.7 ) Tuple Browsing

### Display all the data in the table
```CMD
hbase(main):001:0> scan 'apignerol:library'

ROW                                 COLUMN+CELL
 jverne                             column=author:firstname, timestamp=1605602410346, value=Verne
 jverne                             column=author:lastname, timestamp=1605602398920, value=Jules
 jverne                             column=book:publisher, timestamp=1605602988724, value=Hetzel
 jverne                             column=book:title, timestamp=1605602439823, value=Face au drapeau
 jverne                             column=book:year, timestamp=1605602464553, value=1896
 vhugo                              column=author:firstname, timestamp=1605602231066, value=Victor
 vhugo                              column=author:lastname, timestamp=1605602187660, value=Hugo
 vhugo                              column=book:category, timestamp=1605602295320, value=Po\xC3\xA8mes
 vhugo                              column=book:title, timestamp=1605602266861, value=La l\xC3\xA9gende des si\xC3\xA8cles
 vhugo                              column=book:year, timestamp=1605602335314, value=1883
2 row(s)
Took 0.4344 seconds
```

### Display all the data from the book family
```CMD
hbase(main):001:0> scan 'apignerol:library', COLUMN => 'book'

ROW                                 COLUMN+CELL
 jverne                             column=book:publisher, timestamp=1605602988724, value=Hetzel
 jverne                             column=book:title, timestamp=1605602439823, value=Face au drapeau
 jverne                             column=book:year, timestamp=1605602464553, value=1896
 vhugo                              column=book:category, timestamp=1605602295320, value=Po\xC3\xA8mes
 vhugo                              column=book:title, timestamp=1605602266861, value=La l\xC3\xA9gende des si\xC3\xA8cles
 vhugo                              column=book:year, timestamp=1605602335314, value=1883
2 row(s)
Took 0.3515 seconds
```

### Display the year values of the book family present in the table
```CMD
hbase(main):002:0> scan 'apignerol:library', COLUMN => 'book:year'

ROW                                 COLUMN+CELL
 jverne                             column=book:year, timestamp=1605602464553, value=1896
 vhugo                              column=book:year, timestamp=1605602335314, value=1883
2 row(s)
Took 0.3520 seconds
```

### Scans the tuples by keys between a and n and the fields of the family author
```CMD
hbase(main):001:0> scan 'apignerol:library', COLUMN => 'author', STARTROW => 'a', STOPROW => 'n'

ROW                                 COLUMN+CELL
 jverne                             column=author:firstname, timestamp=1605602410346, value=Verne
 jverne                             column=author:lastname, timestamp=1605602398920, value=Jules
1 row(s)
Took 0.3493 seconds
```

### Display exactly the same, but with a filter
```CMD
hbase(main):002:0> scan 'apignerol:library', COLUMN => 'author', FILTER => "InclusiveStopFilter('n')"

ROW                                 COLUMN+CELL
 jverne                             column=author:firstname, timestamp=1605602410346, value=Verne
 jverne                             column=author:lastname, timestamp=1605602398920, value=Jules
1 row(s)
Took 0.3981 seconds```

### Display the author values: firstname
```CMD
```

### Displays the author values: firstname
```CMD
hbase(main):006:0> scan 'apignerol:library', FILTER => "ColumnPrefixFilter('firstname')"

ROW                                 COLUMN+CELL
 jverne                             column=author:firstname, timestamp=1605602410346, value=Verne
 vhugo                              column=author:firstname, timestamp=1605602231066, value=Victor
2 row(s)
Took 0.0330 seconds
```

### Searches for columns whose value equals the specified title
```CMD
hbase(main):014:0> scan 'apignerol:library', FILTER => "SingleColumnValueFilter('book', 'title', =, 'binary:Face au drapeau')"

ROW                                 COLUMN+CELL
 jverne                             column=author:firstname, timestamp=1605602410346, value=Verne
 jverne                             column=author:lastname, timestamp=1605602398920, value=Jules
 jverne                             column=book:publisher, timestamp=1605602988724, value=Hetzel
 jverne                             column=book:title, timestamp=1605602439823, value=Face au drapeau
 jverne                             column=book:year, timestamp=1605602464553, value=1896
1 row(s)
Took 0.0269 seconds
```

### Display the tuples whose (latest version of the) book column: date is less than or equal to 1890
```CMD
hbase(main):008:0> scan 'apignerol:library', {FILTER => "ValueFilter(>, 'binary:1890') AND ColumnPrefixFilter('year')"}

ROW                                 COLUMN+CELL
 jverne                             column=book:year, timestamp=1605602464553, value=1896
1 row(s)
Took 0.0132 seconds
```

### Searches for tuples whose key begins with jv and one of the values of which matches the regular expression
```CMD
hbase(main):013:0> scan 'apignerol:library', {FILTER => "PrefixFilter('jv') AND RowFilter(=, 'regexstring:^jv.*')"}

ROW                                 COLUMN+CELL
 jverne                             column=author:firstname, timestamp=1605602410346, value=Verne
 jverne                             column=author:lastname, timestamp=1605602398920, value=Jules
 jverne                             column=book:publisher, timestamp=1605602988724, value=Hetzel
 jverne                             column=book:title, timestamp=1605602439823, value=Face au drapeau
 jverne                             column=book:year, timestamp=1605602464553, value=1896
1 row(s)
Took 0.0285 seconds
```

## 1.1.8 ) Updating a value
### put namespace:library, ’vhugo’, ’author:lastname’, ’HAGO’
```CMD
Took 1.0350 seconds
hbase(main):002:0> put 'apignerol:library', 'vhugo', 'author:lastname', 'HAGO'
```

### put namespace:library, ’vhugo’, ’author:lastname’, ’HUGO’
```CMD
Took 0.0085 seconds
hbase(main):003:0> put 'apignerol:library', 'vhugo', 'author:lastname', 'HUGO'
Took 0.0072 seconds
```

### put namespace:library, ’vhugo’, ’author:firstname’, ’Victor Marie’
```CMD
hbase(main):004:0> put 'apignerol:library', 'vhugo', 'author:firstname', 'Victor Marie'
Took 0.0117 seconds
```

### put namespace:library, ’vhugo’, ’author:lastname’, ’Hugo’
```CMD
hbase(main):005:0> put 'apignerol:library', 'vhugo', 'author:lastname', 'Hugo'
Took 0.0078 seconds
```

### get namespace:library, ’vhugo’, ’author’
```CMD
hbase(main):006:0> get 'apignerol:library', 'vhugo', 'author'

COLUMN                              CELL
 author:firstname                   timestamp=1605609930645, value=Victor Marie
 author:lastname                    timestamp=1605609936241, value=Hugo
1 row(s)
Took 0.0522 seconds
```

### get namespace:library, ’vhugo’, COLUMNS=>’author’
```CMD
hbase(main):007:0> get 'apignerol:library', 'vhugo', COLUMNS => 'author'

COLUMN                              CELL
 author:firstname                   timestamp=1605609930645, value=Victor Marie
 author:lastname                    timestamp=1605609936241, value=Hugo
1 row(s)
Took 0.0094 seconds
```

### get namespace:library, ’vhugo’, COLUMNS=>’author’, VERSIONS=>10
```CMD
hbase(main):008:0> get 'apignerol:library', 'vhugo', COLUMNS => 'author', VERSIONS => 10

COLUMN                              CELL
 author:firstname                   timestamp=1605609930645, value=Victor Marie
 author:firstname                   timestamp=1605602231066, value=Victor
 author:lastname                    timestamp=1605609936241, value=Hugo
 author:lastname                    timestamp=1605609886419, value=HUGO
1 row(s)
Took 0.0241 seconds
```



## 1.1.9 ) Deleting a value or a column
### The first deleteall deleted the value author:name=HUGO, but not the other (unless you have entered the wrong timestamp)
```CMD
hbase(main):001:0> deleteall 'apignerol:library', 'vhugo', 'author:lastname', 1605609886419
Took 0.3965 seconds
```

### The second deleted then all values for the column firstname
```CMD
hbase(main):002:0> deleteall 'apignerol:library', 'vhugo', 'author:firstname'
Took 0.0091 seconds
```

### The last deleteall deleted the entire tuple
```CMD
hbase(main):003:0> deleteall 'apignerol:library', 'vhugo'
Took 0.0140 seconds
```

### Use scan command to check the version 10
```CMD
hbase(main):004:0> scan 'apignerol:library'

ROW                                 COLUMN+CELL
 jverne                             column=author:firstname, timestamp=1605602410346, value=Verne
 jverne                             column=author:lastname, timestamp=1605602398920, value=Jules
 jverne                             column=book:publisher, timestamp=1605602988724, value=Hetzel
 jverne                             column=book:title, timestamp=1605602439823, value=Face au drapeau
 jverne                             column=book:year, timestamp=1605602464553, value=1896
1 row(s)
Took 0.0385 seconds
```



## 1.1.10 ) Deleting a table
### Disable the table
```CMD
hbase(main):001:0> disable 'apignerol:library'
Took 1.2589 seconds
```

### Drop the table
```CMD
hbase(main):002:0> drop 'apignerol:library'
Took 0.4816 seconds
```

# 1.2 ) Trees
## 1.2.1 ) Data Insertion

### The idea is to write a program that will insert the data. Its principle is to read the file line by line, ignoring the first one. Each line is broken up into words. The words are grouped by families and inserted in the table.

### First of all, we will create the table with its families

```CMD
hbase(main):003:0> create 'apignerol:trees', {NAME => 'gender', VERSIONS => 5}, {NAME => 'information', VERSIONS => 3}, {NAME => 'address', VERSIONS => 4}

Created table apignerol:trees
Took 1.3247 seconds
=> Hbase::Table - apignerol:trees
```

### Then we create a python code which will be used to insert data in our table

```Python
#/!usr/bin/env python

import sys

lines = []
for line in sys.stdin:
    line = line.split(";")
    lines.append(line)


for line in lines:
    if line[11] != "OBJECTID":
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'gender:genre\', \'" + line[2] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'gender:espece\', \'" + line[3] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'gender:famille\', \'" + line[4] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'gender:nom commun\', \'" + line[9] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'gender:variete\', \'" + line[10] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'information:annee plantation\', \'" + line[5] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'information:hauteur\', \'" + line[6] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'information:circonference\', \'" + line[7] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'address:geopoint\', \'" + line[0] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'address:arrondissement\', \'" + line[1] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'address:adresse\', \'" + line[8] + "\'"
        print "put \'apignerol:trees\', \'" + line[11] + "\', \'address:nom ev\', \'" + line[12] + "\'"
```

### We can now insert the data using this command line

```CMD
hdfs dfs -cat /user/apignerol/input-trees/trees.csv | python table.py | hbase shell


SLF4J: Class path contains multiple SLF4J bindings.
SLF4J: Found binding in [jar:file:/usr/hdp/3.1.5.0-152/hadoop/lib/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: Found binding in [jar:file:/usr/hdp/3.1.5.0-152/hbase/lib/client-facing-thirdparty/slf4j-log4j12-1.7.25.jar!/org/slf4j/impl/StaticLoggerBinder.class]
SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
HBase Shell
Use "help" to get list of supported commands.
Use "exit" to quit this interactive shell.
For Reference, please visit: http://hbase.apache.org/2.0/book.html#shell
Version 2.1.6.3.1.5.0-152, rUnknown, Thu Dec 12 20:16:57 UTC 2019
Took 0.0037 seconds


put 'apignerol:trees', '6', 'gender:genre', 'Maclura'
Took 0.5861 seconds
put 'apignerol:trees', '6', 'gender:espece', 'pomifera'
Took 0.0059 seconds
put 'apignerol:trees', '6', 'gender:famille', 'Moraceae'
Took 0.0076 seconds
put 'apignerol:trees', '6', 'gender:nom commun', 'Oranger des Osages'
Took 0.0044 seconds
put 'apignerol:trees', '6', 'gender:variete', ''
Took 0.0040 seconds
put 'apignerol:trees', '6', 'information:annee plantation', '1935'
Took 0.0036 seconds
put 'apignerol:trees', '6', 'information:hauteur', '13.0'
Took 0.0039 seconds
put 'apignerol:trees', '6', 'information:circonference', ''
Took 0.0058 seconds
put 'apignerol:trees', '6', 'address:geopoint', '(48.857140829, 2.29533455314)'
Took 0.0037 seconds
put 'apignerol:trees', '6', 'address:arrondissement', '7'
Took 0.0039 seconds
put 'apignerol:trees', '6', 'address:adresse', 'Quai Branly, avenue de La Motte-Piquet, avenue de la Bourdonnais, avenue de Suffren'
Took 0.0036 seconds
put 'apignerol:trees', '6', 'address:nom ev', 'Parc du Champs de Mars
'

...

put 'apignerol:trees', '91', 'gender:genre', 'Acer'
put 'apignerol:trees', '91', 'gender:espece', 'opalus'
put 'apignerol:trees', '91', 'gender:famille', 'Sapindaceae'
put 'apignerol:trees', '91', 'gender:nom commun', 'Erable d'Italie'
put 'apignerol:trees', '91', 'gender:genre', 'Acer'
put 'apignerol:trees', '91', 'gender:variete', ''
Took 0.0027 seconds
put 'apignerol:trees', '91', 'information:annee plantation', '1870'
Took 0.0028 seconds
put 'apignerol:trees', '91', 'information:hauteur', '15.0'
Took 0.0025 seconds
put 'apignerol:trees', '91', 'information:circonference', '160.0'
Took 0.0026 seconds
put 'apignerol:trees', '91', 'address:geopoint', '(48.8302532096, 2.41400587444)'
Took 0.0028 seconds
put 'apignerol:trees', '91', 'address:arrondissement', '12'
Took 0.0027 seconds
put 'apignerol:trees', '91', 'address:adresse', 'Ile de Bercy'
Took 0.0027 seconds
put 'apignerol:trees', '91', 'address:nom ev', 'Bois de Vincennes (Ile de Bercy)
'
Took 0.0028 seconds
```


### We check the code did its job correctly
```CMD
hbase(main):001:0> scan 'apignerol:trees'

ROW                                 COLUMN+CELL
 11                                 column=address:adresse, timestamp=1605964250576, value=Cours-la-Reine, avenue Franklin-D.-Roosevelt,
                                     avenue Matignon, avenue Gabriel
 11                                 column=address:arrondissement, timestamp=1605964250569, value=8
 11                                 column=address:geopoint, timestamp=1605964250562, value=(48.8685686134, 2.31331809304)
 11                                 column=address:nom ev, timestamp=1605964250583, value=Jardin des Champs Elys\xC3\xA9es\x0A
 11                                 column=gender:espece, timestamp=1605964250514, value=decurrens
 11                                 column=gender:famille, timestamp=1605964250522, value=Cupressaceae
 11                                 column=gender:genre, timestamp=1605964250505, value=Calocedrus
 11                                 column=gender:nom commun, timestamp=1605964250529, value=C\xC3\xA8dre \xC3\xA0 encens
 11                                 column=gender:variete, timestamp=1605964250535, value=
 11                                 column=information:annee plantation, timestamp=1605964250543, value=1854
 11                                 column=information:circonference, timestamp=1605964250556, value=195.0
 11                                 column=information:hauteur, timestamp=1605964250549, value=20.0
 14                                 column=address:arrondissement, timestamp=1605964250639, value=9
 14                                 column=address:geopoint, timestamp=1605964250632, value=(48.8768191638, 2.33210374339)
 14                                 column=gender:espece, timestamp=1605964250594, value=fraxinifolia
 14                                 column=gender:famille, timestamp=1605964250600, value=Juglandaceae
 14                                 column=gender:genre, timestamp=1605964250589, value=Pterocarya
 14                                 column=gender:nom commun, timestamp=1605964250605, value=P\xC3\xA9rocarya du Caucase
 14                                 column=gender:variete, timestamp=1605964250610, value=
 14                                 column=information:annee plantation, timestamp=1605964250616, value=1862
 14                                 column=information:circonference, timestamp=1605964250626, value=330.0
 14                                 column=information:hauteur, timestamp=1605964250622, value=22.0

 ...

 86                                 column=gender:espece, timestamp=1605964252256, value=orientalis
 86                                 column=gender:famille, timestamp=1605964252260, value=Platanaceae
 86                                 column=gender:genre, timestamp=1605964252253, value=Platanus
 91                                 column=address:adresse, timestamp=1605964252296, value=Ile de Bercy
 91                                 column=address:arrondissement, timestamp=1605964252293, value=12
 91                                 column=address:geopoint, timestamp=1605964252289, value=(48.8302532096, 2.41400587444)
 91                                 column=address:nom ev, timestamp=1605964252300, value=Bois de Vincennes (Ile de Bercy)\x0A
 91                                 column=gender:variete, timestamp=1605964252269, value=
 91                                 column=information:annee plantation, timestamp=1605964252272, value=1870
 91                                 column=information:circonference, timestamp=1605964252280, value=160.0
 91                                 column=information:hauteur, timestamp=1605964252276, value=15.0
24 row(s)
Took 0.4596 seconds




hbase(main):002:0> get 'apignerol:trees', '6'

COLUMN                              CELL
 address:adresse                    timestamp=1605964250491, value=Quai Branly, avenue de La Motte-Piquet, avenue de la Bourdonnais, ave
                                    nue de Suffren
 address:arrondissement             timestamp=1605964250484, value=7
 address:geopoint                   timestamp=1605964250477, value=(48.857140829, 2.29533455314)
 address:nom ev                     timestamp=1605964250498, value=Parc du Champs de Mars\x0A
 gender:espece                      timestamp=1605964250418, value=pomifera
 gender:famille                     timestamp=1605964250428, value=Moraceae
 gender:genre                       timestamp=1605964250409, value=Maclura
 gender:nom commun                  timestamp=1605964250438, value=Oranger des Osages
 gender:variete                     timestamp=1605964250445, value=
 information:annee plantation       timestamp=1605964250454, value=1935
 information:circonference          timestamp=1605964250469, value=
 information:hauteur                timestamp=1605964250463, value=13.0
1 row(s)
Took 0.0317 seconds
```