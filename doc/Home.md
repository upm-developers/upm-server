This is the wiki for upm-server (oh I did totally not know that!)

These are currently all features of the upm server:

## Providing packages
The main purpose of a server is to send the user a package when he wants to `upm copy`

For example:  
upm copy google.com:upm@3.1 upm

This will install the package from google.com because we specified the source is at google.com.

_we can also add an alias for that, like upm download, so it looks better_

How does the server know which package to return when it sees "upm@3.1"?  
It has a list which maps package identifiers to URLs (the packages will usually lie on the server).  

For example one line in the list:
upm@3.1 = http://google.com/upm/upm_3_1

(Syntax is not important)

## Taking packages

However, the packages have to come from somewhere.  
The maintainer can of course add them all by himself, however, this is much work.
He can need some help.  
That's why someone who created a package can **propose** a package for the server.  
That means: he wants to map a package to an identifier.  

For example:  
packager:  
upm copy ./package google.com:package  
this proposes the package
server:
give me da password!  

## Removing packages
The packager can use `upm remove` on the server to remove a package (or multiple)
if he has the rights for the package name.

## Protecting package names
Now, imagine you uploaded a package "sound_goodizer".  
And then imagine a really mean packager. He creates a virus, packages it, and
proposes it under the same name. And then your package is overriden by a virus.  
And all your millions of users will download it because they think you added a new fascinating feature.  
Not really good.  

That's why upm generates a password for you when you uploaded a package.  
And every time you update your package you give the password to upm and say
"yes I am the original packager! i got the key when i created the package!".
And the bad guyz can not override your package!

_Implementation proposal:  
When the server sees that the user wants to copy a package into the server,
it looks if the package name has an entry in the package-password list,
if not, generate password, add it to list, send to user, else, ask for password, compare._
_Implemenation proposal:
When copying the package into the server, add it to the package-url list_

## Checked packages

Now, you can be sure your package stays yours.  
But that does not mean that *your* package is a virus!  
That's why the maintainer has to be able to check the packages.  
There are three stages of a package: `waiting`, `unchecked`, `checked`.
Waiting packages cant be downloaded by users, the are not checked by the maintainer.  
Unchecked packages can be downloaded, though they are not checked. But the user will
be notified.  
Checked packages can be downloaded, and have been checked by the maintainer.  

The maintainer decides whether freshly proposed packages should be `waiting` or `unchecked`.

_Implementation proposal:
Have an entry in the meta file for the state of the check_

## Synchronizing between servers
Maybe the maintainer says, hey i want to have another server in china to deliver faster connections for those who live there.  
However, he wants his two servers to have the same content. So they have to synchronize.  
First, both servers have to know each other. Every server has a list of other servers it synchronizes with.  
Now, if any state changed, the server will notify all other servers in the list.  

## Summary
Server has three lists:
 * Package-URL list
 * PackageName-Password list
 * SyncServer list

Events:
 * update package (parameters: identifier of package)  
for both adding and updating  
changing check state counts as updating (since, if my proposal is accepted, is modification of meta file)
triggered by `upm copy` (implementation proposal)
 * remove package (parameters: identifier of package)
triggered by `upm remove` (implementation proposal)
