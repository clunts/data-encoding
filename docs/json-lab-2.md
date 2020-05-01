# Lab 2 - JSON
JSON stands for JavaScript Object Notation and is another very common data serialization format.  This format originated in javascript but many different languages have included code to utilize JSON.

JSON is an open standard data format that uses key/value pairs and array data.

## Create New Script File
1. Create a new file in the `scripts` directory named `json_data.py`
2. Write the *shebang* line at line 1
3. Import the `json` Python module

## Open, Read and Transform
```python
with open('hosts.json', 'r') as f:
    stream = f.read()
    data = json.loads(stream)
```

## Practice Accessing Data
``` python
print(type(data))

print(data.keys())

for k, v in data.items():
    print(f'DEVICE: {k}')
    for key, value in v.items():
        print(f'    {value}')
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
data.update(nxos2)
```
3. Add `devices` key to wrap our data
```python
data = { 'devices': data }
```

## Write Data to a New File
The last step is to write this JSON data back out to a new file.  This involves three steps.

1. Open a file to write our data to
2. Convert our Python object to JSON string data
3. Write the JSON string data to the file

```python
filename = 'files/new_json_data.json'

with open(filename, 'w') as file:
    json_data = json.dumps(data)
    file.write(json_data)
```

4. Exit the Python interactive shell and inspect your newly created file

```shell
cat files/new_jaml_data.json
```



## Nav

[Lab 3](./xml-lab-3.md)

[Home](../README.md)