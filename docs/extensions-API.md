# Extension API
----------------

## IDE
These APIs are used to interact with the IDE and retrieve data.

 - ### General
- ide.isConnected _(Boolean)_
- ide.isBusy _(Boolean)_
  > _Used to check if, message have been returned from the phone to browser_
- ide.getTabLength()
   > _Returns the number of opened tabs_
- ide.unpackedList _(array)_
   > _Returns list of extensions that haven't been built_
- ide.unpackedPath
   > _Returns unpacked path i.e "__UNPACKED__/"
- ide.projectList  _(array)_
   > _Returns all the files & folders in your project directory_
- ide.setOutput(outputName)
  > _e.g ide.setOutput("Console")_
- ide.showOutput()
- ide.hideOutput()
- ide.log(msg,type)
  > _type => warn/error/success/info_
  ```js
    ide.log("log to mobile console","warn")
  ```
- ide.getColorScheme()
  > _Is current theme light/dark mode_

* ### Statusbar
- ide.addToStatusBar(id?string, component?object) 
    ```js
     ide.addToStatusBar("MCS_UpdateBrowser", {
            view:()=>{
                return m(".ml-2.badge.badge-warning", 
                        m("span.text-xs.z-30",[
                            m("i.fab.pr-2.fa-chrome"), 
                            "Update your browser"
                        ])
                    )
            }
        })
    ```
- ide.clearStatusbar(id?string)
 

* ### Saving
    - ide.isSaved _(Boolean)_
    - ide.save() _(Function)_
      > _Saves current file that is being edited_

* ### Extensions(mcx)
    - ide.installExtension(pathToExtension?string) 
    - ide.uninstallExtension(extensionName?string)
    
* ### Extension view
    - ide.onExtensionClose = () =>{}
    - ide.closeExtension()
    - ide.isExtensionVisible(name)<!--Buggy -->
    - ide.getExtensionName()<!--Buggy -->
      > _Returns the name of the active extension_
    - ide.addExtensionOutput({name: OutputName, content: OutputComponent})<!--Change this later-->
      > used for extension, to add custom view to the output panel
      ```js
        ide.addExtensionOutput({
            name:"Sample",
            content:{
              view:()=>{
                return m("small", "Sample content");
              }
            }
        });
       ```
    <!--- ide.lockExtension()-->
    <!--    > prevent switching-->



## Device
These APIs are used to communicate with your device.

* ### Saving
  - device.saveData(key?string, value?any) _(Function)_
    <!---- _todo:filter the input from key & value from charcters like ',' and '+'_-->
  - device.getData(key?string) _(Function)_
     > Return saved data. 
     ```js
        device.getData("AUTOSAVE_DELAY").then((e)=>console.log(e))
     ```


* ### Files & Folders
    - Files
        - device.saveFile(path?string, content?string, replaceExisting? boolean)
        - device.readFile(pathToFile)
        ```js
          device.readFile("hello-world.html").then(e=>console.log(e)).catch(e=>console.error(e));
        ```
        - device.fileExists(pathToFile)
        ```js
            device.fileExists("output/bundle.min.js").then((e)=>console.log(e)).catch(()=>console.error("failed"))
        ```
        - device.renameFile(source, destination)
        ```js
            device.renameFile("old_name.txt","new_name.txt")
        ```
        - device.copyFile(source, destination, replace)
        ```js
            device.copyFile("fldr/text.txt","text.txt").then(e=>console.log(e))
        ```
        - device.deleteFile(pathToFile)
        ```js
            device.deleteFile("fldr/text.txt").then((e)=>console.log(e))
        ```
        - device.replaceInFile(pathToFile, oldText, newText)
        ```js
            device.replaceInFile("text.txt","Hello John","Howdy David").then((e)=>console.log(e))
        ```
        <!--- device.obfuscateFile(source, destination)
        - device.minifyHtml(source, destination)
        - device.minifyCss(source, destination)
        - device.mergeJs(source, destination)
        - device.downloadFile(source, destination)
        ```
          device.downloadFile("http://sgarman.net/downloadable.txt","/").then(e=>console.log(e)).catch(e=>console.error(e));
        ```-->
       

    - Folders
        - device.zipFolder(path,name,ext,msg); 
            > _Compress a folder to a zip file_
            > _using this automatically creates folder_
        - device.listFolder(path)
        ```js
            device.listFolder("/").then((e)=>console.log(e)).catch(()=>console.error("failed"))
        ```
        - device.folderExists(pathToFile)
        ```js
            device.folderExists("output").then((e)=>console.log(e)).catch(()=>console.error("failed"));
        ```
        - device.createFolder(path)
        ```js
            device.createFolder("new_folder").then((e)=>console.log(e)).catch(()=>console.error("failed"));
        ```
        - device.renameFolder(source, destination)
        ```js
            device.renameFolder("old_name","new_name")
        ```
        - device.copyFolder(source, destination, replace)
        ```js
            device.copyFolder("fldr/img","img").then(e=>console.log(e))
        ```
        - device.deleteFolder(pathToFolder)
        ```js
            device.deleteFolder("fldr/").then((e)=>console.log(e))
        ```



## Others
  - include(pathToScript)
    > e.g include("assets/scripts/components.js").then(e=>eval(e)).catch(e=>console.error(e));
    - > _Note: when using include, dont forget to use a variable, to show that the file has loaded_
  - importScript(urls?array or string)
    > _import script(s) on ide load_
    > _e.g importScript([ "lib/require.min.js", "https://code.jquery.com/jquery-3.7.1.min.js" ])_
  - "Layout","CenterLayout","Button","Txt","Input"


    



<!--
//Tips for extension
1) using `const` for extension makes it private(seems for some types, it best to use `let` instead of `const`)

- for split extensions, `main panel` can only access variables only when `var` is used
- `Side panel` can never access a global var from `Main panel`
- Dont allow underscore in extension names
-->

Help for building extensions
----------------------------
- Always remember to use `ide.isConnected`, when building extensions.
<!-- Note that, `device.minifyJs` return boolean not the merged content-->

- It's not advicable to use `device.replaceInFile`, to replace content in heavy files.
 <!-- or probably create a function for that
note that when  git is used, and -->
- When `git.json` is large, it takes a long time before it loads the ide. (Will work on a fix)
- using `assets` folder is advisable for extension with extra files. (mcs uses "assets" fldr internally)

GLOBAL VARIBLES
---------------

- `ide`
- `getIcon`
- `beautifyOptions`
- `device`
- `glob`
- `aceEditor`
 
