---
main_header: Documentation
sub_header: Artifact Configuration
layout: resources
toc: true
show_toc: 3
description:
---

## Abstract

The artifact configuration describes the structure of the artifacts you want to sign. For simple artifacts, you can use predefined configurations to get started quickly. For signing several artifacts together, and for more complex artifacts, specify the structure of your artifact and provide signing directives using XML.

![Artifact configuration XML](/assets/img/resources/documentation_artifact-configuration.png)

## Creating and editing artifact configurations

### Upload an artifact sample

Before you start creating an artifact configuration from scratch, we recommend that you upload an artifact sample first. SignPath will then analyze your sample and look for nested artifacts that can be signed. 

For non-trivial artifacts, you may want to edit the resulting artifact configuration: 

* It may contain signing instructions for files that you do not want to sign, such as third-party components.
* It may contain fixed strings, such as version numbers, that you want to replace with [wildcards](#using-wildcards).
* It may have very similar sections that you want to unify using [`<directory>`](#directory-element) elements and wildcards ([example](#signing-similar-directories-within-an-msi-file)).

### Create an artifact configuration

When you create a new project, a default artifact configuration will be added. To create additional artifact configurations, select the project and click **Add** in the **Artifact Configurations** section. In either case, you can

* select **Upload an artifact sample** and select an artifact file to have the artifact configuration generated
* select a **Ready to use artifact configuration**
* select one of the **Templates for custom artifact configurations** and edit the XML content
* select **Custom** and create an artifact configuration from scratch

### Edit an artifact configuration

To modify an existing artifact configuration, select a project, then select the artifact configuration you want to change, and then either

* select **Edit** to select pre-defined artifact configurations or edit the XML content
* select **Update from an artifact sample** to generate your artifact configuration from a sample file

When you edit XML content in place, a graphical representation will be rendered immediately. If you prefer to use code completion, select **Download XML schema** and use a schema-aware XML editor, such as Microsoft Visual Studio.

Read more about projects and artifact configurations in [Setting up Projects](/documentation/projects).

## Deep signing

In case you have more complex, nested artifacts, you might want to not only sign the container itself (for instance, an MSI installer package), but also all files that are shipped within the container (e.g., .exe and .dll files within the MSI installer). 

Therefore, every *container* format can contain multiple other *file* or *directory* elements to be signed. Each of those will be extracted, signed, and then put back into the container file during the signing process. All inner elements need a `path` attribute.

## Defining file and directory structure

Every XML description is wrapped in an `<artifact-configuration>` root element which contains exactly one file element. This file element specifies the type of artifact and signing method. Optionally, you can restrict the name of the file using the `path` attribute.

Container-format elements may contain other file elements for deep signing.

### Platform considerations

File and directory names in `path` attributes are case-insensitive. You may use slash `/` or backslash `\` as directory separators.

## File elements

### File element types and signing directives

<table id="signing-file-elements">
<thead>
  <tr>
    <th style="width: 9em;">Element</th>
    <th>Container format</th>
    <th style="width: 10em;">Signing directive</th>
    <th>Extensions</th>
    <th>Description</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td><code>&lt;pe-file&gt;</code></td>
    <td>No</td>
    <td><code><a href="#authenticode-sign">&lt;authenticode-sign&gt;</a></code></td>
    <td>.acm, .ax, .cpl, .dll, .drv, .efi, .exe, .mui, .ocx, .scr, .sys, .tsp</td>
    <td>Portable Executable (PE) files: EXE, DLL, and other executable files</td>
  </tr>
  <tr>
    <td><code>&lt;powershell-file&gt;</code></td>
    <td>No</td>
    <td><code><a href="#authenticode-sign">&lt;authenticode-sign&gt;</a></code></td>
    <td>.ps1, .psm1, psd1, .psdc1, .ps1xml</td>
    <td>PowerShell scripts and modules</td>
  </tr>
  <tr>
    <td><code>&lt;msi-file&gt;</code></td>
    <td>Yes</td>
    <td><code><a href="#authenticode-sign">&lt;authenticode-sign&gt;</a></code></td>
    <td>.msi, .msm, .msp</td>
    <td>Microsoft installer files</td>
  </tr>
  <tr>
    <td><code>&lt;cab-file&gt;</code></td>
    <td>Yes</td>
    <td><code><a href="#authenticode-sign">&lt;authenticode-sign&gt;</a></code></td>
    <td>.cab</td>
    <td>Windows cabinet files</td>
  </tr>
  <tr>
    <td><code>&lt;catalog-file&gt;</code></td>
    <td>Yes</td>
    <td><code><a href="#authenticode-sign">&lt;authenticode-sign&gt;</a></code></td>
    <td>.cat</td>
    <td>Windows catalog files</td>
  </tr>
  <tr>
    <td><code>&lt;appx-file&gt;</code></td>
    <td>Yes</td>
    <td><code><a href="#authenticode-sign">&lt;authenticode-sign&gt;</a></code></td>
    <td>.appx, .appxbundle</td>
    <td>
      App packages for Microsoft Store/Universal Windows Platform<br>
      <svg class="icon blog" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32"><path fill="currentColor" d="M 16 3 C 8.832031 3 3 8.832031 3 16 C 3 23.167969 8.832031 29 16 29 C 23.167969 29 29 23.167969 29 16 C 29 8.832031 23.167969 3 16 3 Z M 16 5 C 22.085938 5 27 9.914063 27 16 C 27 22.085938 22.085938 27 16 27 C 9.914063 27 5 22.085938 5 16 C 5 9.914063 9.914063 5 16 5 Z M 15 10 L 15 12 L 17 12 L 17 10 Z M 15 14 L 15 22 L 17 22 L 17 14 Z"/></svg>
       Deep signing is not yet supported.<br>
      <svg class="icon blog" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32"><path fill="currentColor" d="M 16 3 C 8.832031 3 3 8.832031 3 16 C 3 23.167969 8.832031 29 16 29 C 23.167969 29 29 23.167969 29 16 C 29 8.832031 23.167969 3 16 3 Z M 16 5 C 22.085938 5 27 9.914063 27 16 C 27 22.085938 22.085938 27 16 27 C 9.914063 27 5 22.085938 5 16 C 5 9.914063 9.914063 5 16 5 Z M 15 10 L 15 12 L 17 12 L 17 10 Z M 15 14 L 15 22 L 17 22 L 17 14 Z"/></svg>
       The Common Name of the code signing certificate must match the PublisherDisplayName in the AppxManifest.xml file.
  </td>
  </tr>
  <tr>
    <td><code>&lt;opc-file&gt;</code></td>
    <td>Yes</td>
    <td><code><a href="#opc-sign">&lt;opc-sign&gt;</a></code></td>
    <td>.vsix, .xps, ...</td>
    <td>Open Packaging Conventions (OPC) files including Visual Studio Extensions (VSIX)</td>
  </tr>
  <tr>
    <td><code>&lt;nupkg-file&gt;</code></td>
    <td>Yes</td>
    <td><code><a href="#nuget-sign">&lt;nuget-sign&gt;</a></code></td>
    <td>.nupkg</td>
    <td>NuGet packages</td>
  <tr>
    <td><code>&lt;jar-file&gt;</code></td>
    <td>Yes</td>
    <td><code><a href="#jar-sign">&lt;jar-sign&gt;</a></code></td>
    <td>.jar, .war, .ear, .apk, .aab</td>
    <td>Java archives and Android apps.</td>
  </tr>
  </tr>
    <tr>
    <td><code>&lt;zip-file&gt;</code></td>
    <td>Yes</td>
    <td><code><a href="#jar-sign">&lt;jar-sign&gt;</a></code></td>
    <td>.zip</td>
    <td>Use ZIP archives to sign multiple files at once</td>
  </tr>
  <tr>
    <td><code>&lt;directory&gt;</code></td>
    <td>Yes</td>
    <td><code><a href="#clickonce-sign">&lt;clickonce-sign&gt;</a></code></td>
    <td></td>
    <td>Directories within container files (e.g. ZIP, MSI)</td>
  </tr>
</tbody>
</table>

### File element examples

#### Signing an MSI package

~~~ xml
<artifact-configuration xmlns="http://signpath.io/artifact-configuration/v1">
  <msi-file>
    <authenticode-sign/>
  </msi-file>
</artifact-configuration>
~~~

See [Examples] for more complex artifact configurations.

### Signing multiple artifacts

If you want to sign multiple independent artifacts in one step, you need to package them into a ZIP archive before processing.

You can combine signing multiple artifacts with deep signing.

## &lt;directory&gt; element

All supported container formats have an internal directory structure. You can see this structure if you extract a container to a local disk.

You can either specify these directories in the `path` attribute of each file element or nest these file elements within `<directory>` elements.

`<directory>` elements are also used for [ClickOnce signing].

### &lt;directory&gt; example

<table>
  <thead>
    <th>The following fragment</th>
    <th>is equivalent to</th>
  </thead>
  <tbody> 
    <tr> 
      <td markdown="1">

~~~ xml
<zip-file> 
  <pe-file path="bin/program.exe">
    <authenticode-sign/>
  </pe-file>
</zip-file>
~~~

</td> <td markdown="1">

~~~ xml
<zip-file>
  <directory path="bin">
    <pe-file path="program.exe">
      <authenticode-sign/>
    </pe-file>
  </directory>
</zip-file>
~~~

</td> </tr> </tbody> </table>

## Signing methods

<!-- markdownlint-disable MD026 no trailing punctuation -->

Specify signing directives in file and directory elements. See [file elements](#file-elements) for available methods per element type.

For file and directory sets, specify the signing directive in the `<for-each>` element.

### &lt;authenticode-sign&gt;

Microsoft Authenticode is the primary signing method on the Windows platform. Authenticode is a versatile and extensible mechanism that works for many different file types:

* [Portable Executable](https://en.wikipedia.org/wiki/Portable_Executable) (PE) files: EXE, DLL, and some other executable file formats including device drivers
* Installation formats: AppX, MSI, and CAB
* PowerShell scripts and modules

Using `<authenticode-sign>` is equivalent to using Microsoft's `SignTool.exe`.

[ClickOnce signing]: #clickonce-sign

### &lt;clickonce-sign&gt;

ClickOnce signing, also called 'manifest signing', is a method used for ClickOnce applications and Microsoft Office Add-ins.

ClickOnce signing applies to directories, not to individual files. Therefore, you need to specify a `<directory>` element for this method. If you want to sign files in the root directory of a container, specify `path="."`.

~~~ xml
<artifact-configuration xmlns="http://signpath.io/artifact-configuration/v1">
  <zip-file>
    <directory path=".">
      <clickonce-sign/>
    </directory>
  </zip-file>
</artifact-configuration>
~~~

Using `<clickonce-sign>` is equivalent to using Microsoft's `mage.exe`.

### &lt;nuget-sign&gt;

NuGet package signing is currently being introduced to the [NuGet Gallery](https://www.nuget.org/).

Although the NuGet Package format is based on OPC (see next section), it uses its own specific signing format.

Using `<nuget-sign>` is equivalent to using Microsoft's `nuget` `sign` command.

### &lt;opc-sign&gt;

The [Open Packaging Conventions](https://en.wikipedia.org/wiki/Open_Packaging_Conventions) (OPC) specification has its own signature format. Since OPC is the foundation for several file formats, it is not strictly a code signing format. However, code signing is used for Visual Studio Extensions (VSIX).

Other OPC formats include:

* Open XML Paper Specification (OpenXPS)
* Office Open XML files (Microsoft Office)

Note that while OPC signing can be applied to all OPC formats, specific applications and formats do not necessarily use or verify signatures, or even require a different signing format (case in point: NuGet packages).

Using `<opc-sign>` for Visual Studio Extensions is equivalent to using Microsoft's `VSIXSignTool.exe`.

<!-- markdownlint-enable MD026 no trailing punctuation -->

### &lt;jar-sign&gt;

This signing method can be used to sign the following file formats:

| Format                       | Extensions       | Remarks |
|------------------------------|------------------|---------|
| Java archives                | .jar, .ear, .war | |
| Android apps und app-bundles | .apk, .aab       | Only APK signing scheme v1 (v2 and v3 are not yet supported) |
| ZIP files                    | .zip             | Only UTF-8 encoded ZIP files are supported |

#### Verification

* **Java** always verifies signatures for client components. For server components, you will need to create a policy. Please consult the documentation of your application server or [Oracle's documentation](https://docs.oracle.com/javase/tutorial/security/toolsign/receiver.html).
* **Android** always verifies App signatures.
* If you sign **ZIP files**, the receiver needs to manually check the signature before unpacking the file.

For manual verification, use the following command (requires [JDK](https://openjdk.java.net/install/)):

~~~ cmd
jarsigner -verify -strict <file>.zip
~~~

Add the `-verbose` option to see the certificate.

## Using wildcards

Every path attribute can contain the following wildcard patterns:

| Wildcard | Description | Example | Matches |
| -------- | ----------- | ------- | ------- |
| `*`      | Matches any number of any character (including none, excluding the directory separator) | `m*y` | `my`, `mary`, `my first pony`
| `?`      | Matches  any single character                                              | `th?s`: `this`, `th$s`, but not `ths`
| `[abc]`  | Matches one character given in the bracket                                 | `[fb]oo` | `foo` and `boo`
| `[a-z]`  | Matches one character from the range given in the bracket                  | `[0-9]`  | all digits
| `[!abc]` | Matches one character that is not given in the bracket                     | `[!f]oo` | `boo` and `$oo`, but not `foo`
| `[!a-z]` | Matches one character that is not from the range given in the bracket      | `[!0-9]` | every character that is not a digit
| `**`     | Matches any number of path/directory segments. When used, they must be the only contents of the dedicated segment. | `**/*.dll` | `*.dll` files in all subdirectories (recursive)

### Number of matches

If wildcards are used, optional `max-matches` and `min-matches` parameters can be specified to set the number of files that may match the wildcard expression. 

* Both default to `1`, so wildcard expressions without `max-matches` or `min-matches` must match exactly one file
* Use `min-matches="0"` for optional elements (even without wildcards)
* Use `max-matches="unbounded"` for unlimited matches

## File and directory sets

If multiple files or directories should be handled in the same way, you can enumerate them using one of the following file or directory set elements:

* `<directory-set>`
* `<pe-file-set>`
* `<zip-file-set>`
* `<msi-file-set>`
* `<cab-file-set>`
* `<opc-file-set>`
* `<nupkg-file-set>`

Each set element contains:

* an `<include>` element for each file (or pattern) to be processed
* a `<for-each>` element that will be applied to all included elements of the set

A set's `<for-each>` element can include the same child elements as the corresponding simple file or directory element:

* `<pe-file>` supports `<authenticode-signing/>`
* therefore `<pe-file-set>` supports `<authenticode-signing/>` within the `<for-each>` element

Sets are especially useful if your artifacts contain repeating nested structures.

### File set example

<table>
  <thead>
    <th>The following fragment</th>
    <th>is equivalent to</th>
  </thead>
  <tbody> 
    <tr> 
      <td markdown="1">

~~~ xml
<pe-file path="first.dll">
  <authenticode-sign/>
</pe-file>

<pe-file path="second.dll">
  <authenticode-sign/>
</pe-file>
~~~

</td> <td markdown="1">

~~~ xml
<pe-file-set>
  <include path="first.dll" />
  <include path="second.dll" />
  <for-each>
    <authenticode-sign/>
  </for-each>
</pe-file-set>
~~~

</td> </tr> </tbody> </table>

## File attribute restrictions

For Microsoft Portable Executable (PE) files, the existence of their Product Name / Product Version header attributes can be enforced by setting the <code>productName</code> and <code>productVersion</code> attributes on the <code>&lt;pe-file&gt;</code>, <code>&lt;pe-file-set&gt;</code> and including <code>&lt;include&gt;</code> elements. The value of the <code>&lt;include&gt;</code> overrides any value set on the <code>&lt;pe-file-set&gt;</code> element.

### File attribute restriction example

~~~ xml
<artifact-configuration xmlns="http://signpath.io/artifact-configuration/v1">
  <msi-file>
    <!-- requires all pe-files to have the respective attributes set -->
    <pe-file-set productName="YourProductName" productVersion="1.0.0.0"> 
      <include path="main.exe" />
      <!-- overrides the value of the parent pe-file-set -->
      <include path="resources*.resource.dll" max-matches="unbounded" productVersion="1.0.1.0" />
      <for-each>
        <authenticode-sign />
      </for-each>
    </pe-file-set>
    <authenticode-sign /> <!-- finally sign the MSI file -->
  </msi-file>
</artifact-configuration>
~~~

## User-defined parameters

Available for Enterprise subscriptions
{: .badge.icon-signpath}

You can define parameters for each signing request. Use this to create a more restrictive artifact configuration, track values, or include build-time values.

Parameter values can be set when submitting a signing request via the user interface or API (see [documentation](/documentation/build-system-integration#submit-a-signing-request)). Actual values are displayed on the signing request details page. 

Parameters are defined in an optional <code>parameters</code> block at the beginning of the artifact configuration and can be referenced using the <code>${parameterName}</code> syntax in any XML attribute.

~~~xml
<artifact-configuration xmlns="http://signpath.io/artifact-configuration/v1">
  <parameters>
    <parameter name="productVersion" default-value="1.0.0" required="true" />
  </parameters>
  <pe-file path="my-installer-${productVersion}.exe" productVersion="${productVersion}">
</artifact-configuration>
~~~

[Examples]: #examples

## Examples

<div class='panel info' markdown='1' >
<div class='panel-header'> Examples are shortened</div>
For the sake of clarity, all examples omit the XML prolog. A complete artifact configuration looks like this:

~~~ xml
<?xml version="1.0" encoding="utf-8" ?>
<artifact-configuration xmlns="http://signpath.io/artifact-configuration/v1">
  <!-- ... -->
</artifact-configuration>
~~~
</div>

### Predefined configuration for single Portable Executable file

This configuration works for all PE files.

~~~ xml
<artifact-configuration xmlns="http://signpath.io/artifact-configuration/v1">
  <pe-file>
    <authenticode-sign/>
  </pe-file>
</artifact-configuration>
~~~

### Signing multiple artifacts in a ZIP container

You can sign multiple unrelated artifacts by packing them into a single ZIP file.

~~~ xml
<artifact-configuration xmlns="http://signpath.io/artifact-configuration/v1">
  <zip-file>
    <pe-file path="app.exe">
      <authenticode-sign/>
    </pe-file>
    <powershell-file path="setup.ps1">
      <authenticode-sign/>
    </powershell-file>
  </zip-file>
</artifact-configuration>
~~~

### Deep-signing an MSI installer

This will sign the PE files `libs/common.dll` and `main.exe`, then re-package their MSI container and sign it too. It also restricts the name of the MSI container file.

~~~ xml
<artifact-configuration xmlns="http://signpath.io/artifact-configuration/v1">
  <msi-file path="MyProduct.v*.msi">
    <pe-file path="libs/common.dll">
      <authenticode-sign/>
    </pe-file>
    <pe-file path="main.exe">
      <authenticode-sign/>
    </pe-file>
    <authenticode-sign/>
  </msi-file>
</artifact-configuration>
~~~

### Signing similar directories within an MSI file

This artifact configuration describes an MSI installer package containing several components. The components have a similar structure and are therefore defined as a `<directory-set>`. Each component contains a `main.exe` and zero or more resource DLLs.

<table class="noborder">
<tr><td markdown="1">

~~~ xml
<artifact-configuration xmlns="http://signpath.io/artifact-configuration/v1">
  <msi-file>
    <directory-set>
      <include path="component1" />
      <include path="component2" />
      <include path="component3" />
      <for-each>
        <pe-file-set>
          <include path="main.exe" />
          <include path="resources/*.resource.dll" 
                   min-matches="0" max-matches="unbounded" />
          <for-each>
            <authenticode-sign/>
          </for-each>
        </pe-file-set>
      </for-each>
    </directory-set>
    <authenticode-sign/>
  </msi-file>
</artifact-configuration>
~~~

</td><td markdown="1">

![graphical artifact configuration](/assets/img/resources/documentation_artifact-configuration_similar-definition.png)

</td></tr></table>

Example of a directory structure that would match this configuration:

<table class="noborder">
<tr><td markdown="1">

~~~
• myapp.msi 
  • component1/
    • main.exe
    • resources/de.resource.dll
    • resources/en.resource.dll
    • resources/fr.resource.dll
  • component2/
    • main.exe
  • component3/
    • main.exe
    • resources/en.resource.dll
~~~

(All `msi`, `exe` and `dll` files are signed with Authenticode.)
</td><td markdown="1">

![graphical resolved artifacts](/assets/img/resources/documentation_artifact-configuration_similar-resolved.png)

</td></tr></table>
