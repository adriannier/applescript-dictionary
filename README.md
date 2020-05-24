# AppleScript Dictionary v1

## Overview

A native AppleScript implementation of a mutable key/value storage. The resulting data can be written to disk as a raw text `.applescript` file and both opened with a text editor and loaded when run within Script Editor or using osascript. 

Values of type `integer`, `real`, and `date` are converted to text during serialization and the process is reversed when reading the file back. This should work regardless of any differences in the sending or receiving system’s date and number formatting settings.

Example:

```
set dict to newDictionary()

dict's addValueForKey("some_string", "Hello World")
dict's addValueForKey("some_date", date "Thursday, 1955-02-24 at 9:41:00 AM")
	
dict's addValueForKeyPathRecursively("constants/math/pi", 3.14159265359)
dict's addValueForKeyPath("constants/math/phi", 1.61803398875)
dict's addValueForKeyPath("constants/math/one", 1)
dict's addValueForKeyPath("constants/math/infinity", "∞")
	
dict's addValueForKey("some_list", {"A", 1, 2.0, true, "E"})

dict's addValueForKey("delete_me", "This will not be in the dictionary")
dict's removeValueForKey("delete_me")
	
dict's writeToFile("~/Desktop/DictionaryTest.applescript")
```

The output saved to file after running the script above looks like this:

```
{dict:  ¬
    { ¬
        {_:"some_string", s:"Hello World"}, ¬
        {_:"some_date",   t:"1955-02-24 09:41:00"}, ¬
        {_:"constants",   d: ¬
            {dict:  ¬
                { ¬
                    {_:"math", d: ¬
                        {dict:  ¬
                            { ¬
                                {_:"pi",       f:"3.14159265359"}, ¬
                                {_:"phi",      f:"1.61803398875"}, ¬
                                {_:"one",      i:"1"}, ¬
                                {_:"infinity", s:"∞"} ¬
                            }, v:1 ¬
                        } ¬
                    } ¬
                }, v:1 ¬
            } ¬
        }, ¬
        {_:"some_list",   a: ¬
            { ¬
                {s:"A"}, ¬
                {i:"1"}, ¬
                {f:"2.0"}, ¬
                {b:true}, ¬
                {s:"E"} ¬
            } ¬
        } ¬
    }, v:1 ¬
}
```

Special care was placed upon the human readability of the produced output. Once compiled in Script Editor the white space might minimally change. Notice that each key/value pair is a native AppleScript record. The following property names are used:

- `_` Key (underscore)
- `d` dictionary
- `b` boolean
- `i` integer
- `f` real (float)
- `s` text (string)
- `t` date (time)
- `a` list (array)
- `r` record
- `v` unknown value

## [run](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L89)

Runs all tests.

## [shortTest()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L101)

Performs a short test.

## [speedTest()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L140)

Performs a speed test creating 1000 dictionary items and modifying them.

## [longTest()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L182)

Performs a long test.

## [newDictionaryWithValueForKey(k, v)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L433)

Creates a new dictionary adding the value for the specified key.

## [newDictionary()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L447)

Returns an empty dictionary.

## [Script: Dictionary](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L453)

### General

#### [isDictionary(obj)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L473)

Checks whether the specified object is a dictionary.

Example:
```
set dict to newDictionary()
set dict2 to newDictionary()
return dict's isDictionary(dict2)
```

#### [empty()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L496)

Checks whether this dictionary is empty.

Example:
```
set dict to newDictionary()

if dict's empty() then

	-- Do something
	
end if
```

#### [allKeys()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L527)

Returns all keys.

Example:
```
set dict to newDictionary()

repeat with i from 1 to 4

	dict's addValueForKey("key_" & i as text, i)
	
end repeat

return dict's allKeys()
```

#### [keyCount()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L556)

Returns the count of all keys.

```
set dict to newDictionary()

repeat with i from 1 to 4

	dict's addValueForKey("key_" & i as text, i)
	
end repeat

return dict's keyCount()
```

#### [allKeysRecursively()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L584)

Returns the keys of this dictionary and all its nested dictionaries. The keys of nested dictionaries are returned as key paths.

```
set dict to newDictionary()

dict's addValueForKeyPathRecursively("a/b/c/d/e/f", "test")
dict's addValueForKeyPathRecursively("a/b/x/d/e/f", "test")
dict's addValueForKeyPathRecursively("z/b/x/d/e/f", "test")
				
return dict's allKeysRecursively()
```

#### [allValues()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L610)

Returns all values of this dictionary.

```
set dict to newDictionary()

repeat with i from 1 to 4

	dict's addValueForKey("key_" & i as text, i)
	
end repeat

return dict's allValues()
```

### Key

#### [existsKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L649)

Returns true if the specified key exists, otherwise false.

#### [tryKey(aKey, defaultValue)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L667)

If the specified key exists, its value is returned. Otherwise the value specified as **defaultValue** is returned.

#### [addValueForKey(aKey, aValue)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L688)

Adds the value for the specified key. The key must not exists. If it does an error 2 is raised.

