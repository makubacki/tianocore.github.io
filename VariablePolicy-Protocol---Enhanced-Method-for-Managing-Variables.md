# VariablePolicy Protocol - Enhanced Method for Managing Variables

VariablePolicy is -- conceptually -- a follow-on and replacement for the EdkIIVariableLockProtocol and VariableProperties. It expands upon the capabilities while maintaining a similar interface. The full interface is described in `MdeModulePkg/Include/Protocol/VariablePolicy.h`, but the interface and policy structure don't necessarily give a full picture of the uses of policies, especially some of the more complicated policy constructions. This article aims to describe some possible VariablePolicy-based solutions. It also describes the required changes for a platform adopting VariablePolicy for the first time.

## Policy Layering

## Policy Examples

## Platform Porting

If you're picking up the commits containing VariablePolicy for the first time, you will need to make a few changes to your platform DSC files. All of the changes are library-based and linked against VariableServices. The aim was that no matter what flavor of VariableServices you're consuming, you should only have to make changes to the DSC (or even a shared DSC include) and not to any platform FDFs.

### VariablePolicyLib Instance

VariablePolicyLib contains the business logic for the VariablePolicy engine, and is now expected by VariableServices. There are two flavors of VariablePolicyLib: `VariablePolicyLib` and `VariablePolicyLibRuntimeDxe`. You **must** add one of the following LibraryClasses instances to your DSC file, respective of which flavor of VariableServices you use.

```text
## For SMM-, MM-, or Standalone MM-based VariableServices
[LibraryClasses]
  VariablePolicyLib|MdeModulePkg/Library/VariablePolicyLib/VariablePolicyLib.inf

## For RuntimeDxe-based VariableServices.
[LibraryClasses.common.DXE_RUNTIME_DRIVER]
  VariablePolicyLib|MdeModulePkg/Library/VariablePolicyLib/VariablePolicyLibRuntimeDxe.inf
```

### VariablePolicyHelperLib Instance

VariablePolicyHelperLib contains common helper functions that are a shorthand for common behaviors when creating and registering a new VariablePolicy (ex. packing the policy structure with the variable names). This library is expected by some of the MdeModulePkg changes that were made to replace VariableLockPolicy. It is also just handy to have available for any parts of the platform code that would like to take advantage of VariablePolicy. You **must** add the following LibraryClasses instance to your DSC file.

```text
## For all instances.
[LibraryClasses]
  VariablePolicyHelperLib|MdeModulePkg/Library/VariablePolicyHelperLib/VariablePolicyHelperLib.inf
```

### VarCheckPolicyLib NULL Instance

If a platform is using the SMM-, MM-, or Standalone MM-based VariableServices, there is a VarCheck lib instance that **must** be linked to enable VariablePolicy engine. This VarCheck instance also handles MM communication/handler registration.

```text
MdeModulePkg/Universal/Variable/RuntimeDxe/VariableSmm.inf {
  <LibraryClasses>
    ## Add this instance to enable VariablePolicy.
    NULL|MdeModulePkg/Library/VarCheckPolicyLib/VarCheckPolicyLib.inf
}
```

### PcdAllowVariablePolicyEnforcementDisable

This PCD allows a platform to determine whether the VariablePolicy engine can be disabled. An example of why a platform would want to disable the engine: it would allow a platform-specific debug mode or refurbishement mode to delete even protected variables while removing user data. In keeping with EDK2's goal of "default to maximum security", the default for this PCD is `FALSE`, meaning any calls to VariablePolicy->DisableVariablePolicy() will fail with `EFI_WRITE_PROTECTED`.

If a platform wishes to enable the more flexible mode, they should change this PCD to `TRUE`.

```text
[PcdsFixedAtBuild]
  gEfiMdeModulePkgTokenSpaceGuid.PcdAllowVariablePolicyEnforcementDisable|TRUE
```
