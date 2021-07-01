## Introduction

There is a total of 11 I/O controllers on MT3620 (2 PWM, 4 ISUs, 1 ADC and 2 I2S). These are multiplexed to 2 or more peripherals, and only one peripheral per I/O controller sould be allowed. 

`mt3620_controller.json` contains the list of all I/O controllers supported by Azure Sphere SDK applibs and the allowed peripherals. The structure of the file is as follows:

    {
        "I/O Controller name": {
            "Peripheral name": [
                /* 
                   List of pins within the peripheral that can 
                   be addressed from the Azure Sphere SDK applibs
                */
                [
                    /* 
                       List of existing mappings to this pin 
                    */
                ],
                ...
            ],
            ...
        },
        ...
    }

## Example

Following is a list of 2 peripherals multiplexed to the "PWM-CONTROLLER-0".

    {
        "PWM-CONTROLLER-0": {
            "gpio": [
                [0, "MT3620_GPIO0"],
                [1, "MT3620_GPIO1"],
                [2, "MT3620_GPIO2"],
                [3, "MT3620_GPIO3"]
            ],
            "pwm": [
                [0, "MT3620_PWM_CONTROLLER0"]
            ]
        }
    }

Given the following application level hardware definition, we can infer that **MT3620_GPIO3** pin has been mapped to **TEMPLATE_LED** and is assumed to be used as a **Gpio**. Therefore **PWM-CONTROLLER-0** has been mapped to the **Gpio** peripheral. 

    {
        "Imports" : [ {"Path": "mt3620.json"} ],
        "Peripherals": [
            {"Name": "TEMPLATE_LED", "Type": "Gpio", "Mapping": "MT3620_GPIO3", "Comment": "Dummy LED"}
        ]
    }

Mapping another peripheral to **PWM-CONTROLLER-0** is not allowed. For example, the following is invalid.

    {
        "Imports" : [ {"Path": "mt3620.json"} ],
        "Peripherals": [
            {"Name": "TEMPLATE_LED", "Type": "Gpio", "Mapping": "MT3620_GPIO3", "Comment": "Dummy LED"},
            {"Name": "TEMPLATE_PWM", "Type": "Pwm", "Mapping": "MT3620_PWM_CONTROLLER0", "Comment": "PWM LED"}
        ]
    }

Mappig new pins of the same peripheral to **PWM-CONTROLLER-0** is allowed. For example, the following is valid.

    {
        "Imports" : [ {"Path": "mt3620.json"} ],
        "Peripherals": [
            {"Name": "TEMPLATE_LED_1", "Type": "Gpio", "Mapping": "MT3620_GPIO2", "Comment": "Dummy LED 1"},
            {"Name": "TEMPLATE_LED_2", "Type": "Gpio", "Mapping": "MT3620_GPIO3", "Comment": "Dummy LED 2"},
        ]
    }
