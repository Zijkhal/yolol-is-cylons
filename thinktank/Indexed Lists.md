# Indexed Lists
*By [Zijkhal] (https://github.com/Zijkhal)*

------------
Access variables by an address, instead of a name!

##### Table of Contents
 - [27 Values per Chip](#27-Values-per-Chip)
 

### 27 Values per Chip
*By Zijkhal*

Stores 27 arbitrary values per chip, has support for combining multiple chips into a single multichip array

 - **:Addr** : Global address
 - **:CW** : Chipwait, set it to the same name within a multichip array
 - **:WR** : Write flag, set to 1 to write, set to 0 to read. Setting it to other values will result in incorrect behaviour
 - **a** : internal address calculated from global address
 - **f** : select flag for the first variable on the line, 0 means active
 - **s** : select flag for the second variable on the line, 0 means active
 - **:A** : this is where reads go to
 - **:B** : put the value to write in this
 - **:Cid[two digit number]** : Unique identifier of the chip in the multichip array, start from 0, raise by 1 for each consecutive chip. If desired, it can be replaced with hardcoded value to be set during installation.
 - **a0, a1, etc etc** : storage variables

#### Continuos Addressing

First chip stores first 27 variables, second chip stores second 27, etc etc

```vbnet
a=:Addr%27 f=a%3 s=f-1 goto :W+((:Addr-a)/27==:Cid00)*(a-f+3)/3*2
```
*In case of a miss, it will repeat this line instead. All missed chips in the array will then be paused by the chip with the hit.*

#### Modulo Based Addressing

Each chip stores every Nth variable, where N is the number of chips in the multichip array, offset by the unique id of each chip

```vbnet
a=:Addr-:Cid00 j=a/:NrC f=j%3 s=f-1 goto :WR+(a%:NrC==0)*(j-f+3)/3*2
```
*In case of a miss, it will repeat this line instead. All missed chips in the array will then be paused by the chip with the hit.*

#### Storage section
Core of the chip, put one of the two addressing methods on the first line. 
```vbnet
//put addressing here
if f then if s then :A=a3 else :A=a2 else :A=a1 end end goto --:CW
if f then if s then a3=:B else a2=:B else a1=:B end end goto --:CW
if f then if s then :A=a6 else :A=a5 else :A=a4 end end goto --:CW
if f then if s then a6=:B else a5=:B else a4=:B end end goto --:CW
if f then if s then :A=a9 else :A=a8 else :A=a7 end end goto --:CW
if f then if s then a9=:B else a8=:B else a7=:B end end goto --:CW
if f then if s then :A=ab else :A=aa else :A=a0 end end goto --:CW
if f then if s then ab=:B else aa=:B else a0=:B end end goto --:CW
if f then if s then :A=ae else :A=ad else :A=ac end end goto --:CW
if f then if s then ae=:B else ad=:B else ac=:B end end goto --:CW
if f then if s then :A=ah else :A=ag else :A=af end end goto --:CW
if f then if s then ah=:B else ag=:B else af=:B end end goto --:CW
if f then if s then :A=ak else :A=aj else :A=ai end end goto --:CW
if f then if s then ak=:B else aj=:B else ai=:B end end goto --:CW
if f then if s then :A=ab else :A=am else :A=al end end goto --:CW
if f then if s then an=:B else am=:B else al=:B end end goto --:CW
if f then if s then :A=aq else :A=ap else :A=ao end end goto --:CW
if f then if s then aq=:B else ap=:B else ao=:B end end goto --:CW
```
