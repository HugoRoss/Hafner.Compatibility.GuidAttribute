# Hafner.Compatibility.GuidAttribute

This micro repository has the sole purpose to provide class `System.Runtime.InteropServices.GuidAttribute` for frameworks 
that do not include it, namely `.NET Standard 1.0`.

For all the other supported frameworks the assembly is only an empty shell.

### Multi-Targeting Project Structure

This multi-targeting project uses the following structure to represent the 4 different kind of source files (empty folders are omitted):

| Source File Kind                 | Subfolder                        | Description                                                                                                              | Project Included        |
|:---------------------------------|:---------------------------------|:-------------------------------------------------------------------------------------------------------------------------|:-----------------------:|
| Full Types Framework Agnostic    | \                                | Normal types that are identical for all supported frameworks.                                                            | always                  |
| Full Types Framework Specific    | \SpecificFrameworks              | Normal types that are only needed in a subset of the supported frameworks.                                               | only certain frameworks |
| Partial Types Framework Agnostic | \PartialTypes                    | Partial classes/interfaces that contain the common code that is idential for all the supported frameworks.               | always                  |
| Partial Types Framework Specific | \SpecificFrameworks\PartialTypes | Partial classes/interfaces that contain the framework specific implementations for a subset of the supported frameworks. | only certain frameworks |

The framework specific folders are excluded from normal build in the project file (`*.csproj`) and only included for the according target frameworks.

Here a sample of the relevant elements in the project file (`*.csproj`):

    <PropertyGroup>
      <FrameworksLackingGuidAttribute>|netstandard1.0|</FrameworksLackingGuidAttribute>
    </PropertyGroup>
    <ItemGroup>
      <Compile Remove="SpecificFrameworks\**\*.cs" />
      <None Include="SpecificFrameworks\**\*.cs" />
    </ItemGroup>
    <ItemGroup Condition="$(FrameworksLackingGuidAttribute.Contains('|$(TargetFramework)|'))">
      <None Remove="SpecificFrameworks\**\*.cs" />
      <Compile Include="SpecificFrameworks\**\*.cs" />
    </ItemGroup>
