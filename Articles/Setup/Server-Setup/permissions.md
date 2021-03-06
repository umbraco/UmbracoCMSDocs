---
v8-equivalent: "https://github.com/umbraco/UmbracoDocs/blob/main/Getting-Started/Setup/Server-Setup/permissions.md"
versionFrom: 9.0.0
verified-against: alpha-4
state: partial
updated-links: true
---

# File and folder permissions
<!--- THIS NEEDS TO BE REVISITED
:::note
To ensure a stable and smoothly running Umbraco installation, these permissions need to be set correctly. These permissions should be setup before or during the installation of Umbraco.

The main account that requires 'modify' file permissions to be set on the folders below, is the account used by the Application Pool Identity for the IIS website. Usually IIS APPPOOL\appPoolName or a specific local account or in some circumstances Network Service - If in doubt, ask your server admin / hosting company. Additionally the IUSR account and IIS_IUSRS account only require 'read only' access to the site's folders.

Generally when developing locally with Visual Studio or WebMatrix, permissions do not need to be strictly applied.
:::

:::note
If you have any specific static files / media items / etc then you should add the appropriate permissions accordingly.
The permissions documentation as it is should allow you to run a plain Umbraco install successfully, the rest.. is up to you! 👍
::: 
-->


<table border="0" style="margin-top:20px;">
<thead>
<tr>
<th>File / folder</th>
<th>Permission</th>
<th>Comment</th>
</tr>
</thead>

<tbody>
<tr>
<th>/umbraco</th>
<th>Modify / Full control</th>
<td>
<p>Should always have modify rights as the folder and its files
are used for cache and storage</p>
</td>
</tr>
<tr>
<th>/App_Plugins</th>
<th>Modify / Full control</th>
<td>
<p>Should always have modify rights as the folder and its files
are used by packages</p>
</td>
</tr>
<tr>
<th>/bin</th>
<th>Modify / Full control</th>
<td>
<p>Needed for installing packages, if no packages are installed,
this can be set to read access only</p>
</td>
</tr>
<tr>
<th>/wwwroot</th>
<th>Modify / Full control</th>
<td>
<p>Should always have modify rights as the folder and its files
are used for serving static files (<em>css files, media files uploaded via Umbraco CMS interface, script files</em>)</p>
</td>
</tr>
<tr>
<th>/wwwroot/umbraco</th>
<th>Modify / Full control</th>
<td>
<p>For upgrades and package installation, it should have modify
rights, but can be set to read-only afterwards</p>
</td>
</tr>
<tr>
<th>/Views</th>
<th>Modify / Full control</th>
<td>
<p>Should always have modify rights as the folder and its files
are used for template, partial view and macro files</p>
</td>
</tr>
</tbody>
</table>