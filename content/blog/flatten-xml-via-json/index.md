---
title: Flatten XML via JSON
date: "2021-01-02"
description: "Flatten an XML tree by turning it into JSON first (Python)"
---

## Going Directly from XML to JSON Using xmltodict:

It's pretty straightforward to turn an XML file into a Python dictionary (which is essentially the same as a JSON file's content).

The code snippet below can be copied and pasted into your terminal, and it will prompt you to select a file.

If you run into `ModuleNotFoundError`s, simply type the following into your Python terminal:

`pip` or `pip3 import [NAME OF MISSING MODULE HERE]`

```python
import xmltodict, json
xml_filepath = input('Drag and drop an XML file here:').strip()
with open(xml_filepath) as xml_file_input:
    xml_data_stream = xml_file_input.read()
    data_dict = xmltodict.parse(xml_data_stream)
```

## Flatten JSON recursively with Python

I came across a nice, succinct, and effective piece of code to accomplish what I needed. Thanks [Amir Ziai](https://towardsdatascience.com/flattening-json-objects-in-python-f5343c794b10)!

```python
import json

def flatten_json(y): # or use `pip install flatten_json`
    out = {}

    def flatten(x, name=''):
        if type(x) is dict:
            for a in x:
                flatten(x[a], name + a + '_')
        elif type(x) is list:
            i = 0
            for a in x:
                flatten(a, name + str(i) + '_')
                i += 1
        else:
            out[name[:-1]] = x

    flatten(y)
    return out

json_filepath = input('Drag and drop a JSON file here:').strip()
with open(json_filepath) as json_file_input:
    string_data_stream = json_file_input.read()
    json_data_stream = json.loads(string_data_stream)
    flat_json = flatten_json(json_data_stream)

```

In case you run into an error about an extra character being at the end of your file, you can just leave off the final one (or two, etc.) characters by adding an index to the end of your `string_data_stream`, for example:

```python
json.loads(string_data_stream[:-1])
```

# Don't Forget to Save Your Flattened JSON Data

```python
with open('flattened_xml_via_json.json', 'a') as save_file:
    save_file.write(json.dumps(data_dict, indent=4, sort_keys=True))
```
