# [`orange-i` module](https://github.com/viam-modules/orange-i)

This [orange-i module](https://app.viam.com/module/viam/orange-i) implements an [Orange Pi Zero2](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-Zero-2.html), [Orange Pi Zero 2W](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/Orange-Pi-Zero-2W.html) or [OrangePi 3 LTS](http://www.orangepi.org/html/hardWare/computerAndMicrocontrollers/details/orange-pi-3-LTS.html) using the [`rdk:component:board` API](https://docs.viam.com/appendix/apis/components/board/).

## Setup

First, follow the installation guide for your specific Orange Pi board:

- For an Orange Pi Zero2: follow the [Orange Pi Zero2 installation guide](https://docs.viam.com/installation/prepare/orange-pi-zero2/).
- For an Orange Pi 3 LTS, follow the [Orange Pi 3 LTS installation guide](https://docs.viam.com/installation/prepare/orange-pi-3-lts/).

> [!NOTE]
> There is no setup guide available for the Orange Pi Zero 2W. If you have one of these boards, you can image it with [an Ubuntu image](https://drive.google.com/drive/folders/1g806xyPnVFyM8Dz_6wAWeoTzaDg3PH4Z) to prepare it for running `viam-server`.

> [!NOTE]
> Before configuring your board, you must [create a machine](https://docs.viam.com/cloud/machines/#add-a-new-machine).

## Configure your orangepi board

Navigate to the [**CONFIGURE** tab](https://docs.viam.com/configure/) of your [machine](https://docs.viam.com/fleet/machines/) in the [Viam app](https://app.viam.com/).
[Add board / orange-i:orangepi to your machine](https://docs.viam.com/configure/#components).

> [!NOTE]
> For more information, see [Configure a Machine](https://docs.viam.com/configure/).

### Attributes

The following attributes are available for `viam:orange-i:orangepi` boards:

| Attribute | Type | Required? | Description |
| --------- | ---- | --------- | ----------  |
| `analogs` | object | Optional | Attributes of any pins that can be used as analog-to-digital converter (ADC) inputs. |
| `digital_interrupts` | object | Optional | Any digital interrupts's pin number and name. |

For instructions on implementing analogs, see [Analogs configuration](#Analogs-configuration). For instructions on implementing digital interrupts, see [Digital interrupt configuration](#Digital-interrupt-configuration).

## Example configuration

### `viam:orange-i:orangepi`
```json
  {
     "name": "<your-orange-i-orangepi-board-name>",
      "model": "viam:orange-i:orangepi",
      "type": "board",
      "namespace": "rdk",
      "attributes": {},
      "depends_on": []
  }
```

### Next Steps
- To test your board, expand the **TEST** section of its configuration pane or go to the [**CONTROL** tab](https://docs.viam.com/fleet/control/).
- To write code against your board, use one of the [available SDKs](https://docs.viam.com/sdks/).
- To view examples using a board component, explore [these tutorials](https://docs.viam.com/tutorials/).
