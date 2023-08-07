---
title: Making a Blog with Hugo - Part 1
subtitle: Hugo is a great tool to start a blog quickly
date: 2023-07-31T10:46:37+07:00
tags: ["non-CS", "blog", "hugo", "EN"]
---

In an era of social media dominance, where everyone vies for attention in a sea of updates and distractions, a personal homepage is like an oasis of authenticity. 

<!--more-->

## First step

When I began my quest to build a personal homepage, I was faced with the choice of numerous methods. After careful research and advice from my senior, I stumbled upon Hugo and discovered its capabilities as a static site generator. Unlike traditional Content Management Systems (CMS) that generate web pages dynamically, Hugo compiles static files, resulting in faster load times, enhanced security, and overall better performance. 

## Why Hugo ?

I could link [their official website](https://gohugo.io/) and let they explain it for you of why you should use it, but if I must explain in the fewest words possible, I would say Hugo is fast, flexible, simple and boring.

Hugo is fast. Why? Hugo's impressive speed can be attributed to its foundation in Go, a language known for its performance. Within the Go ecosystem, there’s no concept of 100 megabytes dependencies as everything is optimized for maximum speed and efficiency. This design philosophy enables Hugo to deliver content at <1 ms per page and the average site builds in less than a second, making it a top choice for those seeking a rapid and efficient static site generator.

Hugo is flexible. When I began building my website, I opted for an open-source theme - [BeautifulHugo](https://themes.gohugo.io/themes/beautifulhugo/) providing me with a foundation that matched my preferences and requirements. Sometimes, I found myself want to create and integrate features beyond the scope of a simple blog, and Hugo allowed those unique elements seamlessly into my website. This versatility allowed me to tailor the website to my taste and modify it completely as my needs evolved over time. 

Hugo is simple, which also is the main reason I choose Hugo. There’s not much you have to learn to get started if you are familiar with Markdown.

Hugo is boring, which may not be a bad thing. As a developer, I often tempted to tweak things here and there all the time. Unlike many JavaScript frameworks that boast cool and cutting-edge features, Hugo does not showcase such novelties. This very attribute grants me ample time to concentrate on what truly matters when managing a blog: writing content. With Hugo, the focus remains on the essence - the content itself - rather than the box that holds it.

## Is Hugo suitable for you ?

If you are a developer willing to write content in Markdown, Hugo proves to be a remarkable tool. However, the main obstacle that many individuals might encounter is the learning curve associated with Markdown, as it is not readily embraced by non-technical users. It's understandable why some might hesitate to use Markdown, but there are solutions, for example, the Bear editor, which offers a GUI-centric approach to alleviate this issue.

Additionally, to ensure a seamless workflow, you must be ready to adopt a Git-centric approach as the process for writing a blog involves:

* Writing the post using Markdown
* Commit the changes to a Git repository, typically hosted on platforms like GitHub
* Deploys these changes to the server responsible for hosting the site.

By mastering this workflow, you can make the most out of Hugo and streamline your content creation process.

## Conclusion

In the scope of a small blog, I hope I give you a little guidance if you are planning to start a new blog. Hugo is great, however it is not one of a kind, there are many option to consider like Gatsby, Wordpress, 11ty,... In the next blog, I will show you the technical side of building the blogsite using Hugo
