{
  "title": "Projects Overview",
  "date": "2020-01-20T13:44:19+02:00",
  "tags": [
    "hugo"
  ],
  "projects" : [
    "demeester.dev"
  ]
}

A simple web page to publish my insights via posts ánd a webpage where my projects are listed and posts are clearly linked with these projects.

I only started this blog recently. And as with every project i can see multiple angles for improvement. For this iteration I focused on getting my projects visible.
I want an overview showing all the projects i have worked on and the amount of information (posts) that followed from that project. 
It felt like this means i needed to introduces new types and relations to my site, wich sounded to me like I needed a custom taxonomy.
For this website I came up with the following idea:

{{< img src="project-taxonomy.jpg" alt="project taxonomy">}}

I want to list all my projects and add all related posts to the page of each project.
todo:

  - [ ] build a new content type.
  - [ ] create type templates for that type
  - [ ] add relations between the types `project` and `post`

## Project Content Type

The basis for Hugo's content centered around content types.  As mentioned in [Hugo's documentation](https://gohugo.io/content-management/types/), it is very easy to create a new type. Every folder under content is assumed to specify the type of it's content. We can create a new project page by adding it to a folder called projects under content. And this simple type creation allows me to create a different view and taxonomy for projects as I wished. 

We create the project type by adding the folder content : `/content/projects/`. The first project page I create is for this website, so I a page in this folder and add content: `/content/projects/demeester.dev/index.md`.

  - [x] build a new content type.

I want to add a link to the source and a link to the project website to the project page. The frontmatter is perfect for this type of data.
I add `srcUrl` and `siteUrl` as front matter to the project
```
{
  "title": "DeMeester.dev",
  "project" : "demeester.dev",
  "srcUrl" : "https://github.com/demeesterdev/demeester.dev",
  "siteUrl" : "https://demeester.dev",
  "date": "2020-01-19T12:22:48+01:00",
  "tags": [
    "hugo"
  ]
}
```

## Project Templates.

With the parameters `srcUrl` and `siteUrl` we can create the link pointing to github on the project page and list. 

### Single page
Add the file `layouts\projects\single.html` to create a custom type template and add the following code.

```html
{{ define "main" }}
  <h1 class="title"><a href="{{.Permalink}}">{{ .Title}}</a></h1>
  
  {{ with .Params.srcUrl }}
    {{ $srcSpec := strings.TrimPrefix "http://" (strings.TrimPrefix "https://" .)}}
    <h6 class="subtitle src-link">source: <a href="{{ . }}" target="_blank" rel="noopener noreferrer">{{$srcSpec}}</a></h6>
  {{ end }}
  {{ with .Params.siteUrl }}
    {{ $siteSpec := strings.TrimPrefix "http://" (strings.TrimPrefix "https://" .)}}
    <h6 class="subtitle site-link">site: <a href="{{ . }}" target="_blank" rel="noopener noreferrer">{{$siteSpec}}</a></h6>
  {{ end }}

  {{ .Content}}  
{{end}}
```
because `with` is used `srcUrl` and `siteUrl` are only processed if they are defined in frontmatter.
within the codeblock for each of these a simpeler string is created for users to read.

During rendering this will be formatted to a title and source description:
```html
<h1 class="title"><a href="https://demeester.dev/projects/demeester.dev/">DeMeester.dev</a></h1>
<h6 class="subtitle src-link">source: <a href="https://github.com/demeesterdev/demeester.dev" target="_blank" rel="noopener noreferrer">github.com/demeesterdev/demeester.dev</a></h6>
<h6 class="subtitle site-link">source: <a href="https://demeester.dev" target="_blank" rel="noopener noreferrer">demeester.dev</a></h6>
```

# Project List

We can use similar code for a list as for the project page.
But in a list we need to walk trough every post using `range`.

Create the project list template (`layouts\projects\list.html`) and add the following content:
```html
{{ define "main" }}
  {{ range .Pages }}
    <h1 class="title"><a href="{{.Permalink}}">{{ .Title}}</a></h1>
    {{ with .Params.srcURL }}
      {{ $srcSpec := strings.TrimPrefix "http://" (strings.TrimPrefix "https://" .)}}
      <h6 class="subtitle src-link">source: <a href="{{ . }}" target="_blank" rel="noopener noreferrer">{{$srcSpec}}</a></h6>
    {{ end }}
    {{ with .Params.siteURL }}
      {{ $siteSpec := strings.TrimPrefix "http://" (strings.TrimPrefix "https://" .)}}
      <h6 class="subtitle site-link">site: <a href="{{ . }}" target="_blank" rel="noopener noreferrer">{{$siteSpec}}</a></h6>
    {{ end }}
  {{ end }}
{{end}}
```
This results in a nice template for projects and project lists.  
another part done:

  - [x] create type templates for that type

## Posts Related to Project

Having a list of projects is great, but it is missing a list of posts for that project. 
one way of doing this is listing all posts and only showing the post for a match in frontmatter.
I choose to add a project name to a projects front matter and to add a field `projects` to each posts frontmatter

The plan in short: 
1. add info the info to the frontmatter of post and project template
2. list the related posts on project and list pages

### Connecting posts and projects

To match projects and posts the index needs to be added in the front matter of both types.
for the project i add an item difing its project name.
```json
{
  "project" : "demeester.dev"
}
```
This key is the name used to match posts with projects.
within the post i add an array of projects related to the post
```json
{
  "projects" : ["demeester.dev"]
}
```

### use related posts in rendering.

using the params in frontmatter we can retrieve all related pages for each project.

From a project template we can get all related pages to the current project (`.`) based on the param `"project"`.
```html
{{ $projectPosts := where .Site.RegularPages "Params.projects" "intersect" (slice .Params.project)}}
```
the where clause retursn all items from a collection that fulfill the statement.
to match an araay or slice with a string you can use an in clause. but it only matches a single param from tthe collection item to a slice.
here we want to match the array of projects of a page to the current project name.
this is where we can use intersect. intesrsect returns true if there is an intersection between 2 arrays.
we need to construct an array with `slice` from our singular sting `.Params.project`

now we can add a counter to the post list template, to see how much I published on the peticular project:
```html
   <h6 class="subtitle">posts: <a href="{{.Permalink}}#project-posts">{{len $projectPosts}}</a></h6>
```

and on the projects single template i can add the full list of posts for the project:
```html
{{ begin 'main'}}

  ....

  {{ if $projectPosts}}
    <h3 id="project-posts">Project Posts:</h3>
    <ul>
      {{ range $projectPosts }}
        <li><a href="{{.Permalink}}">{{.Title}}</a></li>  
      {{ end }}
    </ul>
  {{end}}

{{end}}
```

and we are done:

  - [x] build a new content type.
  - [x] create type templates for that type
  - [x] add relations between the types `project` and `post`

## Conclusion

Hugo is a powerful system, and using a custom taxonomy with matching templates you can format content any way you like.
Trough related content you can show relations between different types while keeping the speed you are used to.

I don't know what the power of hugo will bring me in the future., but I'll keep you posted.

