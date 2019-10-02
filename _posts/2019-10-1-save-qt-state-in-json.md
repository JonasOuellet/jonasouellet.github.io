How to convert a *QByteArray* to unicode to be able to save it in a text file using **Python**.

When working with **Qt** it is possible to save and restore some widget state using `saveState` and `restoreState` method.  Calling  `saveState` return a [QByteArray](https://doc.qt.io/qt-5/qbytearray.html).  How can I write this *QByteArray* to a text file and reload it after ?

#### Convert QByteArray to Unicode

```python

import json

from PyQt5 import QtWidgets, QtCore

# create a widget to save the state
my_window = QtWidgets.QMainWindow()

qt_byte = my_window.saveState()

codec = QtCore.QTextCodec.codecForName("UTF-16")
unicode_byte = codec.toUnicode(qt_byte)

to_write = json.dumps({'widget_sate': unicode_byte}, indent=4)

```

#### Convert Unicode to QByteArray

```python

text_encoder = codec.makeEncoder(QtCore.QTextCodec.IgnoreHeader)
qt_byte = text_encoder.fromUnicode(unicode_byte)
my_window.restoreState(qt_byte)

```
