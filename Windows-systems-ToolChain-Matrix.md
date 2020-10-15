Change **TOOL_CHAIN_TAG** in the file Conf\Target.txt for Windows Visual Studio Versions according to the following Matrix

|Visual Studio |	V. Version |	WinXP / Win7 /IA32 |	Win7 / Win8x / Win10 x64 |
| ----------------- | ------------------ | -------------- | -------------- | 
|VS2005	|8	|VS2005	|VS2005x86|
|VS2008	|9	|VS2008	|VS2008x86|
|VS2010	|10	|VS2010	|VS2010x86|
|VS2012**	|11	|VS2012	|VS2012x86|
|VS2013**	|12	|VS2013	|VS2013x86|
| | | | Below Recommended  |
|VS2015***	|14	|VS2015	|VS2015x86|
|VS2017     |15 |VS2017 |VS2017 |
|VS2019     |16 |VS2019 |VS2019 |

Example:

for Windows 10 64 bit OS and Visual Studio 2013 modify the following in Target.txt
```
From:
TOOL_CHAIN_TAG = MYTOOLS
to:
TOOL_CHAIN_TAG = VS2013x86
```

NOTE: VS2012** only with UDK2014 releases or equivalent or later UDK release
NOTE: VS2013** & VS2015** only with UDK2015 releases or equivalent or later UDK release

NOTE: VS2015*** is the current Default

VS2017 & VS2019 recomended for Pytools CI Stuart builds.