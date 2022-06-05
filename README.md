# kivy_translate
Easy translations for kivy (not only) projects

Simple tool to make python applications available in multiple languages.  
Simple collecting translable messages to PO files  
Simple compiling PO files to MO files

Kivy translate have 2 parts:  
- kivy_translate tool - collect and compile messages
- Handler in runtime to translate messages on fly

---
**NOTE**  
Code is not well tested, some bugs may exist, message me if something is not ok.
---

## Installation

Just: `pip install kivy_translate`


## Usage kivy_translate tool

Invoke it from main folder of project (same place where `main.py` is)

```
Usage:
    kivy_translate [target] [--verbose]

Available modes:
    get        Collect all translable strings across project - string should be in `_('string')`
    make       Compile all PO files for all supported languages
```

### .kivy_translate.cfg

You can use custom translation setting file.  
It should be in same folder where `main.py` is.

Example `.kivy_translate.cfg` file

```
[settings]
supported_languages = ["en", "pl"]

; not implemented yet
translator_name = Firstname Lastname
translator_email = translator@example.com

[files]
main_file = main.py
excluded_folders = [".git", ".buildozer", "__pycache__", "bin"]

; 'kv' not implemented yet
filter_extensions = [".py", ".kv"]
```


## Usage translations inside app
---
**NOTE**  
After version 0.2.0 `Trans` class have to be imported from package.  
---

You need to make a file with translations  
e.g.:  
`./translations/translations.py`

```
from kivy_translate.kvtranslate import Trans

py_translations = {
    "English": Trans._("English"),
    "Polish": Trans._("Polish"),
    "German": Trans._("German"),
    "Spanish": Trans._("Spanish"),
}

kv_translations = {
    "Close": Trans._("Close"),
    "Save": Trans._("Save"),
}
```

`key` is string_identifier  
`value` is translable string

`py_translations` are translations used by pure python  
e.g.:  
```
label = Label(text=Trans._("English"))
```


`kv_translations` are used in .kv  
e.g.:  
```
Label:
    text: app.trans['Save']
```
They will change when `Trans.refresh_translations()` is called

---
**NOTE**  
All missing strings (not included in `kv_translations`)
will be replaced with "???????"
---

### `Trans` methods

- `get_current_language()` - returns current language for app
- `switch_language(<lang>)` - change language for gettext and bind it to app. accepts 2 letter code (en, pl, es, de etc...)
- `refresh_translations()` - reloads all .kv translations on fly (kivy only)

### Minimum setup
```
#!/usr/bin/env python

from kivy.app import App
from kivy.properties import ObjectProperty

from kivy_translate.kvtranslate import Trans

class MainApp(App):
    trans = ObjectProperty()
    settings = ObjectProperty()

    def _init_app(self):
        self.settings = {"lang": "en"}
        lang = self.settings.get("lang", "en")
        self._init_translations(lang)

    def _init_translations(self, lang):
        Trans.switch_language(lang.lower())
        from translations.translations import kv_translations

        Trans.kv_translations = kv_translations
        Trans.refresh_translations()

    def build(self):
        self._init_app()
        your code...
        your code...
        your code...


if __name__ == "__main__":
    MainApp().run()

```
All should work now

---
**NOTE**  
To change language on fly  
```
Trans.switch_language(new_lang)
Trans.refresh_translations()
```
---

---
**NOTE**  
Remember to collect and compile messages !
---

---
**NOTE**  
To use kivy_translate with kivy import  
`from kivy_translate.kvtranslate import Trans`  
To use kivy_translate with generic python import  
`from kivy_translate.translate import Trans`
---

## Support

If you need assistance, you can ask for help by email:

* Email: dev@stacja1.pl


## License

kivy_translate is released under the terms of the MIT License.  
Please refer to the LICENSE file.

