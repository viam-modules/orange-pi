# [`orange-pi` module](https://github.com/viam-modules/orange-pi)

This [orange-pi module](https://app.viam.com/module/viam/orange-pi) implements an [Orange Pi Zero2](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-Zero-2.html), [Orange Pi Zero 2W](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-Zero-2W.html) or [OrangePi 3 LTS](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/orange-pi-3-LTS.html) using the [`rdk:component:board` API](https://docs.viam.com/appendix/apis/components/board/).

## Setup

First, follow the installation guide for your specific Orange Pi board:

- For an Orange Pi Zero2: follow the [Orange Pi Zero2 installation guide](https://docs.viam.com/installation/prepare/orange-pi-zero2/).
- For an Orange Pi 3 LTS, follow the [Orange Pi 3 LTS installation guide](https://docs.viam.com/installation/prepare/orange-pi-3-lts/).

> [!NOTE]
> There is no setup guide available for the Orange Pi Zero 2W. If you have one of these boards, you can image it with [an Ubuntu image](https://drive.google.com/drive/folders/1g806xyPnVFyM8Dz_6wAWeoTzaDg3PH4Z) to prepare it for running `viam-server`.

> [!NOTE]
> Before configuring your board, you must [create a machine](https://docs.viam.com/cloud/machines/#add-a-new-machine).

Navigate to the [**CONFIGURE** tab](https://docs.viam.com/configure/) of your [machine](https://docs.viam.com/fleet/machines/) in the [Viam app](https://app.viam.com/).
[Add board / orange-pi:orangepi to your machine](https://docs.viam.com/configure/#components).

## Configure your orangepi board

### Attributes

The following attributes are available for `viam:orange-pi:orangepi` boards:

| Attribute | Type | Required? | Description |
| --------- | ---- | --------- | ----------  |
| `analogs` | object | Optional | Attributes of any pins that can be used as analog-to-digital converter (ADC) inputs. |
| `digital_interrupts` | object | Optional | Any digital interrupts's pin number and name. |

For instructions on implementing analogs, see [Analogs configuration](#Analogs-configuration). For instructions on implementing digital interrupts, see [Digital interrupt configuration](#Digital-interrupt-configuration).

### Example configuration

### `viam:orange-pi:orangepi`
```json
  {
    "name": "<your-orange-pi-orangepi-board-name>",
    "model": "viam:orange-pi:orangepi",
    "type": "board",
    "namespace": "rdk",
    "attributes": {},
    "depends_on": []
  }
```
### Analogs configuration
An [analog-to-digital converter](https://www.electronics-tutorials.ws/combination/analogue-to-digital-converter.html) (ADC) takes a continuous voltage input (analog signal) and converts it to an discrete integer output (digital signal).

To integrate an ADC into your machine, you must first physically connect the pins on your ADC to your board.

Then, integrate `analogs` into your board by adding the following to your board's `attributes` configuration:

```json {class="line-numbers linkable-line-numbers"}
"analogs": [
  {
    "name": "<your-analog-reader-name>",
    "pin": "<pin-number-on-adc>",
    "spi_bus": "<your-spi-bus-index>",
    "chip_select": "<chip-select-index>",
    "average_over_ms": <int>,
    "samples_per_sec": <int>
  }
]
```

### Attributes

The following attributes are available for `analogs`:

| Name | Type | Required? | Description |
| ---- | ---- | --------- | ----------- |
|`name` | string | **Required** | Your name for the analog reader. |
|`pin`| string | **Required** | The pin number of the ADC's connection pin, wired to the board. This should be labeled as the physical index of the pin on the ADC.
|`chip_select`| string | **Required** | The chip select index of the board's connection pin, wired to the ADC. |
|`spi_bus` | string | **Required** | The index of the SPI bus connecting the ADC and board. |
| `average_over_ms` | int | Optional | Duration in milliseconds over which the rolling average of the analog input should be taken. |
|`samples_per_sec` | int | Optional | Sampling rate of the analog input in samples per second. |

### Example configuration

```json {class="line-numbers linkable-line-numbers"}
{
  "components": [
    {
      "name": "<your-orange-pi-orangepi-board-name>",
      "model": "viam:orange-pi:orangepi",
      "type": "board",
      "namespace": "rdk",
      "attributes": {
        "analogs": [
          {
            "name": "current",
            "pin": "1",
            "spi_bus": "1",
            "chip_select": "0"
          },
          {
            "name": "pressure",
            "pin": "0",
            "spi_bus": "1",
            "chip_select": "0"
          }
        ]
      }
    }
  ]
}
```

### Digital interrupt configuration
[Interrupts](https://en.wikipedia.org/wiki/Interrupt) are a method of signaling precise state changes.
Configuring digital interrupts to monitor GPIO pins on your board is useful when your application needs to know precisely when there is a change in GPIO value between high and low.

- When an interrupt configured on your board processes a change in the state of the GPIO pin it is configured to monitor, it ticks to record the state change.
  You can stream these ticks with the board API's [`StreamTicks()`](https://docs.viam.com/appendix/apis/components/board/#streamticks), or get the current value of the digital interrupt with [`Value()`](https://docs.viam.com/appendix/apis/components/board/#value).
- Calling [`GetGPIO()`](https://docs.viam.com/appendix/apis/components/board/#getgpio) on a GPIO pin, which you can do without configuring interrupts, is useful when you want to know a pin's value at specific points in your program, but is less precise and convenient than using an interrupt.

Integrate `digital_interrupts` into your machine in the `attributes` of your board by adding the following to your board's `attributes` configuration:

```json {class="line-numbers linkable-line-numbers"}
{
  "digital_interrupts": [
    {
      "name": "<your-digital-interrupt-name>",
      "pin": "<your-digital-interrupt-pin-number>"
    }
  ]
}
```

### Attributes

The following attributes are available for `digital_interrupts`:

| Name | Type | Required? | Description |
| ---- | ---- | --------- | ----------- |
|`name` | string | **Required** | Your name for the digital interrupt. |
|`pin`| string | **Required** | The pin number of the board's GPIO pin that you wish to configure the digital interrupt for. |

### Example configuration

```json {class="line-numbers linkable-line-numbers"}
{
  "components": [
    {
      "name": "<your-orange-pi-orangepi-board-name>",
      "model": "viam:orange-pi:orangepi",
      "type": "board",
      "namespace": "rdk",
      "attributes": {
        "digital_interrupts": [
          {
            "name": "your-interrupt-1",
            "pin": "15"
          },
          {
            "name": "your-interrupt-2",
            "pin": "16"
          }
        ]
      }
    }
  ]
}
```


## Next Steps
- To test your board, expand the **TEST** section of its configuration pane or go to the [**CONTROL** tab](https://docs.viam.com/fleet/control/).
- To write code against your board, use one of the [available SDKs](https://docs.viam.com/sdks/).
- To view examples using a board component, explore [these tutorials](https://docs.viam.com/tutorials/).
