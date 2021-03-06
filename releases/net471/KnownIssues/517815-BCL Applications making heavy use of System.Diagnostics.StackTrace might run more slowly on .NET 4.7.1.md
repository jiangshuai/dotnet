# Applications making heavy use of System.Diagnostics.StackTrace or Exception.StackTrace might run more slowly on the .NET Framework 4.7.1

## Symptoms
An application that had acceptable performance running on the .NET Framework 4.7 or earlier versions executes more slowly when running on the .NET Framework 4.7.1

## Cause
The .NET Framework 4.7.1 added support for detecting and parsing the Portable PDB file format to show file and line number information in stack traces. As part of this change, each function in a 
stack trace has its defining module checked to determine if that module uses the Portable PDB format. Due to some differences in the internal caching policy, the 
runtime spends far more time searching for Portable PDBs than previous .NET Framework versions spent searching for classic Windows PDBs. This causes formatted stack traces to be
produced more slowly than before. 

The issue was originally reported here: https://github.com/Microsoft/dotnet/issues/529

## Workarounds
Call the [System.Diagnostics.StackTrace.#ctor(Boolean)](https://docs.microsoft.com/dotnet/api/system.diagnostics.stacktrace.-ctor?view=netframework-4.7.1#System_Diagnostics_StackTrace__ctor_System_Boolean_) constructor with `false` argument to avoid capturing source information. This avoids the portion of the code where performance
regressed.

## Resolution
The fix for this issue is expected in a future servicing update. 
