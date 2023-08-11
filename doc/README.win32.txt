
These are raw notes to myself.  Probably not useful for others, just
ignore.  KEGS will support Windows again soon, but not yet.

----

Open Visual Studio 2022 (community edition).
On the right side, "Get Started" list, select the small link underneath
	that says "Continue without code ->".

In this new VS2022 window, select File->New->Project from Existing Code.
Select Visual C++.

Project name: kegswin
Project file location: Select \win10\kegs.1.16\src

Select Finish

On the left hand side, the main window has Solution explorer.  Open
Source Files.  Right click on "macsnd_driver.c" and "xdriver.c" to
select "Exclude from Project"

Then, we need to add 3 DLLs.  In the left view area, select "kegswin", and
then Do Project->Properties.  You should see a dialog "kegswin Property
Pages".  If it says anything else, and if the line underneath says stuff
like "Configuration: N/A" then change view (Solution Explorer is good), and
make sure you select kegswin.

Set Configuration: Release, Platform: x64.
Linker->Input, then on the right, the first item "Additional Dependencies:"
add: "wsock32.lib;dsound.lib;winmm.lib;".  I added it near the end, before
odbccp32.lib.

Set Platform: x86, and add the same libraries for Win32.
To fix "unresolved external symbol _WinMain@16 referenced in function",


To compile:

mkkegswinmac -j 20
cd obj
start ./kegswin.vcxproj

Then Build->Build kegswin (Ctrl-b)

