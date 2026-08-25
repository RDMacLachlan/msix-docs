---
title: Package manifest schema support in the MSIX Packaging Tool
description: >
  Learn how package manifest schema support differs between the MSIX Packaging Tool
  authoring surface and the Windows deployment platform.
ms.date: 08/25/2026
ms.topic: concept-article
keywords: windows, msix, package manifest, schema, MSIX Packaging Tool
---

# Package manifest schema support in the MSIX Packaging Tool

The [MSIX Packaging Tool](tool-overview.md) helps you create and edit MSIX packages
through a graphical interface and command-line conversion options. The tool doesn't define
which package manifest schema elements Windows supports. It provides an authoring and
editing surface for common packaging scenarios.

The Windows deployment platform reads the package manifest when the MSIX package is
validated, registered, installed, updated, or repaired. Windows support for a manifest
element depends on the package manifest schema and the Windows version that deploys the
package. For schema details and version applicability, use the
[package manifest schema reference](/uwp/schemas/appxpackage/uapmanifestschema/schema-root?context=/windows/msix/render).

<!--
SME question for ADO bug 31356148: Please confirm whether the MSIX Packaging Tool team
has a versioned, Microsoft-published list of package manifest schema elements that the
MSIX Packaging Tool GUI and CLI can author or edit. If such a list exists, confirm the
MSIX Packaging Tool version, Windows version, and whether each item applies to the GUI,
CLI, Package Editor direct-manifest editing, or all surfaces.
-->

## Understand the responsibility boundary

- **Windows deployment platform**: Processes the package manifest for deployment
  operations on the target device. Verify that the target Windows version supports the
  package manifest schema elements and namespaces that the package uses.
- **MSIX Packaging Tool GUI and CLI**: Captures, converts, and authors package data for
  supported tool scenarios. Verify that the tool surface exposes the specific manifest
  fields you want to author during conversion or editing.
- **Package Editor**: Opens an existing MSIX package and lets you edit package
  information, package files, capability declarations, and virtual registry data. If a
  needed manifest field isn't exposed in the UI, use the option to open the manifest and
  edit it directly.

A manifest element can be valid for Windows even when the MSIX Packaging Tool doesn't
provide a dedicated field for it. In that case, the tool is not the authority for whether
the operating system can honor the element.

## Tool-authored and hand-authored manifest scenarios

Use the following guidance when deciding how to author package manifest declarations.

### When the MSIX Packaging Tool authors the package

1. Create or convert the package with the MSIX Packaging Tool.
1. Review the generated `AppxManifest.xml` before signing or distributing the package.
1. If the tool doesn't expose a needed manifest declaration, edit the manifest directly
   only after you confirm the declaration in the package manifest schema reference.
1. Sign the package again after manifest or package content changes.
1. Test deployment on the oldest Windows version that you support.

### When you hand-author the manifest

Keep the hand-authored manifest as the source of truth. You can still open the package in
[Package Editor](package-editor.md), but don't treat the visible fields in the editor as
an exhaustive list of valid package manifest schema elements. Use the schema reference to
confirm the exact element name, namespace, attributes, and Windows version applicability.

## Operational risks

- An invalid manifest can cause packaging, validation, or deployment failures.
- A manifest element that works on one Windows version might not be supported on an
  earlier target Windows version.
- Editing a signed package invalidates the package signature. Sign the package again
  before installation or distribution.
- A tool-authored manifest can change if you repeat conversion with different tool or
  template settings. Reapply and review any manual manifest edits.

## About the requested element list

A [Microsoft Tech Community discussion][tech-community-schema-elements] asked for a
list of package manifest schema elements supported by the MSIX Packaging Tool and by the
operating system. The discussion doesn't provide an authoritative, versioned support
matrix for individual elements. Because of that, this article doesn't reproduce a table
of supported elements.

For individual package manifest schema elements, use the package manifest schema
reference as the authoritative source for the schema. For tool behavior, verify the exact
MSIX Packaging Tool version and the authoring surface that you use: GUI conversion,
command-line conversion, or Package Editor.

## Related content

- [MSIX Packaging Tool overview](tool-overview.md)
- [Edit a package using Package Editor](package-editor.md)
- [Package manifest schema reference](/uwp/schemas/appxpackage/uapmanifestschema/schema-root?context=/windows/msix/render)

[tech-community-schema-elements]: https://techcommunity.microsoft.com/t5/msix-packaging-and-tools/list-of-supported-schema-elements/m-p/1813954
