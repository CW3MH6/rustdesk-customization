# Customizing RustDesk
Some tips for customizing RustDesk for Windows (other platforms might require additional changes)

1. Changing the application name
2. Changing the application icons
3. [Embedding the UI resources](#embedding-ui--enable-inline-builds)
4. [Embedding sciter.dll](#embedding-sciterdll)
5. [Toggling the console window](#hide-console-window)

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

# Embedding Sciter.dll
If you want a single portable executable file, you can either statically link Sciter, or embed the DLL. Statically linking it requires you [license Sciter](https://sciter.com/prices/), which costs money--so I went with embedding the dll.


1. Copy sciter.dll to your project root directory (where Cargo.toml resides)
2. Add the following lines into ```fn main()``` in src/main.rs (around line 23 or so)
    ```
        let bytes = include_bytes!("..\\sciter.dll");
        fs::write("sciter.dll", bytes.as_slice());
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
