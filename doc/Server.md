Every server has an ID, a list of trusted server IDs, a list of trusted server URLs and a list of projects (name, project URL and package ID)

If a packager wants to makes a change to his package, the changes will be transmitted. If the transmitted package ID is identical to the one on the server, the changes will be applied.

Then the server connects to all the trusted server URLs in his list to commit the change. To authenticate, the own ID will be send. If this ID is in the trusted Server ID list of the remote Server, the changes will be applied.

The communication between Server and Server as well as between Client and Server are very simple to save time:

--> `<prefix> <the authentication ID> <command> <more arguments>`<br>
<-- `<error code> <answer>`

First the one who wants something sends the prefix, the ID, the command and more arguments if necessary.
Then the remote servers sends an error code (hopefully '0') and the answer to the command.

(For more information about prefix, commands, arguments, error codes and answers check out the wiki page to the protocols (coming soon))