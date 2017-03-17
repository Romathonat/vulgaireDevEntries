I am using Docker a lot these days.

I assume that you know at least a little bit Docker to understand this article.

## The problem
Here was my problem: I want to install a [gitlab](https://about.gitlab.com/) instance and a [taiga](https://taiga.io/) 
instance on the same machine. Both are using the port 80. So I want to use gitlab when I type http://my-ip/gitlab and use taiga when 
I type http://my-ip/taiga.

## Classical resolution
The classical approach for this kind of problem would be to use a reverse proxy (apache server or nginx for example). Each time someone
connects to the server, he reaches the reverse proxy (with port 80), wich then redirect on gitlab or taiga, using different port internally 
(for example 8081 and 8082).

This approach works fine, but I want to use Docker. Indeed, I want to simplify the 
