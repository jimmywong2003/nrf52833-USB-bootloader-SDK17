# nrf52833-usb-bootloader

Example project of the nRF52833 USB Bootloader

This is the example USB bootloader on the nRF52833 which runs with SDK 17.0.  

### Generate the private key
`` 
nrfutil keys generate demo_private.key 
``

### Generate the public key
`` 
nrfutil keys display --key pk --format code demo_private.key --out_file demo_public_key.c 
``

### update the public key inside the dfu_public_key.c

```

#ifdef NRF_DFU_DEBUG_VERSION 

/** @brief Public key used to verify DFU images */
__ALIGN(4) const uint8_t pk[64] =
{
    0xe1, 0xb6, 0x73, 0xfb, 0x24, 0x1c, 0x26, 0xa2, 0x16, 0xe9, 0x22, 0xdf, 0xd5, 0x1d, 0x31, 0xe5, 0x0c, 0x98, 0xb8, 0x75, 0x07, 0x04, 0xc1, 0x01, 0xd5, 0x1c, 0xc5, 0x80, 0x44, 0x90, 0xa4, 0x87,
    0x10, 0x43, 0x47, 0xc4, 0xfd, 0x7f, 0x7f, 0xc3, 0x60, 0x26, 0x69, 0xf6, 0xea, 0xe2, 0xbc, 0x15, 0x11, 0x60, 0xb7, 0x06, 0xbf, 0x36, 0x29, 0xbc, 0xa9, 0x44, 0x54, 0x05, 0x96, 0x14, 0x22, 0x2f
};
#endif
```

### Generate the app_uart package

```

nrfutil pkg generate --hw-version 52 --application-version 0 --application ..\ble_app_uart\pca10100\s140\ses\output\debug\exe\ble_app_uart_pca10100_s140.hex --sd-req 0xCA --key-file demo_private.key app_uart_fw1_pca10100.zip

```

### Command line by using the nRFutil DFU

```
nrfutil dfu usb-serial --package app_uart_fw1_pca10100.zip -p com10
```

## Requirement

* nRF52833 DK / nRF52840 DK 
* Nordic nRF5 SDK 17.0 / S140v7.0.1
* Segger Embedde Studio
