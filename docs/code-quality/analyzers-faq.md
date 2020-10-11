---
title: EditorConfig versus analyzers
ms.date: 03/11/2019
ms.topic: conceptual
helpviewer_keywords:
- analyzers, faq
author: mikejo5000
ms.author: mikejo
manager: jillfra
ms.workload:
- multiple
---
# Code analysis FAQ

This page contains answers to some frequently asked questions about .NET Compiler Platform-based code analysis in Visual Studio.

## Code analysis versus EditorConfig

**Q**: Should I use code analysis or EditorConfig for checking code style?

**A**: Code analysis and EditorConfig files work hand-in-hand. When you define code styles [in an EditorConfig file](/dotnet/fundamentals/code-analysis/code-style-rule-options) or on the [text editor Options](../ide/code-styles-and-code-cleanup.md) page, you're actually configuring the code analyzers that are built into Visual Studio. EditorConfig files can be used to enable or disable analyzer rules, and also to configure NuGet analyzer packages.

## EditorConfig versus rule sets

**Q**: Should I configure my analyzers using a rule set or an EditorConfig file?

**A**: Rule sets and EditorConfig files can coexist and can both be used to configure analyzers. Both EditorConfig files and rule sets let you enable and disable rules and set their severity.

However, EditorConfig files offer additional ways to configure rules too:

- For the .NET code-quality analyzers, EditorConfig files let you [define which types of code to analyze](/dotnet/fundamentals/code-analysis/code-quality-rule-options).
- For the .NET code-style analyzers that are built into Visual Studio, EditorConfig files let you [define the preferred code styles](/dotnet/fundamentals/code-analysis/code-style-rule-options) for a codebase.

In addition to rule sets and EditorConfig files, some analyzers are configured through the use of text files marked as [additional files](../ide/build-actions.md#build-action-values) for the C# and VB compilers.

> [!NOTE]
> - EditorConfig files can only be used to enable rules and set their severity in Visual Studio 2019 version 16.3 and later.
> - EditorConfig files cannot be used to configure legacy analysis, whereas rule sets can.

## Code analysis in CI builds

**Q**: Does .NET Compiler Platform-based code analysis work in continuous integration (CI) builds?

**A**: Yes. For analyzers that are installed from a NuGet package, those rules are [enforced at build time](roslyn-analyzers-overview.md#build-errors), including during a CI build. Analyzers used in CI builds respect rule configuration from both rule sets and EditorConfig files. Currently, the code analyzers that are built into Visual Studio are not available as a NuGet package, and so these rules are not enforceable in a CI build.

## IDE analyzers versus StyleCop

**Q**: What's the difference between the Visual Studio IDE code analyzers and StyleCop analyzers?

**A**: The Visual Studio IDE includes built-in analyzers that look for both code style and quality issues. These rules help you use new language features as they're introduced and improve the maintainability of your code. IDE analyzers are continually updated with each Visual Studio release.

[StyleCop analyzers](https://github.com/DotNetAnalyzers/StyleCopAnalyzers) are third-party analyzers installed as a NuGet package that check for style consistency in your code. In general, StyleCop rules let you set personal preferences for a code base without recommending one style over another.

## Code analyzers versus legacy analysis

**Q**: What's the difference between legacy analysis and .NET Compiler Platform-based code analysis?

**A**: .NET Compiler Platform-based code analysis analyzes source code in real time and during compilation, whereas legacy analysis analyzes binary files after build has completed. For more information, see [.NET Compiler Platform-based analysis versus legacy analysis](../code-quality/fxcop-analyzers-faq.md#whats-the-difference-between-legacy-fxcop-and-fxcop-analyzers).

## Treat warnings as errors

**Q**: My project uses the build option to treat warnings as errors. After migrating from legacy analysis to source code analysis, all of the code analysis warnings are now appearing as errors. How can I prevent that?

**A**: To prevent code analysis warnings from being treated as errors, follow these steps:

  1. Create a .props file with the following content:

     ```xml
     <Project>
        <PropertyGroup>
           <CodeAnalysisTreatWarningsAsErrors>false</CodeAnalysisTreatWarningsAsErrors>
        </PropertyGroup>
     </Project>
     ```

  2. Add a line to your .csproj or .vbproj project file to import the .props file you created in the previous step. This line must be placed before any lines that import the FxCop analyzer .props files. For example, if your .props file is named codeanalysis.props:

     ```xml
     ...
     <Import Project="..\..\codeanalysis.props" Condition="Exists('..\..\codeanalysis.props')" />
     <Import Project="..\packages\Microsoft.CodeAnalysis.FxCopAnalyzers.2.6.5\build\Microsoft.CodeAnalysis.FxCopAnalyzers.props" Condition="Exists('..\packages\Microsoft.CodeAnalysis.FxCopAnalyzers.2.6.5\build\Microsoft.CodeAnalysis.FxCopAnalyzers.props')" />
     ...
     ```

## Code analysis solution property page

**Q**: Where is the Code Analysis property page for the solution?

**A**: The Code Analysis property page at the solution level was removed in favor of the more reliable shared property group. For managing Code Analysis at the project level, the Code Analysis property page is still available. (For managed projects, we also recommend migrating from rulesets to EditorConfig for rule configuration.)  For sharing rulesets across multiple/all projects in a solution or a repo, we recommend defining a property group with CodeAnalysisRuleSet property in a shared props/targets file or Directory.props/Directory.targets file. If you don't have any such common props or targets that all your projects import, you should consider [adding such a property group to a Directory.props or a Directory.targets at a top level solution directory, which is automatically imported in all project files defined in the directory or its sub-directories](../msbuild/customize-your-build.md).

## See also

- [Analyzers overview](roslyn-analyzers-overview.md)
- [.NET coding convention settings for EditorConfig](/dotnet/fundamentals/code-analysis/code-style-rule-options)