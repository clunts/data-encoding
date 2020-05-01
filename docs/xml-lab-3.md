# Lab 3 - XML
XML stands for EXtensible Markup language and is another very common data serialization format created by the W3C to provide an organized web document format.  XML has become a widely-used format for structured data.

XML uses tags and elements to represent data objects and their values.  For example, the following represents a tag named **hostname** and an element named **router1**:

```xml
<hostname>router1</hostname>
```

We will follow similar steps to those in the YAML and JSON sections:

1. Open the file
2. Read the Stream
3. Load data into a Python object
4. Practice accessing data
5. Manipulate data
6. Write back to a new file

## Create New Script File
1. Create a new file in the `scripts` directory named `xml_data.py`
2. Write the *shebang* line at line 1
3. Import the `xmltodict` and `dicttoxml` python modules

```python
import xmltodict
from dicttoxml import dicttoxml
```

## Open, Read and Transform
```python
with open('hosts.xml', 'r') as file:
    stream = file.read()
    data = xmltodict.parse(stream)
```

## Practice Accessing Data
``` python
print(type(data))

print(data.keys())

for key, val in data['devices'].items():
    print(f'DEVICE: {key}')
    for inner_key, inner_val in val.items():
        print(f'    {inner_val}')
```

## Update your Data

1. Create a new device dictionary to add to our dataset
```python
nxos2 = {
    'nxos2':{
        'data': {
            'role': 'distribution',
            'site': 'atc56',
            'type': 'network-device'
        },
        'groups': ['dna_3'],
        'hostname': 'nxos2',
        'platform': 'nxos',
        'username': 'wwt',
        'password': 'WWTwwt1!',
        'port': '22'
    }
}
```

2. Update our dictionary with the new device
```python
data['devices'].update(nxos2)
```

## Write Data to a New File
The last step is to write this XML data back out to a new file.  This involves three steps.

1. Open a file to write our data to
2. Convert our Python object to XML string data
3. Write the XML string data to the file

```python
filename = 'files/new_xml_data.xml'

with open(filename, 'w') as file:
    xml_data = dicttoxml(data)
    file.write(xml_data.decode('utf-8'))
```

## Nav
[Home](../README.md)