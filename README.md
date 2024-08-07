# makepad_image_manipulation

This is a simple application used for benchmarking image rendering in Makepad, it consists of a grid of animated images.

The images either rotate, scale or fade out:

<https://github.com/project-robius/makepad_image_manipulation/assets/22042418/fc0dd5d1-ce2d-45a1-a8a5-05f1b13a9079>

You can find the benchmark results and how to run your own measurements in [this document](https://www.notion.so/wyewrks/Benchmark-Image-rendering-performance-under-Upscaling-f3ffdd6c75ef4748969afa037397bd0a)

We also compared Makepad's results against Flutter and Native. You can find their implementations under [/benchmark_comparisons](benchmark_comparisons)

## Build Instructions

## 1. Setup Makepad

### Clone the Makepad repository

```bash
cd ~
git clone git@github.com:makepad/makepad.git
```

### Change to latest 'rik' branch

```bash
git branch rik
```

### Install makepad subcommand for cargo

```bash
cargo install --path ~/makepad/tools/cargo_makepad
```

## 2. Get Project

### Clone the makepad_image_manipulation repo

```bash
git clone https://github.com/project-robius/makepad_image_manipulation
```

## 3. Android Build

### Install Android toolchain (First time)

```bash
rustup toolchain install nightly
cargo makepad android install-toolchain
```

### Install app on Android devivce or Android emulator

Open either the Android emulator or connect to a real Android device
use `adb` command to make sure there's a device connected properly

```bash
cd ~/makepad_image_manipulation
cargo makepad android run -p makepad_image_manipulation --release
```

## 4. iOS Setup & Install

### Install IOS toolchain (First time)

```bash
rustup toolchain install nightly
cargo makepad apple ios install-toolchain
```

### Install app on Apple devivce or iOS simulator

### iOS Setup

For iOS, the process is slightly more complicated. The steps involved are:

1. Enable your iPhone's Developer Mode, please see instructions here: [Enable Developer Mode](https://www.delasign.com/blog/how-to-turn-on-developer-mode-on-an-iphone/)
1. Setup an Apple Developer account
1. Setup an empty skeleton project in XCode
    1. File -> New -> Project to create a new "App"
    1. Set the Product Name as **`animation`**  (used in --org later)
    1. Set the Organization Identifier to a value of your choice, for this example we will use **`rs.robius`**. (used in --app later)
    1. Setup the Project Signing & Capabilities to select the proper team account
1. In XCode, Build/Run this project to install and run the app on the simulator and device
1. Once the simulator and device has the "skeleton" app installed and running properly, then it is ready for Makepad to install its application.

### Makepad Install

We will run the `cargo makepad apple ios` command, similar to Android build above, but there are some 2 to 6 additional parameters that need to be filled in:

`--org`

First few parts of the organization identifier (which makes up the Bundle Identifier). Usually in the form of **com.somecompany** or **org.someorg**
This is the same value used to setup the initial skeleton app above. For this example:
> `rs.robius`

`--app`

The name of the application or the project. This is the same as the Product Name used to setup the initial skeleton app above. In this case:
> `animation`

### Install app on IOS simulator

```bash
cd ~/makepad_image_manipulation
cargo makepad apple ios \
  --org=rs.robius \
  --app=animation \
  run-sim -p makepad_image_manipulation --release
```

### Install app on IOS device

First run the following command:

```bash
cargo makepad apple list
```

This command will print out the list of all provisioning profiles, signing identities, and device identifiers on the current system. The user has to decide and choose the ones that he/she needs to use for each type. (If you get an error from the command, please follow the iOS Setup instructions above first.)

Once decided, run the folloiwng command and fill in the **unique starting characters** chosen from the output.

```bash
cd ~/makepad_image_manipulation
cargo makepad apple ios \
  --profile=unique-starting-hex-string \
  --cert=UNIQUE_STARTING_HEX_STRING \
  --device=UNIQUE-STARTING-HEX-STRING \
  --org=rs.robius \
  --app=makepad_image_manipulation \
  run-device -p makepad_image_manipulation –release
```

## 5. WASM Build

Running the Makepad application as a WASM build is as simple as a single command. The script will automatically generate the necessary index.html and other files and also start a local webserver at port 8010.

### Demo

<https://wasm.robius.rs/makepad_image_manipulation>

### Install WASM toolchain (First time)

```bash
cargo makepad wasm install-toolchain
```

### Install app as WASM binary for browsers

```bash
cargo makepad wasm run -p makepad_image_manipulation --release
```

After running the command below, just open your browser to <http://127.0.0.1:8010/> in order for the app to load and run.

## 6. MacOS / PC

Although it is a mobile app, Makepad cross-platform means you may run it on desktops if you wish.

```bash
cd ~/makepad_image_manipulation
cargo run
```

or

```bash
cd ~/makepad_image_manipulation
cargo run -p makepad_image_manipulation --release
```

And there should be a desktop application window now running (may need to click on the icon on MacOS's Dock to show it)
