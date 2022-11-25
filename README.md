# esc_pos_printer_v2

[![Pub Version](https://img.shields.io/pub/v/esc_pos_printer_v2)](https://pub.dev/packages/esc_pos_printer_v2)

[//]: # (The library allows to print receipts using an ESC/POS thermal WiFi/Ethernet printer. For Bluetooth printers, use [esc_pos_bluetooth]&#40;https://github.com/andrey-ushakov/esc_pos_bluetooth&#41; library.)

It can be used in [Flutter](https://flutter.dev/) or pure [Dart](https://dart.dev/) projects. For Flutter projects, both Android and iOS are supported.

[//]: # (To scan for printers in your network, consider using [ping_discover_network]&#40;https://pub.dev/packages/ping_discover_network&#41; package. Note that most of the ESC/POS printers by default listen on port 9100.)

## TODO (PRs are welcomed!)

- Print QR Codes using the `GS ( k` command (printing QR code from an image already supported)
- PDF-417 Barcodes using the `GS ( k` command
- Line spacing using the `ESC 3 <n>` command

## How to Help

[//]: # (- Test your printer and add it in the table: [Wifi/Network printer]&#40;https://github.com/andrey-ushakov/esc_pos_printer_v2/blob/master/printers.md&#41; or [Bluetooth printer]&#40;https://github.com/andrey-ushakov/esc_pos_bluetooth/blob/master/printers.md&#41;)
- Test and report bugs
- Share your ideas about what could be improved (code optimization, new features...)
- PRs are welcomed!

## Tested Printers

Here are some [printers tested with this library](printers.md). Please add the models you have tested to maintain and improve this library and help others to choose the right printer.

## Generate a Receipt

### Simple Receipt with Styles:

```dart
void testReceipt(NetworkPrinter printer) {
  printer.text(
        'Regular: aA bB cC dD eE fF gG hH iI jJ kK lL mM nN oO pP qQ rR sS tT uU vV wW xX yY zZ');
  printer.text('Special 1: àÀ èÈ éÉ ûÛ üÜ çÇ ôÔ',
      styles: PosStyles(codeTable: 'CP1252'));
  printer.text('Special 2: blåbærgrød',
      styles: PosStyles(codeTable: 'CP1252'));

  printer.text('Bold text', styles: PosStyles(bold: true));
  printer.text('Reverse text', styles: PosStyles(reverse: true));
  printer.text('Underlined text',
      styles: PosStyles(underline: true), linesAfter: 1);
  printer.text('Align left', styles: PosStyles(align: PosAlign.left));
  printer.text('Align center', styles: PosStyles(align: PosAlign.center));
  printer.text('Align right',
      styles: PosStyles(align: PosAlign.right), linesAfter: 1);

  printer.text('Text size 200%',
      styles: PosStyles(
        height: PosTextSize.size2,
        width: PosTextSize.size2,
      ));

  printer.feed(2);
  printer.cut();
}
```

[//]: # (You can find more examples here: [esc_pos_utils]&#40;https://github.com/andrey-ushakov/esc_pos_utils&#41;.)

## Print a Receipt

```dart
import 'package:esc_pos_printer_v2/esc_pos_printer_v2.dart';

const PaperSize paper = PaperSize.mm80;
final profile = await CapabilityProfile.load();
final printer = NetworkPrinter(paper, profile);

final PosPrintResult res = await printer.connect('192.168.0.123', port: 9100);

if (res == PosPrintResult.success) {
  testReceipt(printer);
  printer.disconnect();
}

print('Print result: ${res.msg}');
```

For a complete example, check `example/example.dart` and `example/discover_printers`.

## Test Print

[//]: # (<img src="https://github.com/andrey-ushakov/esc_pos_printer_v2/blob/master/example/receipt2.jpg?raw=true" alt="test receipt" height="800"/>)
[//]: # ()
[//]: # (<img src="https://github.com/andrey-ushakov/esc_pos_printer_v2/blob/master/example/receipt.jpg?raw=true" alt="test receipt" height="500"/>)
