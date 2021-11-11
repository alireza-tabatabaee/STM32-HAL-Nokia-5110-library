# STM32 HAL Nokia 5110 library (SPI)

This is a port of [Tilen Majerle's Nokia 3310/5110 LCD library](https://stm32f4-discovery.net/pcd8544-nokia-33105110-lcd-stm32f429-discovery-library/), which can be used with the STM32 HAL functions, and STM32CubeMX's auto-generated code. 

The biggest advantage of this SPI-based library compared to Adafruit's more common GPIO-based library, is the noticeably higher speed.

## How to use

Settings in STM32CubeMX: Activate an SPI, set the mode to "Transmit only Master" (since we don't need to recieve anything from the LCD) and set the data size to "8 bits". Then activate three GPIO_Output pins for the reset, DC and CE pins of the LCD.

After setting up SPI and three GPIO_Output pins, go to "stm32f4_pcd8544.h" and edit these lines on top of the file with the handler of your SPI, and the three GPIO pins that you set up for the reset, DC and CE pins of the LCD. In the original files I have set up hspi3, and D7, D5 and D3 pins as GPIO.

```c
//SPI used
extern SPI_HandleTypeDef hspi3;
#define PCD8544_SPI				hspi3

//Default pins used
//Default RST pin
#define PCD8544_RST_PORT		GPIOD
#define PCD8544_RST_PIN			GPIO_PIN_7
//Default CE pin
#define PCD8544_CE_PORT			GPIOD
#define PCD8544_CE_PIN			GPIO_PIN_5
//Default DC pin
#define PCD8544_DC_PORT			GPIOD
#define PCD8544_DC_PIN			GPIO_PIN_3
```

## Usage Example

```c
#include "stm32f4_pcd8544.h"
...
int main()
{
...

    PCD8544_Init(0x38); //LCD initializing function, the input is the contrast value
    PCD8544_Puts("Hello World!", PCD8544_Pixel_Set, PCD8544_FontSize_5x7);
    PCD8544_Refresh(); //Don't forget to refresh to see your text on screen

    while(1)
    { ... }
}
```

## License
[GPL3](https://choosealicense.com/licenses/gpl-3.0/)