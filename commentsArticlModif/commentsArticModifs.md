# Comments and article submission/modification

## Comments

Some time ago, I developed a login/commentary system for this blog. There was a little work to do (this blog
is developed with django) but it worked, so it was fine for me. With time, I decided that it was too much
work for an user to post a comment : you need to register, give your email adress, then post your comment,
it is too much to do for just a simple comment. That's why I decided to use an external service : disqus.

Disqus is a commentary system, widely used on the web. Its utilisation is quite simple: you create an account,
 add some javascript to your website (or use [django disqus](https://github.com/arthurk/django-disqus), and there you go. It's quite good looking, and one of the main advantage 
is that it let you login with your disqus, or facebook, or twitter or google plus (humpf...) account to post 
a comment. You don't have to register anymore !

So if you have a question, or information, or want to express yourself, feel free to do it in the comment
section !

Also, disqus uses django, they made an interesting post about how they scale to billion of requests : [https://blog.disqus.com/scaling-django-to-8-billion-page-views](https://blog.disqus.com/scaling-django-to-8-billion-page-views)

## Article submission

I also developed a system that let you write a new article that you wanted to be on vulgairedev. It worked too,
but there was somme issues: I didn't let the possibility to upload files (for security reasons), which includes
pictures, and it was impossible to save your article to modify it and send it later. Instead of developing
thoses functionalities, wich takes time, I chose to use something that many devs use, wich support markdown:
github. Now you can modify or add an article on github, and make a pull request. You will just have to 
wait for the integration.

As I said github supports markdown, so you can directly see a preview on it, you can add picture in the markdown,
you can even add latex mathematical formulas. It is not supported (yet ?) by github, but it is on vulgairedev.

