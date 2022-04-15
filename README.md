# kivy_translate
Easy translations for kivy projects

Simple tool to make python applications available in multiple languages.  
Simple collecting translable messages to PO files  
Simple compiling PO files to MO files


## Installation

Just: `pip install kivy_translate`


## Usage

Invoke it from main folder of project (same place where `main.py` is)

```
Usage:
    kivy_translate [target] [--verbose]

Available modes:
    get        Collect all translable strings across project - string should be in `_('string')`
    make       Compile all PO files for all supported languages
```

## .kivy_translate.cfg

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

## Support

If you need assistance, you can ask for help by email:

* Email: dev@stacja1.pl


## License

kivy_translate is released under the terms of the MIT License.  
Please refer to the LICENSE file.

