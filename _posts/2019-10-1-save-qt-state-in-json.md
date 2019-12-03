How to convert a *QByteArray* to unicode to be able to save it in a text file using **Python**.

When working with **Qt** it is possible to save and restore some widget state using `saveState` and `restoreState` method.  Calling  `saveState` return a [QByteArray](https://doc.qt.io/qt-5/qbytearray.html).  How can I write this *QByteArray* to a text file and reload it after ?

#### Convert QByteArray to Unicode

```python

import json

from PyQt5 import QtWidgets, QtCore

# create a widget to save the state
my_window = QtWidgets.QMainWindow()

qbyte_array = my_window.saveState()

codec = QtCore.QTextCodec.codecForName("UTF-16")
unicode_str = codec.toUnicode(qbyte_array)

to_write = json.dumps({'widget_sate': unicode_str}, indent=4)

```

#### Convert Unicode to QByteArray

```python

text_encoder = codec.makeEncoder(QtCore.QTextCodec.IgnoreHeader)
qbyte_array = text_encoder.fromUnicode(unicode_str)
my_window.restoreState(qbyte_array)

```
