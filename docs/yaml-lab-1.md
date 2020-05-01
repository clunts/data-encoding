
# Lab 1 - YAML
This lab will focus on learning some basic operations when working with YAML encoded files.  The scenario will use a simple inventory file of network devices.

## What is YAML
![YAML](../images/yaml.png) + ![Python](../images/python.png)

YAML is a very common data serialization syntax in use today.  It's popularity is mostly do to its simplicity and ease of readability.

YAML stands for YAML ain't markup language.  What that means is in contrast to other serialization syntaxes like XML, YAML doesn't use any sort of tagging to define the data structure.  YAML uses whitespace to do this.

Python is the most popular programming language in many areas including network automation.

This lab will demonstrate the basics of how to use python libraries to work with YAML structured data

## Create a new Python file
The first step is to create a new Python file that we will use to build our exploration of YAML data.

1. Create a file named **yaml_data.py** under the `scripts` folder
2. Add a *shebang* line to your python file
```
#!/usr/bin/env python
```
3. Save your file

## Import PyYAML
We will use the `pyyaml` python library in this lab to convert YAML-formatted data to Python objects and vice versa.  `pyyaml` uses two main operations:
- **load()** - The load() operation 'loads' YAML data into a Python object.
- **dump()** - The dump() operation 'dumps' YAML data from a python object to a YAML format.

The PyYAML Python library was installed in the [Getting Setup](./getting-setup.md) section.  In order to utilize a Python library in your script, it must be imported.

``` import yaml ```

## Open, Read and Transform
A file can be opened using the built-in `open` function in Python.  Using this built-in function requires you to manually open and close the file handle.  Another way to perform this action would be to use a context manager.  A context manager will take care of the closing of the file automatically.

The Python code below performs three steps:
1. Open the file
2. Read the file contents
3. Transform the file contents to python objects, saving them to a variable named `data`

```python
with open('hosts.yaml', 'r') as f:
    stream = f.read()
    data = yaml.load(stream, Loader=yaml.SafeLoader)
```

**\*\*NOTE\*\***

[PyYAML yaml.load(input) Deprecation](https://github.com/yaml/pyyaml/wiki/PyYAML-yaml.load(input)-Deprecation)


Now that you have opened the file, we read the contents into a variable named **stream**, and loaded the YAML data into a standard Python object.  Thata could be a dictionary or list so you can work with the data like any other native Python object.

## Practice accessing data
Once you have the data loaded into a native Python object, you can access the attributes of the object using standard Python access methods.

Once the data has been imported using the PyYaml library it will be a Python dictionary.

You can find out what type of Python object your data is by using the `type` built in function.

1. Run your new script file using the `-i` flag for interactive mode.
```shell
python -i scripts/yaml_data.py
```
Running the script with the `-i` option will drop you into the Python interpretter after running the script.  This provides you with access to all of the objects that were instantiated by the script.  In our case, we now have an object named `data` that includes the contents we loaded from our file.

2. Check the type of object using `type()`

```python
print(type(data))
```

OK, we can see that we have a dictionary but what are the keys of our dictionary?
```shell
>>> print(type(data))
<class 'dict'>
```

3. Check the keys with the .keys() dictionary method.

```python
print(data.keys())
```
The output will show that we have a single key in our dictionary.

```shell
>>> print(data.keys())
dict_keys(['devices'])
>>>
```
4.  You can iterate down into the dictionary in this way

```shell
>>> data['devices'].keys()
dict_keys(['nxos1', 'dna-3-a1', 'dna-3-a2', 'dna-3-d1', 'dna-3-d2'])
>>>
```

Now we can see that we have a key named devices which then has a serveral keys that equate to the name of our devices.

5. Next, let's loop through the data and access the data
```python
for host, attrs in data['devices'].items():
    print(attrs.get('data'))
```

The output should look similar to this:

```shell
>>> for host, attrs in data['devices'].items():
...     print(attrs.get('data'))
...
{'site': 'atc56', 'role': 'access', 'type': 'network_device'}
{'site': 'atc56', 'role': 'access', 'type': 'network_device'}
{'site': 'atc56', 'role': 'access', 'type': 'network_device'}
{'site': 'atc56', 'role': 'distribution', 'type': 'network_device'}
{'site': 'atc56', 'role': 'distribution', 'type': 'network_device'}
{'role': 'distribution', 'site': 'atc56', 'type': 'network-device'}
>>>
```
## Manipulate data
This section will now focus on making some changes to our data set.

1. Create a new device entry dictionary.
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

2. Add our new device to our `data['devices']` dictionary.
- Dictionaries have an `.update()` object method that takes a dictionary argument to add to the existing dictionary.  In our case, the dictionary we want to add a key to is actually nested one level deep inside of `devices`.

```python
data['devices'].update(nxos2)
```

We can look at our newly added key and value by typing:

```python
data['devices']['nxos2']
```

```shell
>>> data['devices']['nxos2']
{'data': {'role': 'distribution', 'site': 'atc56', 'type': 'network-device'}, 'groups': ['dna_3'], 'hostname': 'nxos2', 'platform': 'nxos', 'username': 'wwt', 'password': 'WWTwwt1!', 'port': '22'}
```

## Write back to a new file
The last step we will take with this YAML data journey is to write the new data out to a new YAML file.

1. Create a variable to hold your serialized YAML string

```shell
yaml_string = yaml.dump(data)
```

2. Save our string to a new file

```python
with open('files/new_yaml_data.yml', 'w') as f:
    f.write(yaml_string)
```

3. Exit the Python interactive shell and inspect your newly created file
```shell
cat files/new_yaml_data.yml
```

`cat` is a linux command line tool that allows you to output the contents of a text file to the terminal.

## REFERENCES
[PyYAML Documentation](https://pyyaml.org/wiki/PyYAMLDocumentation)

## Nav
[Lab 2](./json-lab-2.md)

[Home](../README.md)