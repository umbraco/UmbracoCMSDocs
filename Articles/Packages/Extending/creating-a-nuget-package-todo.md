# Creating a NuGet version of a package

## Revisit this once we have a template for nuget packages.

The goal of this tutorial is to take something that extends Umbraco and create a NuGet Package for it. Like the [Creating a Package](./creating-a-package.md) tutorial we are using the [Creating a Custom Dashboard Tutorial](../../../Tutorials/Creating-a-Custom-Dashboard/index.md) as a starting point.

NuGet is the standard package manager for .NET projects. More information about NuGet and how it works can be found on the [Microsoft documentation pages for NuGet](https://docs.microsoft.com/en-us/nuget/what-is-nuget).

## NuGet package files

NuGet packages are zip files that follow a specified structure and contain a few predefined files, that tell the consuming application how to extract and install the package. 
