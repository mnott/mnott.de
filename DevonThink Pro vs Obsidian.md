---

title: DevonThink Pro vs. Obsidian
menu_order: 1
post_status: publish
post_excerpt: Comparing DevonThink Pro to Obsidian
taxonomy:
    category:
        - Obsidian
        - Computer
    post_tag:
        - DevonThink Pro
        - DTP
        - DTTG
        - Obsidian
        - Computer

---
 
# DTP vs. Obsidian

On the DevonThink Pro (DTP) forum, this morning, I was reading [a discussion](https://discourse.devontechnologies.com/t/devonthink-obsidian/73208/42) about DTP vs. Obsidian. It made add my own notes, but I also thought, why not elaborate here.


## My Usage of DTP

I'm using DTP since way over 10 years. I've written plugins for it, and I'm managing over 2 million documents, mostly eMails, within it. I am not using DTTP (the "to-go", mobile version), because I never even managed to synchronize 50k documents over to it, and it won't synchronize if you're not having the app in front; which means, as an on-the-go eMail archive, it's unreliable for me. Maybe after 2 years since I paid for DTTP, I've not been able to figure it out.
 
### Mail to DTP

The way that I push my eMails to DTP is through a local IMAP server that I configured within a Docker container. You can get the code [here](https://github.com/mnott/mail2dt).

### OCR

Before switchting to an M1 Mac, I was using my own wrapper around Abbyy, again using a Docker container, to process OCR. That still works, but because that's a very old version of Abbyy, I've switched over to DTP for doing my OCR. DTP comes with a license to Abby, so all you really need to do is something like this:

![DTP OCR](dtpocr.png)



Note the filter `Word Count is less than 1` - this ensures that I'll process only files that have not yet been OCRd.

### Summary

Essentially, I'm using DTP as a long term memory. A shoe box for the basement where I can push all sorts of documents into, and still be able to retrieve them. 


## My Usage of Obsidian

I've been using Obsidian since it came about, probably like 2 years by now. My use of Obisidan is entirely different from DTP. I use Obsidian as my daily control center: Everything I touch daily, goes through Obsidian at some point, if it doesn't go directly into long term storage (DTP). I have three specific workflows that I am using Obsidian for: 

1. Daily Project work, including Meeting notes
2. Scientific Research
3. Web publishing to my blog

Let's go through them one by one.

### Daily Project Work

![Obsidian Dashboard](obsidian1.png)

Let the above screen shot speak for itself, particularly, if you have seen DTP. You can see a folder tree. Since Obsidian just shows what's in the folder tree, I'm using Symbolic Links sometimes to bring entirely different things into one view.

Next to the folder tree, you can see my daily control center. It is like a dashboard where just when hovering over any link, I'll get a little popup showing me the content of that note. It has a tabbed view, so as an example, I've opened my meetings note next to it, showing me the past and upcoming meetings (and, today's meetings, just as of this Saturday, I have none, so none are shown.) These lists of meetings are generated automatically for me as part of some programming that I have done. You can find some of those scripts [here](https://github.com/mnott/obsidian-scripts).

Next to that, to the right, you can see a calendar view; when I click on a day, I'll generate a "daily note". And below that you can see a graph tree that links to related notes for the current note. The graph view in itself warrants it's own screenshot:

![Obsidian Graph](obsidian2.png)

You can run queries to display only parts of it, you can color code it, etc. If you hover over any of those notes, it'll show you not only the note title, but als it's related notes. This is for me a good way to see which things are not yet linked to other things.

When it comes to a typical meeting during the day, I can just create a meeting note within my meetings folder. And because, using the Templater plugin (more on plugins later), I can define for each folder a default plugin, I'll have my meeting note already set up so that it'll contain only the content that I need: By adding participants and tags to a YAML header, the "related meetings view" (similar to what you see in the dashboard above) will show me only meetings that I held or will hold that are relevant for the current meeting. This uses precisely the same code that I've shown above, by just adding this to the markdown file:

```dataviewjs
	customJS.Meeting.getRelatedMeetings(dv, window, -7, 7,
	{header: "Related Meetings", sort: "asc"});
```

### Scientific Research

I was using [Scrivener](https://www.literatureandlatte.com/) quite a lot as a means to organize snippets of thoughts and to then have them exported and run through LaTeX. I even wrote some automation around that; you can see it [here](https://github.com/mnott/texdown) and watch a video about my academic workflow [here](https://youtu.be/86rnBMz6XnU).

When I started using Obsidian, some of the basic functionality that I was using with Scrivener, like, seeing a continuous view of my notes, and dynamically including and excluding stuff, were missing. It was also at a point where extending Obsidian was still a bit poorly documented, so I set it aside for a while.

Just recently though, I got back to it as part of my dissertation, and re-implemented my LaTeX wrapper so that it can work well with Obsidian. You can download the code [here](https://github.com/mnott/dissertation-template) and see it here:

![Obsidian LaTeX](obsidian3.png)

On the left, you can see my LaTeX script that manages the different tasks required to move from the editing view in Obsidian (center), to the finished document (right). As you can see, the way I've implemented a markdown language on top of LaTeX allows me to have very few non-content elements in the text that I am writing, while at any moment maintaining the entire flexibility of LaTeX. After all, I'm using LaTeX since about 30 years, and while I am not at all afraid of seeing all sorts of LaTeX code within the text that I am writing, I strive for having as little of it as possible so that I can actually concentrate on the text, not on the format.

On top of that, and this is not shown here, Obsidian integrates perfectly well with my Literature database that I manage in Bibdesk; I can just pull in any citation from that database with a hotkey. This, again, shows the power of Obsidian when it comes to extensibility: Someone wrote a plugin for it.


### Web publishing to my blog

I have a little wordpress blog that you're reading this off of. I really hate wordpress when it comes to editing, I have to say. So much so that I've not been blogging at all for years. It was just outside my work environment, in a browser, aweful. So I wouldn't.

Then, just yesterday, I thought, why not blog from within Obsidian, and I made it work in a slightly unexpected way:

1. I write my blog in Obsidian
2. I push the blog to a Github repository
3. Github pushes the content on to my blog

You can read about the process [here](http://www.mnott.de/obsidian-for-wordpress/).

### Summary

If DTP is my long term memory, then Obsidian is my short term memory. It is dynamic, active, very adaptable to my way of working. It helps me staying on top of things. Even though it calls its containers "vaults", I rather see DTP as a vault. Somewhere hidden in the basement. Obsidian is right there, at my fingertips.

## Comparison of DTP and Obsidian

### Esthetics

There are as many tastes as there are people. Hence there is no point arguing which one is nicer. What we can arguably call out though is to which degree which tool can be adapted to whatever your taste is.

In this place, Obsidian beats DTP out of the water easily. DTP is just not really adaptable at all. For example, I posted on the DTP forums, like a year ago, whether we could make it so that elements in the trash whould not be strike through - because I just have a hard time reading those items:

![DTP Trash](dtptrash.png)

I've automated importers for my mails that throw certain things into the bin immediately. But every now and them, before emptying the trash, I'd like to quickly scroll through to see if I'm missing anything.

Not possible. I'd literally have to pull them out of there just for that, or use a separate folder as a pre-trash.

On the forum, I got told by the DTP people, nope. Not going to do that. It is what it is.

Likewise, when I go to create a new smart group in DTP, already the selection of columns etc. of those tables are not reusable. I literally have to reselect and align my table columns for each and every view. This is beyond annoying.

Compare that to Obsidian: You'd not even ask these questions. You'd just launch the developer tools (because it's essentially just a browser), point at the element you want to change the style of, find the CSS element that you need to add, add that modification to your own CSS, done. Every single aspect of how Obsidian is presenting itself, you can adapt in any way you want. Literally. You are only limited by your own ability, not by the tool or its developers.

### Extensibility

#### Plugins

Yes, you can script DTP. I've done that quite a lot when it comes to automatically filing stuff etc. But those scripts are essentially things that you need to call yourself. They sit in a script folder.

Obsidian, again, wins over DTP hands down. First of all, there is a substantial community out there that keeps adding plugins to obsidian. You'll find plugins for just about anything. I'm using plugins to link to my LaTeX database, to push to git (I even wrote a git integration plugin myself, but there's a better one out there).

#### Dataviews

One of the most popular plugins is Dataviews. This allows you to see your whole Obsidian vault as a database, and to run SQL like queries on it. For example, just showing up my tasks, or a set of my recently changed files, just is a simple query away. Those queries, compared to DTP, are not sitting in some scripts folder. You have them right in your notes, if you want to.

#### Inline JavaScript

On top of that, you can go further and embed JavaScript right in your notes. Like when I want to see just the latest changed notes, etc., that note will have a little JavaScript in them like this:

```dataviewjs
 var rows = 12;
 var dateformat = "YYYY-MM-DD hh:mm";
 function ftitle(page){
   if (typeof page.title !== 'undefined') {
	   return page.title;
   } else {
	   return page.file.name;
   }
 }
 dv.table(["Modified","File"],
   dv.pages()
   .sort(f=>f.file.mtime.ts,"desc")
   .limit(rows)
   .map(l=>[
	   moment(l.file.mtime.toString()).format(dateformat),
	   "[["+l.file.path+"|"+ftitle(l)+"]]"
     ]
	)
 )
 dv.container.style.width="100%";
```

As you can see, it's pretty simple JavaScript. It is also very fast. The above code shows me the most recently changed files, inside a  note, like so:

![Obsidian Query](obsidian4.png)

Again, as you can see, it is interactive, you can hover over those notes, and they'll show you the content of the linked note.

### Outside JavaScript

If you want to re-use your JavaScript code across a bunch of notes, it of course makes no sense to have it in your notes. So for my meeting notes, the code that shows the  related meetings, is in some external files that I pull in as shown above. Again, there's a plugin, CustomJS, which allows you to do just that.

### Power

All those scripts have access not only to the note, but actually to the complete application model. As the code samples that I linked to show, particularly [this](https://github.com/mnott/obsidian-scripts/blob/main/Meeting.js#L13), you can see that you have access to the entire application model and presentation of Obsidian.


## Community

The plugins show that maybe the substantial advantage of Obsidian is its community. You're missing something? You'll either find a plugin, or just do it yourself. It is very open.

## Static vs. Dynamic Content, and Speed

DTP beats Obsidian easily when it comes to the multitude of document types that it supports. Because it keeps its own metadata about everything, a search across millions of documents is instantaneous in DTP. Obsidian is not there - yet. I'm saying, yet, because Obsidian has already started keeping metadata in its vault, and it is probably just a question of time. But for the moment, DTP is my go to application when I need to find an old email, or some old document (while for documents, I most likely will already find it using Spotlight).

## Verdict

I am not using DTP to its full potential. I use it as a mass storage facility with reliable retrieval. For that, I don't see Obsidian there yet.

In that sense, as I said at the beginning, I can compare those like so:

- DTP: Long term memory / storage. Fast, reliable retrieval. I go there only in the same way that I would go into my basement to fetch something out of a shoe box.
- Obsidian: Short term memory. Very dynamic. My work center every day.

## Outlook

I see Obsidian as a strong competitor to DTP. It is not there yet for some of the reasons I mentioned. But because of its openness and extensibility, there is no reason whatsoever why it shouldn't be able to. So in that sense, it is like Sharing vs. Telling:

- DTP: Telling. Developers literally tell you how you should use it. You don't like it, you often have no way to influence it. They do listen though, like when years ago I found a bug in their metadata storage in extended attributes and fixed it myself. But more generally, it is very much 1990s.
- Obsidian: Sharing. Literally. You have the power to change every single aspect of it.

Therefore, while I'm using them both next to each other, for different purposes, I clearly can see that DTP has found its disruptor there. I'll keep using, as Kristensen would say, DTP only as long as Obsidian is not yet good enough for me to do what I do with DTP; the moment it is, I'll jump ship for these remaining use cases, and never look back (like with Scrivener).

Your mileage will vary, of course.



















