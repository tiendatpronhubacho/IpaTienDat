# Codemagic setup — TIENDAT on 3106

## Workflow

The repository root contains `codemagic.yaml`. The workflow is `tiendat-ios` and builds the shared Xcode scheme `3106` from `ThreeOneOSSix.xcodeproj` in Release configuration.

## Required Codemagic configuration

1. Connect this repository to Codemagic and select `codemagic.yaml` workflow.
2. In Codemagic Team settings, add an Apple Distribution signing certificate and an In-House (Enterprise) provisioning profile for bundle ID `com.qingtang.3106` (or configure equivalent signing assets for the chosen distribution method).
3. The provisioning profile must include the app-group entitlement required by `ThreeOneOSSix/ThreeOneOSSix.entitlements`, currently `group.com.qingtang.3106`. If your signing team uses a different App Group, update the entitlement and provisioning profile together before building.
4. Start the `tiendat-ios` workflow. The IPA is collected from `build/ios/ipa/*.ipa`.

## Notes

- This workflow uses Enterprise (In-House) distribution. The exported IPA is intended only for internal distribution within the organization under Apple's Enterprise Program; it does not require device UDIDs.
- Codemagic cannot grant arbitrary App Groups. The selected provisioning profile must authorize the exact App Group entitlement in the app.
- This file configures CI; it does not itself provide certificates, provisioning profiles, Apple account access, or guarantee a successful signed build.
