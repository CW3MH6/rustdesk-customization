# Customizing RustDesk
Some tips for customizing RustDesk for Windows (other platforms might require additional changes)

1. [Changing the application name](#changing-the-application-name)
2. [Changing the application icons](#changing-the-application-icons)
3. [Embedding the UI resources](#embedding-ui--enable-inline-builds)
4. [Embedding sciter.dll](#embedding-sciterdll)
5. [Toggling the console window](#hide-console-window)
6. [Hosting your own server](#)
7. [Using a static encryption key](#)

# Changing the application name

1. In ```Cargo.toml``` under the ```[package]``` block, modify the following values

    ```
    [package]
    name = "myapp"  # your exe name
    version = "1.2.0"
    authors = ["rustdesk <info@rustdesk.com>"]
    edition = "2021"
    build= "build.rs"
    description = "A remote control software."
    default-run = "myapp" # should be same as name
    ```
# Changing the application icons

The icons are stored in a number of locations, which change depending on platform

There are a few \*.png, \*.svg, and \*.ico files in ```/rustdesk/res```, you can replace those with your own versions.

There is also a Base64 encoded icon in the source code. Open ```/rustdesk/libs/hbb_common/src/config.rs``` and modify the following lines (starting at line 32)
```
// 128x128
#[cfg(target_os = "macos")] // 128x128 on 160x160 canvas, then shrink to 128, mac looks better with padding
pub const ICON: &str = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIAAAACACAMAAAD04JH5AAAAyVBMVEUAAAAAcf8Acf8Acf8Acv8Acf8Acf8Acf8Acf8AcP8Acf8Ab/8AcP8Acf////8AaP/z+f/o8v/k7v/5/v/T5f8AYP/u9v/X6f+hx/+Kuv95pP8Aef/B1/+TwP9xoP8BdP/g6P+Irv9ZmP8Bgf/E3f98q/9sn/+01f+Es/9nm/9Jif8hhv8off/M4P+syP+avP86iP/c7f+xy/9yqf9Om/9hk/9Rjv+60P99tv9fpf88lv8yjf8Tgf8deP+kvP8BiP8NeP8hkP80gP8oj2VLAAAADXRSTlMA7o7qLvnaxZ1FOxYPjH9HWgAABHJJREFUeNrtm+tW4jAQgBfwuu7MtIUWsOUiCCioIIgLiqvr+z/UHq/LJKVkmwTcc/r9E2nzlU4mSTP9lpGRkZGR8VX5cZjfL+yCEXYL+/nDH//U/Pd8DgyTy39Xbv7oIAcWyB0cqbW/sweW2NtRaj8H1sgpGOwUIAH7Bkd7YJW9dXFwAJY5WNP/cmCZQnJvzIN18on5LwfWySXlxEPYAIcad8D6PdiHDbCfIFCADVBIENiFDbCbIACKPPXrZ+cP8E6/0znvP4EymgIEravIRcTxu8HxNSJ60a8W0AYECKrlAN+YwAthCd9wm1Ug6wKzIn5SgRduXfwkqDasCjx0XFzi9PV6zwNcIuhcWBOg+ikySq8C9UD4dEKWBCoOcspvAuLHTo9sCDQiFPHotRM48j8G5gVur1FdAN2uaYEuiz7xFsgEJ2RUoMUakXuBTHHoGxQYOBhHjeUBAefEnMAowFhaLBOKuOemBBbxLRQrH2PBCgMvNCPQGMeevTb9zLrPxz2Mo+QbEaijzPUcOOHMQZkKGRAIPem39+bypREMPTkQW/oCfk866zAkiIFG4yIKRE/aAnfiSd0WrORY6pFdXQEqi9mvAQm0RIOSnoCcZ8vJoz3diCnjRk+g8VP4/fuQDJ2Lxr6WwG0gXs9aTpDzW0vgDBlVUpixR8gYk44AD8FrUKHr8JQJGgIDnoDqoALxmWPQSi9AVVzm8gKUuEPGr/QCvptwJkbSYT/TC4S8C96DGjTj86aHtAI0x2WaBIq0eSYYpRa4EsdWVVwWu9O0Aj6f6dyBMnwEraeOgSYu0wZlauzA47QCbT7DgAQSE+hZWoEBF/BBmWOewNMK3BsSqKUW4MGcWqCSVmDkbvkXGKQOwg6PAUO9oL3xXhA20yaiCjuwYygRVQlUOTWTCf2SuNJTxeFjgaHByGuAIvd8ItdPLTDhS7IuqEE1YSKVOgbayLhSFQhMzYh8hwfBs1r7c505YVIQYEdNoKwxK06MJiyrpUFHiF0NAfCQUVHoiRclIXJIR6C2fqG37pBHvcWpgwzvAtYwkR5UGV2e42UISdBJETl3mg8ouo54Rcnti1/vaT+iuUQBt500Cgo4U10BeHSkk57FB0JjWkKRMWgLUA0lLodtImAQdaMiiri3+gIAPZQoutHNsgKF1aaDMhMyIdBf8Th+Bh8MTjGWCpl5Wv43tDmnF+IUVMrcZgRoiAxhtrloYizNkZaAnF5leglbNhj0wYCAbCDvGb0mP4nib7O7ZlcYQ2m1gPtIZgVgGNNMeaVAaWR+57TrqgtUnm3sHQ+kYeE6fufUubG1ez50FXbPnWgBlgSABmN3TTcsRl2yWkHRrwbiunvk/W2+Mg1hPZplPDeXRbZzStFH15s1QIVd3UImP5z/bHpeeQLvRJ7XLFUffQIlCvqlXETQbgN9/rlYABGosv+Vi9m2Xs639YLGrZd0br+odetlvdsvbN56abfd4vbCzv9Q3v/ygoOV21A4OPpfXvH4Ai+5ZGRkZGRkbJA/t/I0QMzoMiEAAAAASUVORK5CYII=
";
#[cfg(not(target_os = "macos"))] // 128x128 no padding
pub const ICON: &str = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAIAAAACACAMAAAD04JH5AAAA7VBMVEUAAAAAcf8Acf8Acf8Adf8Acf8Acf8AcP8Acv8AcP8Acf8Acf8Acf8Acv8Acf8Acf8Ab/8AcP8Acf8Acf8Acf/////7/f8Dc/8TfP/1+f/n8v9Hmf/u9v+Uw//Q5f9hp/8Yfv8Qev8Ld/+52P+z1f+s0f81j/8wjP8Hdf/3+/8mh/8fg//x9//h7//H4P9xsP9rrf9oq/8rif/r9P/D3v+92/+Duv9bpP/d7f/U5/9NnP8/lP8jhP/L4v/B3P+OwP9+t/95tf9Rn/8bgf/Z6v+Zx/90sv9lqf85kf+hy/9UoP+Wxf+kzP+dyP+Lvv/H4q8IAAAAFHRSTlMA+u6bB6x5XR4V0+S4i4k5N+a81W8MiAQAAAVcSURBVHjazdvpWtpAGIbhgEutdW3fL2GHsMsiq4KI+66t5384XahF/GbizJAy3j/1Ah5CJhNCxpm1vbryLRrBfxKJrq+sbjtSa5u7WIDdzTVH5PNSBAsSWfrsMJ+iWKDoJ2fW8hIWbGl55vW/YuE2XhUsb8CCr9OCJVix9G//gyWf/o6/KCyJfrbwAfAPYS0CayK/j4mbsGjrV8AXWLTrONuwasdZhVWrzgqsWnG+wap1Jwqrok4EVkUcmKhdVvBaOVnzYEY/oJpMD4mo6ONF/ZSIUsX2FZjQA7xRqUET+y/v2W/Sy59u62DCDMgdJmhqgIk7eqWQBBNWwPhmj147w8QTzTjKVsGEEBBLuzSrhIkivTF8DD/Aa6forQNMHBD/VyXkgHGfuBN5ALln1TADOnESyGCiT8L/1kILqD6Q0BEm9kkofhdSwNUJiV1jQvZ/SnthBNSaJJGZbgGJUnX+gEqCZPpsJ2T2Y/MGVBrE8eOAvCA/X8A4QXLnmEhTgIPqPAG5IQU4fhmkFOT7HAFenwIU8Jd/TUEODQIUtu1eOj/dUD9cknOTpgEDkup3YrOfVStDUomcWcBVisTiNxVw3TPpgCl4RgFFybZ/9iHmn8uS2yYBA8m7qUEu9oOEejH9gHxC+PazCHbcFM8K+gGHJNAs4z2xgnAkVHQDcnG1IzvnCSfvom7AM3EZ9voah4+KXoAvGFJHMSgqEfegF3BBTKoOVfkMMXFfJ8AT7MuXUDeOE9PWCUiKBpKOlmAP1gngH2LChw7vhJgr9YD8Hnt0BxrE27CtHnDJR4AHTX1+KFAP4Ef0LHTxN9HwlAMSbAjmoavKZ8ayakDXYAhwN3wzqgZk2UPvwRjshmeqATeCT09f3mWnEqoBGf4NxAB/moRqADuOtmDiid6KqQVcsQeOYOKW3uqqBRwL5nITj/yrlFpAVrDpTJT5llQLaLMHwshY7UDgvD+VujDC96WWWsBtSAE5FnChFnAeUkDMdAvw88EqTNT5SYXpTlgPaRQM1AIGorkolNnoUS1gJHigCX48SaoF3Asuspg4Mz0U8+FTgIkCG01V09kwBQP8xG5ofD5AXeirkPEJSUlwSVIfP5ykVQNaggvz+k7prTvVgDKF8BnUXP4kqgEe/257E8Ig7EE1gA8g2stBTz7FLxqrB3SIeYaeQ2IG6gE5l2+Cmt5MGOfP4KsGiH8DOYWOoujnDY2ALHF3810goZFOQDVBTFx9Uj7eI6bp6QTgnLjeGGq6KeJuoRUQixN3pDYWyz1Rva8XIL5UPFQZCsmG3gV7R+dieS+Jd3iHLglce7oBuCOhp3zwHLxPQpfQDvBOSKjZqUIml3ZJ6AD6AajFSZJwewWR8ZPsEY26SQDaJOMeZP23w6bTJ6kBjAJQILm9hzqm7otu4G+nhgGxIQUlPLKzL7GhbxqAboMCuN2XXd+lAL0ajAMwclV+FD6jAPEy5ghAlhfwX2FODX445gHKxyN++fs64PUHmDMAbbYN2DlKk2QaScwdgMs4SZxMv4OJJSoIIQBl2Qtk3gk4qiOUANRPJQHB+0A6j5AC4J27QQEZ4eZPAsYBXFk0N/YD7iUrxRBqALxOTzoMC3x8lCFlfkMjuz8iLfk6fzQCQgjg8q3ZEd8RzUVuKelBh96Nzcc3qelL1V+2zfRv1xc56Ino3tpdPT7cd//MspfTrD/7R6p4W4O2qLMObfnyIHvvYcrPtkZjDybW7d/eb32Bg/UlHnYXuXz5CMt8rC90sr7Uy/5iN+vL/ewveLS/5NNKwcbyR1r2a3/h8wdY+v3L2tZC5oUvW2uO1M7qyvp/Xv6/48z4CTxjJEfyjEaMAAAAAElFTkSuQmCC
";
```

[Convert your image to Base64](https://www.base64-image.de/) and then replace the contents of ```ICON``` with your base64 encoded image (it should start with ```data:image/png;base64,``` followed by your image data)


# Embedding UI / Enable Inline Builds
In order to include the applicatin's UI resources in the executable, you will need to enable the ```inline``` feature. This compiles the application resources (*src/ui*) into the executable so you do not have to deploy them yourself.

1. Enable ```inline``` feature by editing ```Cargo.toml```

    Under the ```[features]``` block, find this line

    ```
    default = ["use_dasp"]
    ```

    and change it to read
    
    ```
    default = ["use_dasp", "inline"]
    ```

2. Build the UI resources (requires [Python 3](https://www.python.org/downloads/) to be installed)

    Run ```python res/inline-sciter.py``` every time you want to build the UI resources again (i.e. every time you modify the UI)
    
    If it complains that it cannot find python or the command is unrecognized, you need to install the latest version of [Python 3](https://www.python.org/downloads/) and add it to your PATH environment variable.
3. Now build and run application via ```cargo run``` or ```cargo build```
# Embedding Sciter.dll
If you want a single portable executable file, you can either statically link Sciter, or embed the DLL. Statically linking it requires you [license Sciter](https://sciter.com/prices/), which costs money--so I went with embedding the dll.


1. Copy sciter.dll to your project root directory (where Cargo.toml resides)
2. Add the following lines into ```fn main()``` in src/main.rs (around line 23 or so)
    ```
        let bytes = std::include_bytes!("..\\sciter.dll");
        std::fs::write("sciter.dll", bytes.as_slice());
    ```
    Note: Do not remove the "..\\" as main.rs resides in */rustdesk/src*, and will not be able to find the file otherwise. Alternatively you can put ```sciter.dll``` in */rustdesk/src*
    
    Also please make sure you are putting this in the ```fn main()``` that is targeting Windows, where it specifies
    ```
    #[cfg(not(any(target_os = "android", target_os = "ios", feature = "cli")))]
    ```
    as there are other ```fn main()``` that target OTHER platforms.

    The function should now look like so:
    ```
    #[cfg(not(any(target_os = "android", target_os = "ios", feature = "cli")))]
    fn main() {

        //BEGIN CHANGES
        //Embed the Sciter.dll file into the exe, and then write it to disk when application starts
        println!("================ LOADING SCITER DLLL ==================");
        let bytes = std::include_bytes!("..\\sciter.dll"); //since main.rs is in rustdesk/src, we need to go up one level (to rustdesk)
        std::fs::write("sciter.dll", bytes.as_slice());
        //END CHANGES

        if !common::global_init() {
            return;
        }
        
        if let Some(args) = crate::core_main::core_main().as_mut() {
            ui::start(args);
        }
        common::global_clean();
    }
    ```

# Hide Console Window
You can toggle the console terminal window by uncommenting line 3 in ```src/main.rs``` 

```#![windows_subsystem = "windows"]```

Comment it out to show console window, uncomment it to hide.
