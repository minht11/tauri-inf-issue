# Tauri .inf issue reproduction
I am trying to install OpenVPN Tap driver on Windows.
Required driver files are embedded as additional files https://v2.tauri.app/develop/resources/.

Interestingly when built on on Mac/Linux everything is fine.
When built on Windows .inf file for some reason is different and driver installation fails.
You can reproduces this yourself by building application on Windows and then trying to run driver install command.

When application is installed, go to install directory -> `resources/tap-unzipped/amd64`  and run `"./devcon.exe" install ./OemVista.inf tap0901` command.
- If built on Mac/Linux it will succeed.
- If built on Windows it will fail.

Now unzip `resources/tap.zip` and try running the same command, it should succeed.

Tauri does something to files on Windows when embedding resource files, the `OemVista.inf` file size is slightly different as well. The workaround I found was to use zip file.

> When testing this it is STRONGLY recommended to run installer inside Windows Sandbox because this installs a driver.

### Building

Install [moon](https://moonrepo.dev/moon) and [rustup](https://rustup.rs/) 

```
pnpm install
# Run on Mac/Linux
moon run tauri:build-windows-cross
# Run on Windows
moon run build
```