#### [removeValueForKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L716)

Removes the value for the specified key. The key must exist otherwise error 1 is raised.

#### [setValueForKey(aKey, aValue)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L746)

Sets the value for the specified key. The key must exist otherwise error 1 is raised.

#### [valueForKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L765)

Returns the value for the specified key. The key must exist otherweise error 1 is raised.

#### [typeForKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L783)

Returns a string representing the type of the value for the specified key. The key must exist otherweise error 1 is raised.

#### [classForKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L799)

Returns the AppleScript class for the value of the specified key. The key must exist otherweise error 1 is raised.

#### [positionForKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L815)

Returns the 1-based position for the specified key. The key must exist otherweise error 1 is raised.

#### [indexForKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L833)

Returns the 0-based index for the specified key. The key must exist otherweise error 1 is raised.

### Key_path

#### [existsKeyPath(keyPath)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L854)

Returns true if the key path can be fully satisified, otherwise false.

#### [tryKeyPath(keyPath, defaultValue)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L870)

Returns the value for the key path if a value is found, other **defaultValue** is returned.

#### [addValueForKeyPath(keyPath, aValue)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L892)

Adds the specified value at the key path. The dictionaries found along the path must exist. The final key must not exist.

#### [addValueForKeyPathRecursively(keyPath, aValue)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L916)

Adds the specified value at the key path. The dictionaries found along the path **will be created automatically**. The final key must not exist.

#### [removeValueForKeyPath(keyPath)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L962)

Removes the value at the specified key path.

#### [setValueForKeyPath(keyPath, aValue)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L991)

Sets the value at the specified key path. The final key and all the keys along the path must exist.

#### [valueForKeyPath(keyPath)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1015)

Returns the value at the specified key path.

#### [typeForKeyPath(keyPath)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1031)

Returns a string representing the type of the value at the specified key path. Error 1 is raised when any of the keys along the path do not exist.

#### [classForKeyPath(keyPath)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1047)

Returns the AppleScript class for the value at the specified key path. Error 1 is raised when any of the keys along the path do not exist.

### Input_Output

#### [writeToFile(filePath)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1066)

Writes this dictionary to a text file at the specified path. The path should end with .applescript

#### [readFromFile(filePath)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1117)

Reads data for this dictionary from a text file generated by **writeToFile()** at the specified path.

### Representations

#### [textRepresentation()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1160)

Returns a textual representation of this dictionary.

#### [recordRepresentation()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1178)

Returns this dictionary as an AppleScript record

### Private

Below are undocumented private methods that could change at any time.

#### [_pos(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1225)
#### [_add(aKey, aValue, valueType)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1275)
#### [_set(pos, aKey, aValue, valueType)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1298)
#### [_sanitizeKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1319)
#### [_sanitizeKeyPath(keyPath)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1331)
#### [_existsKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1349)
#### [_existsKeyPath(keyPath, lastItem)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1361)
#### [_key(pos)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1382)
#### [_longestKey()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1394)
#### [_recursiveKeys()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1418)
#### [_value(pos)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1451)
#### [_valueForKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1463)
#### [_valueForKeyPath(keyPath, lastItem)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1476)
#### [_valueFromPair(pair)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1488)
#### [_valueFromPairWithType(pair, valueType)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1500)
#### [_type(pos)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1548)
#### [_typeForPair(pair)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1560)
#### [_typeForValue(aValue)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1618)
#### [_typeForKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1653)
#### [_classForPair(pair)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1665)
#### [_pairForKey(aKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1701)
#### [_pairForKeyPath(keyPath, lastItem)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1713)
#### [_newPair(aKey, aValue, valueType)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1749)

### Conversion

#### [_convertLoadedDict(loadedDict)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1797)
#### [_convertLoadedValue(theValue, valueType)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1827)
#### [_convertLoadedList(loadedList)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1860)
#### [_convertLoadedInteger(str)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1885)
#### [_convertLoadedFloat(str)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1897)
#### [_initDecimalPointSymbol()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1917)

### Dumping

#### [_letterForType(valueType)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1938)
#### [_dump(anItem, depthLevel, dictKey)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L1968)
#### [_listTextRepresentation(aList, depthLevel)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2035)
#### [_dictTextRepresentation(depthLevel)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2078)
#### [_toString(var)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2124)

### List

#### [_removePositionFromList(aList, aPosition)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2169)
#### [_implodeList(aList, lastItem, aDelimiter)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2211)
#### [_explodeString(aString, lastItem, aDelimiter)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2232)

### Date

#### [_dateFromTimestamp(aTimestamp)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2264)
#### [_timestamp(aDate)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2292)

### Utilities

#### [_handleError(fnc, eMsg, eNum)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2346)
#### [_snr(aText, aPattern, aReplacement)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2365)
#### [_genIndt(depthLevel)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2396)
#### [_padWithSuffix(aText, newWidth, aSuffix)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2413)
#### [_uuid()](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2451)
#### [_hfsPath(aPath)](https://github.com/adriannier/applescript-dictionary/blob/master/Dictionary.applescript#L2479)