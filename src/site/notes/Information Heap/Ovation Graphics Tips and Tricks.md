---
{"dg-publish":true,"permalink":"/information-heap/ovation-graphics-tips-and-tricks/","noteIcon":"","created":"2025-09-23T13:42:40.513-05:00"}
---

Date: [[-Daily Activity Log-/2025 09-September 23\|2025 09-September 23]]

In order to use images in graphics, they need to be bitmap (.bmp) files. If the image you want to use is a png or a jpeg, you can convert it using Paint, which is on the Drop.
They need to be in a particular folder- in the case of DROP 212, it is C:\Ovation\MMI\graphics\cstfiles.
Once the new bmp files are in the csfiles folder, you need to import the new bmp files using Developer Studio. To do this, right click on `Graphics` and choose Import. In the support files section, you can filter for just .bmp files, in the first folder browser.
Once you have done this, the new image will be available for usage in Graphics Builder.  However, if the graphic you are editing has been open, you'll have to close it and relaunch the graphic for the new bmp to become available.

Yes, this entire process is silly. Yes, we are judging Emerson for it.

Graphics Builder is accessed by double-clicking on a graphics in Developer Studio.

The export file will not compile if there are errors. Errors can be easier to manage using the GBNT text editor. You can ensure compilation by running "where used" on the graphic and then ensuring that the list of "Find all references made by" is not empty.

Whichever drop you are working on, you must then download (even within the same machine), so that the graphic is actually consumable.


