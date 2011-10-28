---
layout: page
title: "Adding themes to mephisto"
date: 2008-03-27 00:39
comments: true
sharing: true
footer: true
---
Before I got to writing my setup, I wanted to theme my blog.  So, I went hunting for themes.  There seems to be only one good repository of themes at the [Mephisto Themes Gallery](http://mephisto-themes.nanorails.com/gallery), and it doesn't really have that many themes.  Still, I found one I liked.

However, I didn't find anywhere how to install the themes.  

*UPDATE* You can install themes from the Mephisto Design area.  It does seem to be easier if you have a zip file.  Skip down if you want to manually add a theme to Mephisto.
<!--more--> 
### Installing using the Mephisto admin area 

First, go to the Design section within the admin area.  There is a button at the top called "Manage Themes".  This will show you the currently installed themes.   Click the "Import new Theme" button to add another theme.  From there you can upload a zip file of a theme that you got from the [Mephisto Themes Gallery](http://mephisto-themes.nanorails.com/gallery) or you can click on the link in the text that takes you to the [Custom Templates](http://mephisto.stikipad.com/help/show/Custom+Templates) area of the Mephisto wiki.  There are also a couple more links there that will take you to other repositories of Mephisto themes.  Still, almost all of them are available at the Themes Gallery, and you get a preview there also, which is enormously helpful.

Download a zip file and import it into Mephisto by using the file input box.


### How to install a theme into Mephisto manually

I decided that I liked the sharp000 theme (UPDATE: I have subsequently decided it was too dark, and I went with "Beautiful Day" I'd link it, but it's hard to deep link into the Themes Gallery).  Unfortunately, it's hard to get a direct link to the theme, so I have to download it to my desktop, then upload it to my rails server.  Bummer.  Otherwise I would just wget the url to the server directly.

Once you have the theme, navigate to the `themes/site-1` directory in your mephisto install. This is where all of the themes live.  It doesn't appear that the Gallery zips the folder with the files, so create a directory to hold the theme, I think the name is important, so make it the same as the theme, if you can.  In my case, I created a `sharp000` directory, then unzipped the files into there.  

Inside the theme directory there should now be something that looks like this:

```
mongrel@pol:/var/rails/mephisto/themes/site-1/sharp000$ ls
about.yml  images  javascripts  layouts  preview.png  stylesheets  templates
```

Then you open up your Mephisto admin and in the settings page, change your theme to the name of the directory for your theme.  In my case it was "sharp000".  I also had to delete the cache files (in the settings area also, click the 'caches' button) in order for the theme to show up.

Oh, and don't forget to update the templates, mine had the template author's personal stuff all over it. :)
