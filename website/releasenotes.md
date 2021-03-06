# Release Notes
## 2.8.0
**Important:** This release targets full .NET framework and **may** not run on Unix machines. Although as tested it runs fine in [microsoft/dotnet 2.1 docker container](https://hub.docker.com/r/microsoft/dotnet/) on Linux, still for Unix based Continuous Integration builds it is better to use [SecurityCodeScan.VS2017 NuGet package](https://www.nuget.org/packages/SecurityCodeScan.VS2017), that targets netstandard.

Added external configuration files: per user account and per project. It allows you to customize settings from [built-in configuration](https://github.com/security-code-scan/security-code-scan/blob/master/SecurityCodeScan/Config/Main.yml) or add your specific Sinks and Behaviors. Global settings file location is `%LocalAppData%\SecurityCodeScan\config-1.0.yml` on Windows and `$XDG_DATA_HOME/.local/share` on Unix.  
An example of user's config-1.0.yml with custom Anti CSRF token:
```yml
CsrfProtectionAttributes:
  -  HttpMethodsNameSpace: Microsoft.AspNetCore.Mvc
     AntiCsrfAttribute: MyNamespace.MyAntiCsrfAttribute
```

For project specific settings add SecurityCodeScan.config.yml into a project. Go to file properties and set the *Build Action* to *AdditionalFiles*:

![image](https://user-images.githubusercontent.com/26652396/43063175-d28dc288-8e63-11e8-90eb-a7cb31900aff.png)

An example of SecurityCodeScan.config.yml with custom sink function (method that shouldn't be called with untrusted data without first being sanitized):
```yml
Version: 1.0
Sinks:
  UniqueKey:
    Namespace: MyNamespace
    ClassName: Test
    Member: method
    Name: VulnerableFunctionName
    InjectableArguments: [0]
    Locale: SCS0001
```

Audit Mode setting (Off by default) was introduced for those interested in warnings with more false positives.

## 2.7.1
Couple of issues related to VB.NET fixed:
* VB.NET projects were not analyzed when using the analyzer from NuGet.
* 'Could not load file or assembly 'Microsoft.CodeAnalysis.VisualBasic, Version=1.0.0.0...' when building C# .NET Core projects from command line with dotnet.exe

## 2.7.0
[Insecure deserialization analyzers](#SCS0028) for multiple libraries and formatters:
* [Json.NET](https://www.newtonsoft.com/json)
* [BinaryFormatter](https://msdn.microsoft.com/en-us/library/system.runtime.serialization.formatters.binary.binaryformatter(v=vs.110).aspx)
* [FastJSON](https://github.com/mgholam/fastJSON)
* [JavaScriptSerializer](https://msdn.microsoft.com/en-us/library/system.web.script.serialization.javascriptserializer(v=vs.110).aspx)
* [DataContractJsonSerializer](https://msdn.microsoft.com/en-us/library/system.runtime.serialization.json.datacontractjsonserializer(v=vs.110).aspx)
* [NetDataContractSerializer](https://msdn.microsoft.com/en-us/library/system.runtime.serialization.netdatacontractserializer(v=vs.110).aspx)
* [XmlSerializer](https://msdn.microsoft.com/en-us/library/system.xml.serialization.xmlserializer(v=vs.110).aspx)
* and many more...

Added warning for the usage of AllowHtml attribute.  
Different input validation analyzer and CSRF analyzer improvements.

## 2.6.1
Exceptions analyzing VB.NET projects fixed.

## 2.6.0
XXE analysis expanded.
More patterns to detect Open Redirect and Path Traversal.
Weak hash analyzer fixes.
Added request validation aspx analyzer.
False positives reduced in hardcoded password manager.

Web.config analysis:
* The feature was broken. [See how to enable.](#AnalyzingConfigFiles)
* Added detection of request validation mode.
* Diagnostic messages improved.

Taint improvements:
* Area expanded.
* Taint diagnostic messages include which passed parameter is untrusted.

## 2.5.0
Various improvements were made to taint analysis. The analysis was extended from local variables into member variables.
False positive fixes in:
* XSS analyzer.
* Weak hash analyzer. Added more patterns.
* Path traversal. Also added more patterns.

New features:
* Open redirect detection.
