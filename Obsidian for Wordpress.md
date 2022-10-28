---

title: Obsidian for Wordpress
menu_order: 1
post_status: publish
post_excerpt: This shows how to Feed from Obsidian to Wordpress.
taxonomy:
    category:
        - Obsidian
        - Computer
    post_tag:
        - Obsidian
        - Wordpress
        - Git

---

# How to use Obsidian to publish to Wordpress

I was looking for a long while for a way to publish content out of Obsidian to wordpress. There is a [Chinese plugin](https://github.com/devbean/obsidian-wordpress) that I wasn't actually able to make work, and I nearly skipped it until I thought, well, what about Git.

As it turns out, while I didn't find a way to directly publish into Wordpress, there is an alternative way:

1. Publish a part of your Obsidian Vault into a Github repository
2. From there, publish the content over to Wordpress. You can configure everything so that this is done automatically whenever you push something into your Github repository.

In other word, there is a [Git plugin for Wordpress](https://www.aakashweb.com/docs/git-it-write/faq/). You can install it just as a Wordpress plugin. It is actually very well documented. You can also look at its [Github Page](https://github.com/vaakash/git-it-write). Essentially, you do this:

## Create a Github Repository

Create a Github repository (it only works with Github) for your Wordpress content. It can be a private repository if you do not want to publish images; if you want to, you cannot make it private: Github will send a payload to Wordpress, including the link to eventual images; the Wordpress plugin will then try to fetch those images, but won't be able to if the repository is private.

Once you have created your repository, go to the User Settings within Github, and from there all the way down on the left side, go to `Developer` settings. Go to `Personal access tokens` and within that, to `Tokens (classic)`. Click on `Generate a new token (classic)`. At this point you're going to be asked your password. After that, essentially in the `Note` field write something that will later remind you of what that token was for. Choose an expiration of that token. You can think of this token as a password that you want your Wordpress page to use to log into your Github repository. Under `Select scopes`, just check the top box `repo` (which will check some boxes underneath). Then press `Generate token` at the bottom, and copy out the token that will be displayed to you. You'll need that for Wordpress.


## Install the Github plugin for Wordpress

Install the Github plugin `Git it Write` for Wordpress and "marry" it to your Git repository:

### Hack the Github plugin

This part of the description may look scary, but bear with me. I wasn't able to get it to work otherwise, and I'm contacting the plugin author to look for a better fix. Essentially, this is only relevant if you want to post images; it may not apply to your case, so try going without this. Anyway, here is what I did.

First of all, when I was adding an image to my post, like so:

```markdown
![Obsidian Git Wordpress](obsidian_git_wordpress.png)
```

This would make the whole process of Github calling the Wordpress plugin fail. With a bit of googling, I found that the plugin was missing some includes.

So I edited the file `wp-content/plugins/git-it-write/includes/publisher.php` and added these three lines just after the line that reads `<?php`:

```php
require_once(ABSPATH . 'wp-admin/includes/media.php');
require_once(ABSPATH . 'wp-admin/includes/file.php');
require_once(ABSPATH . 'wp-admin/includes/image.php');
```

I don't know if all of them are needed; I just added all of them.

At this point, the images would upload, but not show up in the post; the URL would be created incorrectly. To "fix" it (I don't think it's a good fix, but it works for me), I edited the file `wp-content/plugins/git-it-write/includes/parsedown.php`. Looko for something that looks like this (around line 60 for me):

```php
     if( $first_character == '.' ){
		 $prefix = '../';
	 }
```

and change it to read like this:


```php
     if( 1 || $first_character == '.' ){
		 $prefix = '../wp-content/uploads/';
	 }
```

With those changes, the uploads of images would work; well, if I'd delete an image from the Wordpress site, it would not upload again; I'd have to actually rename the image.

### Add a new Repository

At the top of the plugin settings, there's a button to `Add a new repository to publish posts from`. Within those settings, you'll set

- Your Github username
- The name of your Github repository
- The Branch to publish from (default is `master`, but you can change that)
- The Folder within the Github repository to publish from; in the above scenario, leave that blank
- The Post type to publish to: I chose `Posts (Public)`

You can make some more settings, which I didn't need.

### Marry the Repository in Wordpress to your Github Repository

The really nice thing about this setup is that you can configure it in a way that whenever you push something to your Github repository, it will automatically be pushed over to your Wordpress installation.

 So, once you've added the repository, back in the plugin's settings page, you need to do this:

1. Choose some password of your liking as the password that you want Github to use when it connects to your Wordpress installation.
2. Notice that on the right side of that text box, there is a `payload URL` that is shown. Copy that URL. Depending on what kind of Permalink strategy you have configured for Wordpress, this setting will automatically adapt; otherwise saying, if you ever change your Permalink strategy, you need to go back here and copy the then updated setting over to Github.
3. Add your Github username
4. Add the Access token that you created earlier, within Github.

### Marry your Github Repository to your Wordpress

Go back to your Github repository, and click on Settings for the repository. Within that, you'll find `Webhooks`. Go in there and click `Add webhook`. Configure it like this:

1. As `Payload URL`, paste what you had copied out of the Plugin's settings back in Wordpress
2. As Content type, choose `application/json`
3. As Password, enter the password of your liking that you had configured within the Plugin's setting in Wordpress
4. Choose `Just the push event` for the trigger, and
5. Make sure the `Active` checkbox is ticked.
6. Ultimately, click `Add webhook`.


## Clone the Github Repository into your Vault

Clone that (still empty) repository into your vault: Let's say, you'll have a folder `Web` within your vault. Create an `_images` directory as a subfolder of `Web`. So you do something like this:

```bash
cd $VAULT
git clone https://github.com/$gituser/$repo Web
cd Web
mkdir _images
```

Within that, obviously,

- `$VAULT` is the location of your Obsidian Vault
- `$gituser` is your Github user
- `$repo` is the repository you created earlier.

## Add some Sample Content

Add a sample page like this one to your vault:

![Obsidian Git Wordpress](obsidian_git_wordpress.png)

## Commit and Push the repository

Commit your changes and push them; this is what you'll regularly do later (there are ways to have a button within Obsidian to do that for you, but I won't cover this here):

```bash
cd $VAULT/Web
git add . && git commit -a -m "Web Update." && git push
```

Where obviously, `$VAULT` stands for whereever your vault happens to be.

## Verify that your Content comes through to Wordpress

### Within Github

Where you configured your Webhook within Github, there is a `Recent Deliveries` tab, where you can see the actual requests that were sent over to your Wordpress site, as well as any potential errors. You can also re-trigger the sending right there.

### Within Wordpress

Where you configured your Plugin, you can see your repository, and at the top there's a `Logs` button which will show you if and how the plugin was called by Github.


## Caveats

### Hack needed for Images

As written above, I needed to hack the plugin to make images work; also, I'd have to rename an image each time I'd want to change it. I'll contact the developer of the plugin and post an update here if this gets fixed somehow.

### Lag for Refresh

I spent like half an evening trying to find out why on earth an update did not end up on my page. I could publish a new post, but an existing post apparently wasn't updated. Turns out, I was not waiting long enough. So be patient. It may take 5 minutes.

Sometimes it is instantaneous, sometimes, it does not happen at all; in that case, I may just push again...

### No Delete (and no Edit within Wordpress)

You cannot delete posts. Well you can (and you should) from your Web folder and thereby from your Git repository, but you would have to delete posts manually from your Wordpress repository. Essentially, it's a oneway street. You can of course edit in Wordpress, but this will not be pushed back to your Github repository.

### No old Content

If your Wordpress site already has some legacy content, you can of course go through the hassle and copy it all out and re-publish it (really manually); it wasn't worth it for me: the new procedure just adds (and later updates) posts from within Obsidian, so the old content can just stay there the way it is.

# Summary

This is really an amazing plugin. It allows me to use Obsidian, where I work every day anyway, to just publish content very easily to my Wordpress site.


