# How to Build?

NOTE: a Prebuilt binary exists [here!!!](https://github.com/ange-yaghi/engine-sim/tags)

# Step 1 - Installing Chocolatey, git and Cmake

Run a Powershell.exe **As Administrator** and enter this command to install chocolatey like this:

![adminpshell](https://cdn.discordapp.com/attachments/794975038616895488/1008036672736342086/unknown.png)
```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
![comand](https://cdn.discordapp.com/attachments/794975038616895488/1008037125448548402/unknown.png)

close Powershell and open it Again as Administrator and enter this command:
```
chocolatey install cmake git
```
Answer Y to every question.

![chocolatey](https://cdn.discordapp.com/attachments/794975038616895488/1008037498640928868/unknown.png)

After these are installed, make sure you have Visual Studio installed. **important note: you need to check “Desktop development with C++”!!!**.
When Visual Studio is installed, navigate to `C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\**.**.**\bin\Hostx64\x64`
And copy this location. (the stars are there, because you might have a different version.)

if you found the folder, run this command. ("your_folder" is the complete directory, eg. "C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\14.33.31629\bin\Hostx64\x64")

```
cmd /c setx.exe PATH "%PATH%;your_folder;C:\Program Files\Cmake\Bin"
```
*note: please do this step very carefully.*

# Step 2 - Cloning repository

Open another Powershell and use `cd` to navigate into the desired location (default folder is C:/Users/your_name/)

*How do i use `cd`?*
Use `cd ../` to go back one Folder and use `cd path/to/your_folder/` to go into that Folder. This also works for full paths, i.e. C:/Users/example/Documents/.

To clone the repository ether these commands one by one:
```
git clone --recurse-submodules https://github.com/ange-yaghi/engine-sim
cd engine-sim/
mkdir build/
cd build/
```
![repo](https://cdn.discordapp.com/attachments/794975038616895488/1008038407362064404/unknown.png)

*info: leave the Powershell open for now.*

# Step 3 - Dependencies

First, create a `local` folder in your `C:` drive.

![folder](https://cdn.discordapp.com/attachments/794975038616895488/1008038688393015326/unknown.png)

Then put [these files](https://mega.nz/file/PyIkWDjT#AbKPu2AdwqL56wgsINcUP1Q9I6P2vEcMf1a5_T-lLx4) in the local folder like this:

![deps](https://cdn.discordapp.com/attachments/794975038616895488/1008042014291529789/unknown.png)

Now, copy this chunk of text and paste it into the CMakeLists.txt in the engine-sim folder after `set_property(TARGET gtest_main PROPERTY FOLDER "gtest")`:
```
set(SDL2_INCLUDE_DIR C:/local/SDL2/include)
set(SDL2_LIBRARY C:/local/SDL2/lib/x64/SDL2.lib)
set(SDL2_DIR C:/local/SDL2)
set(SDL2MAIN_LIBRARY C:/local/SDL2/lib/x64/SDL2main.lib)

set(SDL2_IMAGE_INCLUDE_DIR C:/local/SDL2_image/include)
set(SDL2_IMAGE_LIBRARY C:/local/SDL2_image/lib/x64/SDL2_image.lib)

set(Boost_DIR C:/local/Boost_1_80_0)
set(Boost_INCLUDE_DIR C:/local/Boost_1_80_0)
set(Boost_FILESYSTEM_LIBRARY_RELEASE C:/local/Boost_1_80_0/lib64-msvc-14.3/libboost_filesystem-vc143-mt-x64-1_80.lib)
set(Boost_FILESYSTEM_LIBRARY_DEBUG C:/local/Boost_1_80_0/lib64-msvc-14.3/libboost_filesystem-vc143-mt-gd-x64-1_80.lib)
set(Boost_LIBRARY_DIR_DEBUG C:/local/Boost_1_80_0/lib64-msvc-14.3)
set(Boost_LIBRARY_DIR_RELEASE C:/local/Boost_1_80_0/lib64-msvc-14.3)

set(FLEX_EXECUTABLE C:/local/WinFlexBison/win_flex.exe)
set(BISON_EXECUTABLE C:/local/WinFlexBison/win_bison.exe)

include_directories(${Boost_INCLUDE_DIR})
include_directories(${SDL2_INCLUDE_DIR})
include_directories(${SDL2_IMAGE_INCLUDE_DIR})
```

![cmakelist](https://cdn.discordapp.com/attachments/794975038616895488/1008042756083568720/unknown.png)

Also, change `option(PIRANHA_ENABLED "Enable scripting input" OFF)` to `option(PIRANHA_ENABLED "Enable scripting input" ON)`:

![piranha](https://cdn.discordapp.com/attachments/794975038616895488/1008049103986511964/unknown.png)

# Step 4 - Building the Simulator

Return to the Powershell you left open in step 2 and run these commands:
```
cmake ..
cmake --build .
```

![cmake1](https://cdn.discordapp.com/attachments/794975038616895488/1008043402698424401/unknown.png)

Everything should build sucessfully. You can ignore the Yellow warnings while building.

# Step 5 - Starting the Simulator

First, you need to put [These DLL's](https://cdn.discordapp.com/attachments/794975038616895488/1008047097020436500/DLLs.zip) into the `Releases` Folder.
If you don't see This folder, create it. *info: the capital R is important*

![dlls](https://cdn.discordapp.com/attachments/794975038616895488/1008047688429871125/unknown.png)

In the engine-sim folder, open the `build` folder and open the file `engine-sim.sln`.

![buildfolder](https://cdn.discordapp.com/attachments/794975038616895488/1008044394156408922/unknown.png)

![solution](https://cdn.discordapp.com/attachments/794975038616895488/1008044430336467014/unknown.png)

Then you select `Release` in this dropdown menu:

![dropdown](https://cdn.discordapp.com/attachments/794975038616895488/1008045055250014229/unknown.png)

and in the solution explorer, right click `engine-sim-app` and select `Set as Startup project`:

![project](https://cdn.discordapp.com/attachments/794975038616895488/1008045401309462559/unknown.png)

now you can Press `F5` to start the Simulator.
*info: This will not run Standalone without moving files, so you need to start it with VS.*
