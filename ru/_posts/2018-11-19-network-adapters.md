---
layout: post
lang: ru
title: Network adapters
tags: [dev]
---

## Сетевые адаптеры

<!-- more -->

### Как узнать информацию о потерях
Ключевые слова: missed, dropped, fifo, error, rx.  
`ip -s -s link show eth1`  

Нужно проверить ошибки RX. Некоторые сетевые карты предоставляют более подробную информацию о потерях:  
`ethtool -S eth1`

Потери могут быть не только на сетевых картах вашего сервера. Они также могут находиться на порту сетевого оборудования. Вы можете узнать, как это увидеть из документации производителя сетевого оборудования.  

### Размер буфера сетевой карты  
```
[root@astra ~]# ethtool -g eth1
Ring parameters for eth1:
Pre-set maximums:
RX:		4096
RX Mini:	0
RX Jumbo:	0
TX:		4096
Current hardware settings:
RX:		4096
RX Mini:	0
RX Jumbo:	0
TX:		256
```
Здесь мы видим, что rx-буфер увеличен до максимума. Обычно найти верное значение довольно сложно. Наиболее оптимальным является некоторое «среднее» значение. С высокочастотным и многоядерным процессором (> 3GHz) вы можете получить хорошие результаты с максимальным размером буфера. 
Пример команды для увеличения буфера:   
`ethtool -G eth1 rx 2048`   