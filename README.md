# TestRebuildError

## Summary

This sample project demonstrates that calling the `Rebuild` target might fail with a shared output directory.

## Steps

1. dotnet build
2. dotnet msbuild -t:rebuild -m:1 

 > `msbuild` command is used to disable parallel compilation, so the projects are built one after the other to easily reproduce this issue.

## Analyzation

The `Clean` target of the test project will also clear the `*.deps.json` of the referenced project,
which then leads to the issue that the test project can no longer copy the `*.deps.json` file when the `CopyFilesToOutputDirectory` target is invoked to copy transtively referenced items.

## Workaround

Setting `UseCommonOutputDirectory` to `true` will prevent any copying and cleaning of transitively referenced items and might be the recommended solution here.
