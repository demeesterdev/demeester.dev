{
  "title": "DeMeester.dev: A Static Website",
  "date": "2020-01-05T12:22:48+01:00",
  "tags": [
    "hugo"
  ],
  "projects" : [
    "demeester.dev"
  ]
} 

Plain old HTML and CSS with a splash of JS to add the bare necessities. The thought of building a simple website has been popping in and out of my mind for quite some time. and I always loved the idea of a website that would just work. A website that would not need a platform with regular maintenance.

A website using just HTML and CSS would do the trick. I always loved writing simple HTML, but the amount of boilerplate needed to get a nice and readable website always kept me back from starting a website this way. When I started reading and writing pieces of code I fell in love with the simplicity of markdown, simple, no boilerplate and most version control systems could render it. need a header? just add a `#`, need _italics_? surround your text with `_underscores_`. 

The best of both worlds is combined using static site generators. The main idea: write your content in plain text, let an SSG parse this content to HTML and publish your content on a hosting platform ready to serve static files.  The main idea of static site generators relives me of writing all the boilerplate and lets me focus on writing content. my weapons of choice? markdown and hugo.

{{< img src="code-ssg-webpage.jpg" alt="Logo of Hugo SSG">}}

I love some of the inherent properties of this method. All pages are stored as plain text and are thus storable in version control. This leaves every change viable and traceable, leaving a public history of all my work. There is no CMS or other platform behind it all and that ensures that it keeps working without needing constant love from it's maintainer. and the nice bonus? it is blazingly fast.

This post is the second post I am publishing on this platform and I love the flow of it. It is as simple as writing content in a new file, committing it to version control and pushing it to the sever. If you want to see what you are doing Hugo has a server mode. It allows you to see the result of your writing while you work on it and saved changes are visible nearly instantaneous.

{{< img src="hugo-refresh.gif" alt="Logo of Hugo SSG">}}

During the next few weeks I will shed some lights on some of the specifics I dealt with building this Blog.

I'll keep you posted.