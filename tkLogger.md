## tkLogger
Generic logging and error-reporting. Also happens to include which-game detection. Options can be found at the top of the script.

### Game detection
tkLogger.Game has a true/false value for each game. Example:
```
local game = tkLogger.Game
if game.SOC then
	-- Do SoC things
elseif game.CS then
	-- Do CS things
elseif game.COP then
	-- Do CoP things
end
```

### Logging
To get a logging object for your script, call tkLogger like so:  
`local logger = tkLogger(script_name(), true)`  
The first value will be attached to logged messages; using script_name() is recommended so it will always use the current name of the script. The second value enables or disables the logger; bugs and raw messages will be logged even if false, however.

Each logging function takes a message and optional format parameters.  
`logger:Debug(message, ...)`  
Use this for spammy messages needed only for bug hunting.  
`logger:Info(message, ...)`  
Use this for verification of normal function.  
`logger:Warning(message, ...)`  
Use this for non-critical but potentially problematic issues.  
`logger:Bug(message, ...)`  
Use this for critical errors. Always logs to the console, flushes it, and displays a HUD (SoC and CS) or news (CoP) message.  
`logger:Confirm(condition, message, ...)`  
Convenience function for error detection. If the condition is false, the message will be sent as a bug. Returns the condition. Example:
```
function AddNums(a, b)
	if not logger:Confirm(tonumber(a) and tonumber(b),
    	"AddNums requires two numbers; got %s and %s", tostring(a), tostring(b)) then
        return 0
    else
    	return a + b
    end
end
```
`logger:Raw(message, ...)`  
Writes the message to the console without adding the source name and logging level or checking if the logger is disabled. For instance, I use it to dump packet data on request.
