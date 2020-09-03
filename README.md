# symcon

This python module is for api communication with smarthome software ip-symcon (https://www.symcon.de).
> Requires at minimum python 3.6 and ip-symcon 3.0. Some functions may require newer versions.

Following errors on api call will throw an exception:
* http return code <> 200
* error message received by ip-symcon (eg. requested object not found)


## Installation
See [PyPI page](https://pypi.python.org/pypi?:action=display&name=symcon) for details.

```bash
pip install symcon
```

## Usage examples
To establish symcon connection the following properties are required:

* servername
* port (default: 37777)
* protocol (http or https)
* username (or string "token")
* password (or token value)

```python
import symcon
connection = symcon.Symcon("symcon.local",3777,"http","token","123456789")
```

### Execute command in ip-symcon
```python
print("symcon dir: " + connection.execCommand("IPS_GetKernelDir"))
```
> symcon dir: /var/lib/symcon/

### Execute script
```python
connection.execScript(36773)
```
### Get variable value
Can return variable value in raw or formatted form (with prefix).
```python
print(connection.getValue(19920,True))
```
> 20.4 °C

```python
print(connection.getValue(19920,False))
```
> 20.400000000000002

### Set variable value
```python
connection.setValue(16412,False)
```

### Request action
Execute default or user defined action script for provided variable id.
```python
connection.requestAction(35615,0.4)
```

### Get object details
```python
print(connection.getObjDetails(32926))
```
```json
{"ParentID": 0, "ObjectID": 32926, "ObjectType": 1, "ObjectIdent": "", "ObjectName": "Archive Handler", "ObjectInfo": "", "ObjectIcon": "", "ObjectSummary": "", "ObjectPosition": 0, "ObjectIsReadOnly": false, "ObjectIsHidden": false, "ObjectIsDisabled": false, "ObjectIsLocked": false, "HasChildren": false, "ChildrenIDs": []}
```

### Find object by name within parent object
Show id of "Sunrise" object under id 147888 ("Location")
```python
print(connection.getIdByName("Sunrise",14788))
```

### List details on child objects
List objects with details under the provided id
```python
print(connection.getChildsList(20147))
```
```json
["{\"ParentID\": 14788, \"ObjectID\": 47732, \"ObjectType\": 2, \"ObjectIdent\": \"AstronomicTwilightEnd\", \"ObjectName\": \"Astronimoc twilight end\", \"ObjectInfo\": \"\", \"ObjectIcon\": \"\", \"ObjectSummary\": \"\", \"ObjectPosition\": 8, \"ObjectIsReadOnly\": true, \"ObjectIsHidden\": false, \"ObjectIsDisabled\": false, \"ObjectIsLocked\": false, \"HasChildren\": false, \"ChildrenIDs\": []}"]
```

## Release history
* 0.0.1: Initial release