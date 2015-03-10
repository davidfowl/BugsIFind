Needs more error handling when the target framework being run that isn't in the project file:

```JSON
{
    "version": "1.0.0-*",
    "dependencies": {
    },
    "commands": {
        "run": "run"
    },
    "frameworks" : {
        "net45" : {}
    }
}
```

```
C:\Users\davifowl\Documents\Visual Studio 14\Projects\ClassLibrary29\src\ClassLibrary29> k run
System.NullReferenceException: Object reference not set to an instance of an object.
   at Microsoft.Framework.Runtime.NuGetDependencyResolver.<GetDependencies>d__13.MoveNext() in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.Runtime\DependencyManagement\NuGetDepend
cyResolver.cs:line 85
   at Microsoft.Framework.Runtime.WalkContext.<>c__DisplayClass1_0.<Walk>b__0(Node node) in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.Runtime\DependencyManagement\WalkContext.cs
ine 51
   at Microsoft.Framework.Runtime.WalkContext.<>c__DisplayClass5_0.<ForEach>b__0(Node node, Int32 _) in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.Runtime\DependencyManagement\Wa
Context.cs:line 265
   at Microsoft.Framework.Runtime.WalkContext.ForEach[TState](Node root, TState state, Func`3 visitor) in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.Runtime\DependencyManagement\
lkContext.cs:line 252
   at Microsoft.Framework.Runtime.WalkContext.ForEach(Node root, Action`1 visitor) in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.Runtime\DependencyManagement\WalkContext.cs:line
3
   at Microsoft.Framework.Runtime.WalkContext.Walk(IEnumerable`1 dependencyResolvers, String name, SemanticVersion version, FrameworkName frameworkName) in C:\dev\git\ProjectK\KRuntime\src
icrosoft.Framework.Runtime\DependencyManagement\WalkContext.cs:line 42
   at Microsoft.Framework.Runtime.DependencyWalker.Walk(String name, SemanticVersion version, FrameworkName targetFramework) in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.Runtime
ependencyManagement\DependencyWalker.cs:line 45
   at Microsoft.Framework.Runtime.DefaultHost.Initialize() in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.Runtime\DefaultHost.cs:line 74
   at Microsoft.Framework.Runtime.DefaultHost.GetEntryPoint(String applicationName) in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.Runtime\DefaultHost.cs:line 59
   at Microsoft.Framework.ApplicationHost.Program.ExecuteMain(DefaultHost host, String applicationName, String[] args) in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.ApplicationHo
\Program.cs:line 206
```



Another bug that is a bit more complicated:

A -> B

A/project.json

```JSON
{
    "version": "1.0.0-*",
    "dependencies": {
        "ClassLibrary1": ""
    },
    "commands": {
        "run": "run"
    },
    "frameworks" : {
        "dnx451" : { }
    }
}
```

B/project.json

```JSON
{
    "version": "1.0.0-*",
    "dependencies": {
        "Newtonsoft.Json": "6.0.6"
    },
    "frameworks" : {
        "net40": { }
    }
}
```

After running restore and running:

```
C:\Users\davifowl\Documents\Visual Studio 14\Projects\ClassLibrary29\src\ClassLibrary29> k run
System.InvalidOperationException: Unable to load application or execute command 'ClassLibrary29'. Available commands: run. ---> System.NullReferenceException: Object reference not set to an
instance of an object.
   at Microsoft.Framework.Runtime.NuGetDependencyResolver.PopulateMetadataReferences(PackageDescription description, FrameworkName targetFramework, IDictionary`2 paths) in C:\dev\git\Project
K\KRuntime\src\Microsoft.Framework.Runtime\DependencyManagement\NuGetDependencyResolver.cs:line 309
   at Microsoft.Framework.Runtime.NuGetDependencyResolver.GetLibraryExport(ILibraryKey target) in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework.Runtime\DependencyManagement\NuGetDepen
dencyResolver.cs:line 292
   at Microsoft.Framework.Runtime.CompositeLibraryExportProvider.<>c__DisplayClass2_0.<GetLibraryExport>b__0(ILibraryExportProvider r) in C:\dev\git\ProjectK\KRuntime\src\Microsoft.Framework
```