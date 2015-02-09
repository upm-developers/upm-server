This is the wiki for upm-server (oh I did totally not know that!)

These are currently all features of the upm server:

## Providing packages
The main purpose of a server is to send the user a package when he wants to `upm copy`

For example:  
`upm copy google.com:upm@3.1 upm`

This will install the package from `google.com` because we specified the source is at google.com.

_we can also add an alias for that, like `upm download <serverpackage>`, so it looks better_

How does the server know which package to return when it sees "upm@3.1"?  
It has a list which maps package identifiers to URLs (the packages will usually lie on the server).  
This list should be a file.  

For example one line in the list:
`upm@3.1 = http://google.com/upm/upm_3_1`

(Syntax is not important here)

## Taking packages

However, the packages have to come from somewhere.  
The maintainer can of course add them all by himself, however, this is much work.
He can need some help.  
That's why someone who created a package can upload a package to the server.  

For example:  
packager:  
`upm copy ./package google.com:package`  

Uploads the package to google.com.

*we can also add an alias like `upm upload <package> <server>` for better readability*

## Removing packages
The packager can use `upm remove` on the server to remove a package (or multiple)
if he has the rights for the package name.

## Protecting package names
Now, imagine you uploaded a package "sound_goodizer".  
And then imagine a really mean packager. He creates a virus, packages it, and
proposes it under the same name. And then your package is overriden by a virus.  
And all your millions of users will download it because they think you added a new fascinating feature.  
Not really good.  

This is why every user has certain rights associated with a package *name*.  
How this is done is not clarified yet. (TODO)

## Checked packages

Now, you can be sure your package stays yours.  
But that does not mean that *your* package is a virus!  
That's why the maintainer has to be able to check the packages.  
Every package has *check states*. The maintainer of the server can specify which stages exist.  
One example of different stages is:  
Waiting packages cant be downloaded by users, the are not checked by the maintainer.  
Unchecked packages can be downloaded, though they are not checked. But the user will
be notified.  
Checked packages can be downloaded, and have been checked by the maintainer.  

Please note, that a newer version of package is a different package and thus has to be checked again, which makes sense.

## Synchronizing between servers
Maybe the maintainer says, hey i want to have another server in china to deliver faster connections for those who live there.  
However, he wants his two servers to have the same content. So they have to synchronize.  
First, both servers have to know each other. Every server has a list of other servers it synchronizes with.  
Now, if any state changed, the server will notify all other servers in the list.  
