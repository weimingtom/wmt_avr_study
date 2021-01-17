# wmt_avr_study
My AVR Study

## ATMEGA328P DIP version min system burning, by my weibo    
搭建atmega328p最小系统（用Arduino串口烧录）所需要的材料（AVR有很多，  
但如果想上Arduino的话建议用atmega328p和16MHz晶振）：    
(1) 面包板+若干公对公杜邦线。也可以准备一些公对母的杜邦线，  
USB-TTL转换器可能会用到公对母杜邦线  
(2) atmega328p的DIP版28脚，最好有对应的针脚图，不需要插座，  
因为插座不容易插在面包板上。  
如果(3)买了山寨uno，可以不用再买这个DIP芯片了  
(3) atmega8开发板和usbasp。可以用DIP芯片的山寨uno板+usbasp代替，  
但需要另外购买一个10针转6针ICSP的转接板。用于烧录bootloader和熔丝位。  
建议买山寨uno板，可以省略不用烧录bootloader，而且可以得到熔丝位4个字节  
的设置方法（也可以网上找到）  
(4) USB-TTL串口转换器+104陶瓷电容。可以用拔掉DIP芯片的山寨uno板代替，  
这样就不需要reset脚串联104电容了。这个同时也作为最小系统的5V电源  
(5) 16MHz晶振和22陶瓷电容2个，注意晶振用8MHz或者其他值可能会无法串口烧录，  
但可以用usbasp烧录，所以建议用16M晶振  
(6) 1个200R电阻和1个LED。做D13脚的测试灯，电阻可以买一包黄色版的碳膜电阻，  
比蓝色电阻要便宜一些，LED随便都没问题，其实150欧-200欧配合不同颜色和  
压降LED的效果差别并不大，不需要担心烧掉。200欧的阻值是可以算出的，  
以前已经讨论过这个问题了  

## progisp.exe, ICSP烧录，使用usbasp烧录器烧录bootloader固件时设置熔丝位方法：  
* 低位值：FF、高位值：DE、扩展位值：FD、加密值FF  
* 时钟校正：8.0MHz：85  
* 固件文件, optiboot_atmega328.hex, see below  
* 注意事项, 小心变砖：  
* 因为禁用ISP把一块芯片变砖了（相当于永远地锁死芯片不能读写）  
* progisp+USBASP烧录完bootloader，就可以用USB转串口来烧录了（Arduino IDE），方法是：  
* 用它(atmega328p)的Reset、RX、TX这三个脚做串口烧录  

## atmega328p bootloader firmware file, see Arduino IDE board.txt    
uno.bootloader.tool=avrdude  
uno.bootloader.low_fuses=0xFF  
uno.bootloader.high_fuses=0xDE  
uno.bootloader.extended_fuses=0xFD  
uno.bootloader.unlock_bits=0x3F  
uno.bootloader.lock_bits=0x0F  
uno.bootloader.file=optiboot/optiboot_atmega328.hex  
