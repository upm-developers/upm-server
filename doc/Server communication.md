
## Providing packages
The main purpose of a server is to send the user a package which matches the
identifier the user specified.  

For example:  
user: "give me package upm>3.1"  
server: "okay, they want upm with a greater version than 3.1... ah, here it is! upm@3.4,
I'll send the contents of the package!"  

Now, we don't know which file the maintainer of the server wants to return on an identifier.  
That's he *either* has to provide a file which maps package identifiers to their locations *or*
a script which will take an identifier and return the location.  

## Taking packages

However, the packages have to come from somewhere.  
The maintainer can of course add them all by himself, however, this is much work.
He can need some help.  
That's why someone who created a package can **propose** a package for the server.  
That means: he wants to map a package to an identifier.  

For example:  
packager: "Hey, why don't use this package here as upm@3.5 in your server?"  
server: well, what does he say?

Once the proposal arrived at the server, what should happen?  
We don't know what the maintainer of the server wants. So, as above, we solve the problem
by looking for a script that takes a proposal (which consists of an identifier and its proposed package content, e.g. a .zip), and then does something with it.  
Some examples:
server: "Ah okay, i will put it in the list for proposed packages, the maintainer will accept it maybe"  
or:  
server: "OH YES I TRUST THIS PACKAGE IT IS GOOD"  
or:  
server: "Okay, nobody ever created a package named verycool, so you are allowed. please setup a password"
or:  
server: "Oh, but somebody already created a package with that name. if you have the password, i'll do it."  

## Passing extra data between server and user
There is just one problem with above examples: the server asks for some extra data, like a password. And maybe he wants to return some data. We don't know and don't want to dictate what data have to be exchanged.

Therefore we have to provide a general way of exchanging data in both directions.  

How this works is not yet determined. One option could be that the script gets a "handle" to the connection which the script can pass to an upm command, like "upm send <connection handle> <message>" (which sends a message to user console output) and "upm receive <connection handle>" (which receives and return s input of user), this way the server can send and ask for data as it likes. This way the script does not even have to have a library for connecting to something! Not still confirmed, however.

## Synchronizing between servers
Maybe the maintainer says, hey i want to have another server in china to deliver faster connections for those who live there.  
However, he wants his two servers to have the same content. So they have to synchronize.  
To accomplish that, both servers first have to somehow know each other, they have to have their adresses saved somewhere.
Second, they somehow have to be able to send data to each other.

Now, we don't want to dictate again. And we don't have to. Because the maintainer can say, whenever he accepts a package proposal to his list, he connects to all his servers and says "hey I got something new".  
Okay, that solves it, right? well, there is just one problem: what will the server that heard of the news do? The server does not know what the maintainer wants to do - so, you guessed, we execute a script that handles an incoming synchronize request and does something with it. Like adding it to its own list!

## Putting it together
Now the maintainer has to provide three scripts:
 * One which takes a request for downloading and returns a file
 * One which takes a request for updating a package and may ask for extra data
 * One which takes a request for synchroniying  

See a pattern? They all react to an incoming message, and do something, and maybe send back something.  
Why not putting all three together? To one script, that handles a message, and using the upm commands "upm send" and "upm receive", it may also interact with the user, without needing to call into a library to handle connections.  
However, it somehow has to know what is being asked. So we need a protocol.  

For example, to download:  
`d<package-identifier>`
and to update:
`update<package-identifier> <package>`
where package is just the direct data or an url (dunno)  
And for server-server communication, they have their own rules, upm does not care.  
This is just an example protocol for illustrating, however, it would work.
