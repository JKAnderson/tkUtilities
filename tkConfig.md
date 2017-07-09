## tkConfig
A safe and convenient interface for reading the system config, game config, or individual .ltx files.

### Accessing config objects
Since the system config is the most commonly used, you can access it by just calling methods on the script itself.  
`tkConfig.GetString("wpn_ak74", "inv_name")`  
You can also access it through the System variable. Likewise, the game config is in Game.  
`tkConfig.System.GetString("wpn_ak74", "inv_name")`  
`tkConfig.Game.GetString("global_map", "texture")`  
To load individual .ltx files, call the script with the path.
```
local config = tkConfig("scripts\\my_config.ltx")
config:GetString("my_section", "my_field")
```

### Available methods
`SectionExists(section)` Returns true if the section exists or false if not.  
All other methods consider it a bug to be called with an invalid section.  
`FieldExists(section, field)` Returns true if the section contains the field or false if not.  
`GetBool, GetFloat, GetString, GetVector(section, field, default)` Returns the appropriate value of the field.  
If the field is not present, returns the default if provided or nil if not.  
`GetBools, GetFloats, GetStrings(section, field, default)` Returns a numerically-indexed list of values.  
Value in field is assumed to be space and/or comma-separated.  
`GetLines(section)` Returns a table of all values in the section, indexed by field names.

### Getting section objects
If you want to read multiple fields in the same section and don't feel like typing it out each time, you can get a section object with GetSection. All of the above methods are supported except SectionExists (obviously.)  
```
local section = tkConfig.GetSection("wpn_ak74")
section:GetString("inv_name")
```
When loading a specific .ltx, you can pass a main section name as well; it will be considered a bug if that section is not found, in order to detect missing files (since Xray returns an empty config instead of nil in that case.) If passed, a section object will be returned along with the config object.
```
local config, section = tkConfig("scripts\\my_config.ltx", "my_section")
section:GetString("my_field")
```
