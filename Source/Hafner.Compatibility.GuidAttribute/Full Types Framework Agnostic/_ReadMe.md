### Multi-Targeting Project Structure

This multi-targeting project is structured into the following 4 folders

| Folder                              | Description                                                                                                                  | Project Included  |
|:------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------|:-----------------:|
| `Full Types Framework Agnostic`    | Normal types that are identical for all the supported frameworks.                                                            | yes               |
| `Full Types Framework Specific`    | Normal types that are only needed in a subset of all the supported frameworks.                                               | only specific FWs |
| `Partial Types Framework Agnostic` | Partial classes/interfaces that contain the common code that is idential for all the supported frameworks.                   | yes               |
| `Partial Types Framework Specific` | Partial classes/interfaces that contain the framework specific implementations for a subset of all the supported frameworks. | only specific FWs |

The `* Types Framework Specific` folders are excluded from normal build in the project file (*.csproj) and only included for the according target frameworks.

Here a sample of the relevant elements in the project file (`*.csproj`):

      <PropertyGroup>
        <FrameworkLackingExtensionMethods>|net20|net30|</FrameworkLackingExtensionMethods>
      </PropertyGroup>
      <ItemGroup>
        <Compile Remove="Partial Types Framework Specific\**\*.cs" />
        <Content Include="Partial Types Framework Specific\**\*.cs" />
        <Compile Remove="Full Types Framework Specific\**\*.cs" />
        <Content Include="Full Types Framework Specific\**\*.cs" />
      </ItemGroup>
      <ItemGroup Condition="$(FrameworkLackingExtensionMethods.Contains('|$(TargetFramework)|'))">
        <Content Remove="Full Types Framework Specific\**\ExtensionAttribute.cs" />
        <Compile Include="Full Types Framework Specific\**\ExtensionAttribute.cs" />
      </ItemGroup>
