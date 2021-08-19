# Annoyances in Visual Studio 2019/2022

## Directory.Build.props

- VS finds outdated NuGet dependencies in Directory.Build.props - GREAT :)
- When you attempt to update that outdated dependency instead of updating the `Directory.Build.props` file VS adds the new dependency version to all CSPROJ files instead - NOT GREAT :(

[GitHub Issue](https://github.com/NuGet/Home/issues/10850)

## Unable to ignore outdated dependencies

MyLibrary.csproj has multiple target frameworks, a package reference for the netstandard2.0 target is deliberately old due to breaking changes in newer versions;

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFrameworks>netstandard2.0;net5.0;net6.0</TargetFrameworks>
    <Nullable>enable</Nullable>
  </PropertyGroup>

  <ItemGroup Condition="'$(TargetFramework)' == 'netstandard2.0'">
    <PackageReference Include="MessagePack" Version="1.9.11" />
  </ItemGroup>
  <ItemGroup Condition="'$(TargetFramework)' == 'net5.0' OR '$(TargetFramework)' == 'net6.0'">
    <!--breaking changes in MessagePack v2, see https://github.com/neuecc/MessagePack-CSharp/issues/744 -->
    <PackageReference Include="MessagePack" Version="2.3.75" />
    <PackageReference Include="MessagePackAnalyzer" Version="2.3.75" />
  </ItemGroup>

</Project>
```

Every time you use NuGet package explorer in VS it will attempts to update the older dependency which is annoying :(
There is no way to `ignore` or `skip` the older package version... or is there?
