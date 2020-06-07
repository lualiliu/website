在这里使用荔枝派nano做示例，而因为f1c100s只有两组spi，一组给了tf卡，另一组给了屏幕，就不能给spi flash了，所以，我们需要把原来spiflash（spi0）的接线引到spi屏幕上。

### 接线表

|  F1C100S   | SCREEN  |
|  ----  | ----  |
| PC3_SPI0_MOSI_LCD_RS  | SDA |
| PC0_SPI0_SCK_LCD_CS  | SCL |
| PC1_SPI0_CS | CS |
| PE11_RES | RESET |
| PE10_CHRG | WR/A0/DC |

背光直接接VCC

### 设备树

先删除原来spiflash的设备树内容

```
		spi0: spi@1c05000 {
			compatible = "allwinner,suniv-spi",
				     "allwinner,sun8i-h3-spi";
			reg = <0x01c05000 0x1000>;
			interrupts = <10>;
			clocks = <&ccu CLK_BUS_SPI0>, <&ccu CLK_BUS_SPI0>;
			clock-names = "ahb", "mod";
			resets = <&ccu RST_BUS_SPI0>;
			#address-cells = <1>;
			#size-cells = <0>;
			pinctrl-names = "default";
			pinctrl-0 = <&spi0_pins_a>;
			status = "okay";
		       	ili9341@0 {
				compatible = "ilitek,ili9341";
				reg = <0>;
				spi-max-frequency = <50000000>; 
				bgr;
				rotate = <180>;
				fps = <30>;
				buswidth = <8>;
				reset-gpios = <&pio 4 11 GPIO_ACTIVE_LOW>;
				dc-gpios = <&pio 4 10 GPIO_ACTIVE_LOW>;
				debug = <0>;
		       	};
		};
```

### 选用内核

kernel 4.14

```
$ cd
$ wget https://github.com/steward-fu/miyoo/releases/download/v1.2/kernel.7z
$ 7za x kernel.7z
$ cd kernel
$ make ARCH=arm CROSS_COMPILE=arm-linux- miyoo_defconfig menuconfig
> Device Drivers > SPI support 
   <*>   Allwinner A31 SPI controller  
> Device Drivers > Staging drivers > Support for small TFT LCD display modules
   <*>   FB driver for the ILI9341 LCD Controller
$ make ARCH=arm CROSS_COMPILE=arm-linux- clean
$ make ARCH=arm CROSS_COMPILE=arm-linux- -j8
```

将编译后的内核替换即可

[RETURN](https://lualiliu.github.io/website/)
