# Zebra Browser Print Wrapper

This package is update of [Zebra Browser Print Wrapper](https://github.com/lhilario/zebra-browser-print-wrapper#readme) and allows you to connect [Zebra Browser Print](https://www.zebra.com/la/es/support-downloads/printer-software/by-request-software.html#browser-print) with your (ReactJs, Angular) aplication through https protocol. The only difference with original version is configuration of API_URL in file contstants.ts!

## Install

Install the module in your project via YARN

```bash
yarn add zebra-browser-print-wrapper-https
```

Or NPM

```bash
npm i zebra-browser-print-wrapper-https
```


## Available Methods

##### **getAvailablePrinters()**

Return a list of the current available printers

##### **getDefaultPrinter()**

Gets the current default printer

##### **setPrinter()**

Sets the printer field

##### **getPrinter()**

Returns the printer field

##### **checkPrinterStatus()**

Returns an object indicating if the printer is ready and if not returns the error.

**Returned object:**

```js
{
 isReadyToPrint: boolean;
 errors: string
}
```

**Possible errors:**

- Paper out
- Ribbon Out
- Media Door Open
- Cutter Fault
- Printhead Overheating
- Motor Overheating
- Printhead Fault
- Incorrect Printhead
- Printer Paused
- Unknown Error

##### **print(str)**

Prints a text string.

You can use this method with simple text or add a string using the [ZPL language](https://www.zebra.com/content/dam/zebra/manuals/printers/common/programming/zpl-zbi2-pm-en.pdf "ZPL language")


## Example Angular

```js
// Import the zebra-browser-prit-wrapper package
import ZebraBrowserPrintWrapperHttps from "zebra-browser-print-wrapper-https";

    async printLabel() {
        try {
            // Create a new instance of the object
            const browserPrint = new ZebraBrowserPrintWrapper();
            // Select default printer
            const defaultPrinter = await (browserPrint.getDefaultPrinter());
            // Set the printer
            browserPrint.setPrinter(defaultPrinter);
            // Check if the printer is ready
            const printerStatus = await browserPrint.checkPrinterStatus();

            if (printerStatus.isReadyToPrint) {
                    // ZPL script to print a qr code
                let zpl = `^XA
                        ^CFD,30
                        ^FO380,100^FD Date:${some date}^FS
                        ^CFD,50
                        ^FO370,200^FD ${some-data}^FS
                        ^FO50,50^BQN,2,7^FDQA,${qrCodeString}^FS
                        ^XZ`;
                browserPrint.print(zpl);
                    console.log("Print succes");
            } else {
                    console.log("Error/s", printerStatus.errors);
            }

        } catch (error) {
            throw new Error(error);
        }
    }
