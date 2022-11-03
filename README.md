# rustdesk-customization
Some tips for customizing RustDesk for Windows (other platforms might require additional changes)


# Embedding Sciter.dll
If you want a single portable executable file, you can either statically link Sciter, or embed the DLL. Statically linking it requires you [license Sciter](https://sciter.com/prices/), which costs money--so I went with embedding the dll.


1. Copy sciter.dll to your project root directory (where Cargo.toml resides)
2. Add the following lines into ```fn main()``` in src/main.rs
```
    let bytes = include_bytes!("..\\sciter.dll");
    fs::write("sciter.dll", bytes.as_slice());
```

The function should now look like so:
```
#[cfg(not(any(target_os = "android", target_os = "ios", feature = "cli")))]
fn main() {
    
    //BEGIN CHANGES
    //Embed the Sciter.dll file into the exe, and then write it to disk when application starts
    println!("================ LOADING SCITER DLLL ==================");
    let bytes = include_bytes!("..\\sciter.dll");
    fs::write("sciter.dll", bytes.as_slice());
    //END CHANGES
    
    if !common::global_init() {
        return;
    }

    //println!("Key: {}", hbb_common::config::RS_PUB_KEY);
    //println!("Password: {}", hbb_common::config::RS_PASS);
    //println!("Salt: {}", hbb_common::config::RS_SALT);
    if let Some(args) = crate::core_main::core_main().as_mut() {
        ui::start(args);
    }
    common::global_clean();
}
```
