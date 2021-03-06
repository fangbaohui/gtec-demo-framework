Tutorial: 
---------
The framework supports loading files from the Content folder.

Given a content folder like this:
```
  Content/Texture1.bmp
  Content/Stuff/Readme.txt
```

You can load the files via the `IContentManager` service like this:

```C++
  Bitmap bitmap;
  contentManager->Read(bitmap, "Texture1.bmp", PixelFormat::R8G8B8_UINT);
  const std::string content = contentManager->ReadAllText("Stuff/Readme.txt");
```

If you prefer to control the loading yourself you can retrieve the path to the
files like this:
```C++
  IO::Path contentPath = contentManager->GetContentPath();
  IO::Path texture1Path = IO::Path::Combine(contentPath, "Texture1.bmp");
  IO::Path readmePath = IO::Path::Combine(contentPath, "Stuff/Readme.txt");
```
You can then open the files with any method you prefer.

Both methods works for all supported platforms.


Android details: 
----------------
As long as you utilize one of the methods above to load the resources, you dont
really need to know the following. However if you experience problems it might 
be useful for you to know.
Under android builds we package all content using the Android 'assets' system.
Since the system requires that the asset files are located under it's 'assets'
folder (located at Android/assets in our samples) we utilize a one way folder
synchronization utility called `FslContentSync.py` to ensure that all files and
directories under Content exist inside the asset folder as well. 
The synchronization script is automatically invoked during the android build 
process. To complicate things further the Android assets can not normally be 
accessed via filenames using standard C/C++ methods. Because of this the assets 
are 'unpacked' on target to either the external or internal filesystem which 
allows us to open the files any way we like. Unfortunately this means that there 
will be a slight unpacking delay the first time a sample is executed.

Ubuntu details:
---------------
As long as you utilize one of the methods above to load the resources, you dont
really need to know the following. However if you experience problems it might 
be useful for you to know.
The ubuntu build expects the content folder to be located at 
`<executable directory>/content`.
Since the binary is put in the sample root directory where the content folder 
is located, there should be no problem loading the resources.

Windows details:
----------------
As long as you utilize one of the methods above to load the resources, you dont
really need to know the following. However if you experience problems it might 
be useful for you to know.
The windows build expects the content folder to be located at 
`<current working directory>/content`.
When you launch the sample via the visual studio project the current working 
directory will be equal to the sample root directory where the content folder
is located, so there should be no problem loading the resources.

Yocto details:
--------------
As long as you utilize one of the methods above to load the resources, you dont
really need to know the following. However if you experience problems it might 
be useful for you to know.
The yocto build expects the content folder to be located at 
`<executable directory>/content`. 


FslContentSync.py notes
-----------------------
- Does not copy files that start with a '.' in its file or directory name.
- Does not allow files to contain ".." in its name.
- Due to android asset packer its not recommended to use unicode file names as
  they are unsupported by the android tool at the moment.
