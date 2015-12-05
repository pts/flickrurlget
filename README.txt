flickrurlget: Flickr photo downloader from command-line in batch

flickrurlget is a command-line tool written in Python 2.x that can be used
to download photos from Flickr in batch. flickrurlget itself doesn't
download photos, but it generates a list of raw photo URLs which can be
downloaded with a download manager (even with `wget -i').

Features:

* Can get highest-resolution (if possible, original) image URLs in:
** a Flickr photo
** a photostream of a Flickr user (i.e. all photos of the user)
** the favorite photos of a Flickr user
** an album (photoset) of a Flickr user
** a Flickr group
** a gallery of a Flickr user
* Batch operation: started from the command-line with a bunch of Flickr page
  URLs, and it discovers all the photos in there without any interaction.
* Can get anybody's photos (not only your own).
* Can get non-public and adult (non-safe) photos as well (after loggig in).
* Doesn't need a GUI or a web browser for most of the operations.

Requirements:

* A Unix systems (such as Linux or Mac OS X). Windows is not supported yet.
* Python 2.7, 2.6, 2.5 or 2.4 with the standard ssl module. Earlier Python 2.x
  is not supported yet. Python 3 is not supported.
* A Flickr API key and maybe API secret (can be obtained online, see below).
* Optional: To download the actual photos, a download manager (something as
  simple as wget will do).
* Optional: To get non-public and/or adult (non-safe) photos, a Flickr account.

Installation:

1. Obtain your flickr API key (and maybe API secret).
   The official way is to obtain them online:
   https://www.flickr.com/services/api/

   The API key is a 32-character long hex string, and the corresponding API
   secret (not needed) is a 16-character long hex string.

   For read-only access to public and safe (non-adult) photos on Flickr,
   all you need as an API key. For read-write access or for getting non-public
   and/or adult (non-safe) photos from Flicker, you need both an API key and
   an API secret.

   If the official way is too cumbersome for you, there is an API key in
   http://stackoverflow.com/a/28200449/97248

   There is an API key and and API secret in the file
   https://upsilon.cc/~zack/blog/posts/2006/11/flickr_download/flickr_download.rb

   Save the API key to the file ~/.flickr_download in the format (don't forget
   to change the command to include your API key):

     $ echo api_key: 32_CHARACTER_API_KEY_OBTAINED___ >~/.flickr_download
     $ chmod 600 ~/.flickr_download

   Here is how to append the API secret to the config file (don't forget to
   change the command to include your API secret):

     $ echo api_secret: 16_CHAR_SECRET__ >>~/.flickr_download

   If you don't have an API secret, you may want to append the dummy value
   to the config file, so other tools (such as flickr_download) would also
   work:

     $ echo api_secret: dummy >>~/.flickr_download

2. Install Python 2.7, 2.6, 2.5 or 2.4.

   After installation it should work using the commands `python2.7',
   `python2.6', `python2.5', `python2.4' or `python'. To double check, run
   this command, and check that it prints True in the end:

     $ python -c 'vi = "%s.%s" % __import__("sys").version_info[:2]; print("%s %s" % (vi, vi in ("2.7", "2.6", "2.5", "2.4")))'
     2.7 True

3. Download the flickrurlget script and install it somewhere on your $PATH.
   If you don't know how to install, here is a typical way:

     $ sudo chown root.root flickrurlget
     $ sudo mv flickrurlget /usr/local/bin/

   Check it by checking that this command displays a short help:

     $ flickrurlget --help
     flickrurlget: Flickr photo downloader from command-line in batch
     ...

4. If you want to download the images, you need a download manager. For
   simplicity, you can use wget. Install it and check that it works:

     $ wget --help
     GNU Wget 1.15, a non-interactive network retriever.

5. If you want to get non-public and/or adult (non-safe) photos, you need a
   Flickr account. Register on Flickr by clicking on ``Sign Up'' on
   http://www.flickr.com/ . You can also use your existing Yahoo account to
   sign in.

   To make flickrurlget log in on behalf of you, you need to log in in your
   browser on http://www.flickr.com/ first, and then set up the OAuth tokens
   (credentials) in the file ~/.flickr_token to be used by flickrurlget. To
   do so, run the flickrapilogin tool, which can be downloaded from the same
   source as flickrurlget. Just run it without arguments:

     $ flickrapilogin

   If you haven't installed it, you may need the ./ prefix to run it:

     $ ./flickrapilogin

   Follow the interactive instructions displayed by the program.
   It will tell you what to do in your web browsers, and if you follow the
   instructions, it will succeed and create the file ~/.flickr_token, with
   hex keys, something like this, _without_ a trailing newline:

     aaaaaaaaaaaaaaa42-bbbbbbbbbbbbbb43
     cccccccccccccc44

   Alternatively, it's also possible to log in using the flickr_download
   tool instead. If you have it installed, run it like this:

     $ flickr_download -t

   Logging in like this needs to be done only once in a lifetime, it also
   can be done on a different computer with a web browser, and the
   ~/.flickr_token file can be copied over.

   There is no point logging in multiple times, because the contents of
   ~/.flickr_token will be the same after re-login.

How to use flickrurlget:

1. Browse and use Flickr in your web browser. If you find something
   interesting, keep it open in a browser tab.

2. Copy-paste one or more interesting URLs to the command-line, and run
   flickrurlget on it. For example:

     $ flickrurlget https://www.flickr.com/photos/108161225@N04/galleries/72157656641788568 https://www.flickr.com/groups/cat_motivation https://www.flickr.com/photos/parismadrid
     info: logging in to Flickr
     https://farm3.staticflickr.com/2929/14614300620_8c165ca47b_b.jpg
     https://farm9.staticflickr.com/8852/17909424246_4e0e78e5db_b.jpg
     https://farm9.staticflickr.com/8750/16386660144_a6c4026657_b.jpg
     ...

   If it takes very long, you may want to run it in GNU Screen.

3. If you want to download those images, you can do it with wget. Example:

     $ flickrurlget https://www.flickr.com/groups/cat_motivation >urls.lst
     info: logging in to Flickr
     $ wget -i urls.lst
     ...

   If it takes very long, you may want to run it in GNU Screen.

Why is flickrurlget better than https://github.com/beaufour/flickr-download ?

* Takes Flickr page URLs and figures out all parameters automatically.
* Can download groups and galleries (in addition to user photostreams and
  photosets) as well.
* Has user ID, group ID and photoset ID discovery: the user doesn't
  have to do manual ID lookups before running the tool.
* Uses only standard Python modules, no other dependencies.
* Better compatibility with old Python: works with 2.7, 2.6, 2.5 and 2.4;
  not only 2.7.

__EOF__
