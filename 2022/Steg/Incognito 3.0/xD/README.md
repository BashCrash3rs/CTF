# <|-- Incognito 3.0 - xD --|>

For this challenge we were given nothing more than a file named tv_chal.jpg.                 
As there was no hint nor description available, we approached this with some basic recon first...


## <- Some recon to get an idea of what we need to do ->

Before we run the image file through any of our tools, let's first do <code>eog tv_chall.jpg</code> to see if we can view the image before proceeding. Our image viewer throws 
an error, telling us that it does not recognize the image as an actual JPEG. 

![eog error](https://user-images.githubusercontent.com/104336820/165018827-0ee7e2c9-7103-478c-b879-91b24dc34044.png)

Of course, we didn't expect it to be as easy as that but nonetheless, it is always a good idea to run down the list of possible avenues to gain any extra info you can.
                

Next, we run some basic tools to get some more technical information about the image we are working with. We put the file through binwalk first with <code>binwalk -e tv_chal.jpg</code>

![binwalk](https://user-images.githubusercontent.com/104336820/165020194-2a2a0689-4478-4fd9-bf4f-2de5ef113c02.png)

binwalk shows us that there is nothing here to extract. This leads us to believe that maybe we should look at the metadata 
as we now know the flag isn't in a file that we need to extract. The image started life as a TIFF type. Nothing odd here so moving on to try to dig a bit deeper.

We fire up exiftool with <code>exiftool -v tv_chal.jpg</code> to take a look at the metadata of the file. 
Here we find a couple of things of interest...

![exiftool](https://user-images.githubusercontent.com/104336820/165020443-d386a41f-2448-4eb6-9ff2-6ac16eeab562.png)

exiftool gives us a warning that we have an unknown 30-byte header. Then it proceeds to reset the file type as the header configuration is unrecognized which results in our inability to view the image in its current state. All of our recon has led us to assume that the image itself contains the flag and likely it is a matter of a corrupted header. We likely have ourselves a magic numbers issue.


## <- Fixing the header ->

To take a look at the header we use <code>hexeditor tv_chal.jpg</code> and immediately we are able to see that the file signature for a JPEG file is not what we have here for the magic numbers. 

![hexeditor](https://user-images.githubusercontent.com/104336820/165021657-cf2d5924-f090-44e5-bf89-538d467d63f1.png)

Let's fix that!

![header fixed](https://user-images.githubusercontent.com/104336820/165021788-104b16c0-89cf-47d2-83b0-679c987cc082.png)

Now that the magic numbers have been corrected, we check <code>eog tv_chal.jpg</code> again to see if we can view the image now...

![can view](https://user-images.githubusercontent.com/104336820/165021971-5fc73c3a-71a8-49e8-8eeb-bc2ae8cb63bd.png)

Looks like the issue was indeed just a corrupted header! And in the corner of the television in the image, we can see an image of some text. If we zoom in we can retrieve our flag.

![tv flag](https://user-images.githubusercontent.com/104336820/165022223-d28d0fc8-9ca7-40bd-88f4-7e771656f216.png)


