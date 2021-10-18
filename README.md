# DiffPatchXmlSF

Simple command line utility based on an old Microsoft diff/patch tool, XmlDiffPatch, that will diff/patch one version of a Service Fabric configuration file (ApplicationManifest.xml or Settings.xml) into another, later version, preserving setting values of the earlier version.
This makes it trivial to update a new version of an SF base app configuration file with the current settings you have established for an earlier version, removing the need to do this manually everytime these files are updated in a new release of some application. The patched file will include all the new settings for the latest version while preserving all of the old settings, assuming they exist in the new version.

As mentioned above, today this just supports ApplicationManifest.xml and Settings.xml files, but can very easily be extended to support other XML files you use for SF application configuration. 

This supports the Windows-only nuget package [XmlDiffPatch](https://www.nuget.org/packages/XMLDiffPatch/).

### Usage

Help:

```DiffPatchXmlSF ? ```

Diff/Patch ApplicationManifest.xml:

``` DiffPatchXmlSF "C:\repos\FO\3.1.17\configs\ApplicationManifest.xml" "C:\repos\FO\3.1.18\configs\ApplicationManifest.xml" ``` 

The above command will produce a patched file, C:\repos\FO\3.1.18\configs\ApplicationManifest_patched.xml, containing any new settings for the latest version and carrying over the settings established in the earlier version. 

You can optionally provide a third parameter which is the full path to the patched file: 

``` DiffPatchXmlSF "C:\repos\FO\3.1.17\configs\ApplicationManifest.xml" "C:\repos\FO\3.1.18\configs\ApplicationManifest.xml" "C:\repos\FO\3.1.18\configs\ApplicationManifest.xml" ``` 

The above command produces a patched version of the latest ApplicationManifest that contains settings values from the earlier (current) version. 

Note that if your current config files contain elemements that the new file does not, they will be carried over. This is to support FO Plugins and their related configuration settings in Settings.xml and ApplicationManifest.xml.

It should be easy to run this utility in a devops workflow. 
