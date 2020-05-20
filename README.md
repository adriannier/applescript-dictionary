# Dictionary v1

A native AppleScript implementation of a key/value storage. The resulting data can be written to disk as a raw text AppleScript file (.applescript) and both opened with a text editor and loaded when run within Script Editor or using osascript.

Values of type integer, real, and date are converted to text during serialization and the processed is reversed when reading the file back. This process should work regardless of the difference in the sending or receiving system’s differences in date and number formats.

Example:

```
set dict to newDictionary()
dict's addValueForKey("last_refresh", current date)
dict's writeToFile("~/Desktop/DictionaryTest.applescript")
```


## run

Runs all tests.


## shortTest()

Performs a short test.


## speedTest()

Performs a speed test creating 1000 dictionary items and modifying them.


## longTest()

Performs a long test.


## newDictionaryWithValueForKey(k, v)

Creates a new dictionary adding the value for the specified key.


## newDictionary()

Returns an empty dictionary.


### Script: Dictionary

#### General

##### isDictionary(obj)

Checks whether the specified object is a dictionary.

Example:
```
set dict to newDictionary()
set dict2 to newDictionary()
return dict's isDictionary(dict2)
```


##### empty()

Checks whether this dictionary is empty.

Example:
```
set dict to newDictionary()

if dict's empty() then

    -- Do something
    
end if
```


##### allKeys()

Returns all keys.

Example:
```
set dict to newDictionary()

repeat with i from 1 to 4

    dict's addValueForKey("key_" & i as text, i)
    
end repeat

return dict's allKeys()
```


##### keyCount()

Returns the count of all keys.

```
set dict to newDictionary()

repeat with i from 1 to 4

    dict's addValueForKey("key_" & i as text, i)
    
end repeat

return dict's keyCount()
```


##### allKeysRecursively()

Returns the keys of this dictionary and all its nested dictionaries. The keys of nested dictionaries are returned as key paths.

```
set dict to newDictionary()

dict's addValueForKeyPathRecursively("a/b/c/d/e/f", "test")
dict's addValueForKeyPathRecursively("a/b/x/d/e/f", "test")
dict's addValueForKeyPathRecursively("z/b/x/d/e/f", "test")
                
return dict's allKeysRecursively()
```


##### allValues()

Returns all values of this dictionary.

```
set dict to newDictionary()

repeat with i from 1 to 4

    dict's addValueForKey("key_" & i as text, i)
    
end repeat

return dict's allValues()
```


#### Key

##### existsKey(aKey)

Returns true if the specified key exists, otherwise false.


##### tryKey(aKey, defaultValue)

If the specified key exists, its value is returned. Otherwise the value specified as **defaultValue** is returned.


##### addValueForKey(aKey, aValue)

Adds the value for the specified key. The key must not exists. If it does an error 2 is raised.


##### removeValueForKey(aKey)

Removes the value for the specified key. The key must exist otherwise error 1 is raised.


##### setValueForKey(aKey, aValue)

Sets the value for the specified key. The key must exist otherwise error 1 is raised.


##### valueForKey(aKey)

Returns the value for the specified key. The key must exist otherweise error 1 is raised.


##### typeForKey(aKey)

Returns a string representing the type of the value for the specified key. The key must exist otherweise error 1 is raised.


##### classForKey(aKey)

Returns the AppleScript class for the value of the specified key. The key must exist otherweise error 1 is raised.


##### positionForKey(aKey)

Returns the 1-based position for the specified key. The key must exist otherweise error 1 is raised.


##### indexForKey(aKey)

Returns the 0-based index for the specified key. The key must exist otherweise error 1 is raised.


#### Key path

##### existsKeyPath(keyPath)

Returns true if the key path can be fully satisified, otherwise false.


##### tryKeyPath(keyPath, defaultValue)

Returns the value for the key path if a value is found, other **defaultValue** is returned.


##### addValueForKeyPath(keyPath, aValue)

Adds the specified value at the key path. The dictionaries found along the path must exist. The final key must not exist.


##### addValueForKeyPathRecursively(keyPath, aValue)

Adds the specified value at the key path. The dictionaries found along the path **will be created automatically**. The final key must not exist.


##### removeValueForKeyPath(keyPath)

Removes the value at the specified key path.


##### setValueForKeyPath(keyPath, aValue)

Sets the value at the specified key path. The final key and all the keys along the path must exist.


##### valueForKeyPath(keyPath)

Returns the value at the specified key path.


##### typeForKeyPath(keyPath)

Returns a string representing the type of the value at the specified key path. Error 1 is raised when any of the keys along the path do not exist.


##### classForKeyPath(keyPath)

Returns the AppleScript class for the value at the specified key path. Error 1 is raised when any of the keys along the path do not exist.


#### Input Output

##### writeToFile(filePath)

Writes this dictionary to a text file at the specified path. The path should end with .applescript


##### readFromFile(filePath)

Reads data for this dictionary from a text file generated by **writeToFile()** at the specified path.


#### Representations

##### textRepresentation()

Returns a textual representation of this dictionary.


##### recordRepresentation()

Returns this dictionary as an AppleScript record


#### Private

Below are undocumented private methods that could change at any time.

##### _pos(aKey)
##### _add(aKey, aValue, valueType)
##### _set(pos, aKey, aValue, valueType)
##### _sanitizeKey(aKey)
##### _sanitizeKeyPath(keyPath)
##### _existsKey(aKey)
##### _existsKeyPath(keyPath, lastItem)
##### _key(pos)
##### _longestKey()
##### _recursiveKeys()
##### _value(pos)
##### _valueForKey(aKey)
##### _valueForKeyPath(keyPath, lastItem)
##### _valueFromPair(pair)
##### _valueFromPairWithType(pair, valueType)
##### _type(pos)
##### _typeForPair(pair)
##### _typeForValue(aValue)
##### _typeForKey(aKey)
##### _classForPair(pair)
##### _pairForKey(aKey)
##### _pairForKeyPath(keyPath, lastItem)
##### _newPair(aKey, aValue, valueType)

#### Conversion

##### _convertLoadedDict(loadedDict)
##### _convertLoadedValue(theValue, valueType)
##### _convertLoadedList(loadedList)
##### _convertLoadedInteger(str)
##### _convertLoadedFloat(str)
##### _initDecimalPointSymbol()

#### Dumping

##### _letterForType(valueType)
##### _dump(anItem, depthLevel, dictKey)
##### _listTextRepresentation(aList, depthLevel)
##### _dictTextRepresentation(depthLevel)
##### _toString(var)

#### List

##### _removePositionFromList(aList, aPosition)
##### _implodeList(aList, lastItem, aDelimiter)
##### _explodeString(aString, lastItem, aDelimiter)

#### Date

##### _dateFromTimestamp(aTimestamp)
##### _timestamp(aDate)

#### Utilities

##### _handleError(fnc, eMsg, eNum)
##### _snr(aText, aPattern, aReplacement)
##### _genIndt(depthLevel)
##### _padWithSuffix(aText, newWidth, aSuffix)
##### _uuid()
##### _hfsPath(aPath)
