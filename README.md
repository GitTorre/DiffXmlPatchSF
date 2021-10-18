# XmlDiffPatchSF

Simple command line utility that will diff/patch one version of a Service Fabric configuration file (ApplicationManifest.xml or Settings.xml) into another, later version, preserving setting values of the earlier version.
This makes it trivial to update a new version of an SF base app configuration file with the current settings you have established for an earlier version, removing the need to do this manually everytime these files are updated in a new release of some application. The patched file will include all the new settings for the latest version while preserving all of the old settings even if they do not exist in the new config (this is important for [FabricObserver](https://github.com/microsoft/service-fabric-observer) plugin configuration, for example).

As mentioned above, today this just supports ApplicationManifest.xml and Settings.xml files, but can very easily be extended to support other XML files you use for SF application configuration. 

This utility employs the old-yet-still-used-by-many XML diff/patch tool [XmlDiffPatch](https://www.nuget.org/packages/XMLDiffPatch/). 

XmlDiffPatchSF is built as a .NET Framework (v472) (Windows-only) console application.

### Usage

Help:

```XmlDiffPatchSF ? ```

Diff/Patch ApplicationManifest.xml:

``` XmlDiffPatchSF "C:\repos\FO\3.1.17\configs\ApplicationManifest.xml" "C:\repos\FO\3.1.18\configs\ApplicationManifest.xml" ``` 

The above command will produce a patched file, C:\repos\FO\3.1.18\configs\ApplicationManifest_patched.xml, containing any new settings for the latest version and carrying over the settings established in the earlier version. 

You can optionally provide a third parameter which is the full path to the patched file: 

``` XmlDiffPatchSF "C:\repos\FO\3.1.17\configs\ApplicationManifest.xml" "C:\repos\FO\3.1.18\configs\ApplicationManifest.xml" "C:\repos\FO\3.1.18\configs\ApplicationManifest.xml" ``` 

The above command produces a patched version of the latest ApplicationManifest that contains settings values from the earlier (current) version. 

Note that if your current config files contain elements that the new (latest, target) file does not, then they will be carried over. This is to support FabricObserver Plugins and their related configuration settings in Settings.xml and ApplicationManifest.xml. This tool was written to primarily support FabricObserver customers given how often new releaes of FO are released and the changes that take place in its configuration files. That said, you can see how easy it would be to change this utility to support a wide range of application configurations (XML-based).

**Make sure you run this utility over both ApplicationManifest and Settings XML files as new settings added to latest ApplicationManifest will also be present in the latest Settings.xml file.** 

``` XmlDiffPatchSF "C:\repos\FO\3.1.17\configs\Settings.xml" "C:\repos\FO\3.1.18\configs\Settings.xml" ``` 

It should be easy to run this utility in a devops workflow. 
