# Dockit
Script that supports local Swift development and debug with Docker

## Getting Started

### Clone Project
First you will want to clone the project directory:
```
~ $ git clone https://github.com/pbohrer/dockit.git
Cloning into 'dockit'...
remote: Counting objects: 14, done.
remote: Compressing objects: 100% (12/12), done.
remote: Total 14 (delta 3), reused 13 (delta 2), pack-reused 0
Unpacking objects: 100% (14/14), done.
~ $ cd dockit
dockit $
```

### Run Install Step
Now we need to run the install sub-command in the installed script. This command is pretty simple.  It just attempts to create a link from /usr/local/bin/dockit to the local script. In addition, it will pull the docker image needed for local dev/debug in Swift.
```
dockit $ ./dockit install
++ pwd
+ sudo ln -sf /Users/pbohrer/work/swift/docker-swift-script/dockit /usr/local/bin/dockit
+ docker pull ibmcom/swift-ubuntu
Using default tag: latest
latest: Pulling from ibmcom/swift-ubuntu
16da43b30d89: Already exists 
1840843dafed: Already exists 
91246eb75b7d: Already exists 
7faa681b41d7: Already exists 
97b84c64d426: Already exists 
a1545d31f6a0: Pull complete 
acc54fa9ad21: Pull complete 
5b8a589dad34: Pull complete 
aacead654d22: Pull complete 
4209b9a4bd91: Pull complete 
db4a7e0f6312: Pull complete 
a53325605dc1: Pull complete 
95693604b961: Pull complete 
7f2d4189618e: Pull complete 
55ebe86dee91: Pull complete 
623b2896d3d4: Pull complete 
b33040a2d643: Pull complete 
97a4d4f4fd28: Pull complete 
124d6e745607: Pull complete 
e48951805a91: Pull complete 
db8e720fc4c5: Pull complete 
Digest: sha256:5741e4e39aab970cb423d6413a78a226ce308f81fc5ba690c249fe96573ea35c
Status: Downloaded newer image for ibmcom/swift-ubuntu:latest
+ set +x
dockit $
```

### Install Docker Dependency
If the script does not detect Docker installed, it will prompt you to install it first

```
$ dockit
Install docker on this machine to use this script.
Go to https://docs.docker.com/docker-for-mac/ for more instructions.
```

## Usage
```
$ dockit -h
Usage: ./dockit [-h|-o|-p <port>] {build | install | run <command>}
	-h : display options
	-p <port> : port to listen on both locally and in container (default=8090)
	-o : open browser on localhost:8090
	     default <command> is .build/debug/docker-swift-script
```

## Working Directory
This script assumes that you are in a working Swift project directory. It looks for a Package.swift in the local directory and if it does not find one, it will report an error:
```
$ dockit
Usage Error: This command must be run in a directory that contains a Swift package

Usage: ./dockit [-h|-k|-o] {build | install | run <command>}
	-h : display options
	-p <port> : port to listen on both locally and in container (default=8090)
	-k : kill any other process listening on local port 8090
	-o : open browser on localhost:8090
	     default <command> is .build/debug/docker-swift-script
```

## Build
This option is used to issue a "swift build" for the current directory within the Docker container.
```
kitura-angular $ dockit build
Building on Linux
Cloning https://github.com/IBM-Swift/HeliumLogger.git
HEAD is now at a6ea950 Regenerated API documentation
Resolved version: 1.6.0
Cloning https://github.com/IBM-Swift/LoggerAPI.git
HEAD is now at 1e6f08e Perf: Use autoclosures to prevent String construction (#18)
Resolved version: 1.6.0
Cloning https://github.com/IBM-Swift/Kitura.git
HEAD is now at 570f9f3 Updated dependencies
Resolved version: 1.6.0
Cloning https://github.com/IBM-Swift/Kitura-net.git
HEAD is now at 5e7ad7d Updated dependencies
Resolved version: 1.6.0
Cloning https://github.com/IBM-Swift/BlueSocket.git
HEAD is now at 0ca58bd Minor changes to correct some error messages that were inaccurate due to cut/paste errors.
Resolved version: 0.12.27
Cloning https://github.com/IBM-Swift/CCurl.git
HEAD is now at 3cfb752 Add header callback helper function (#9)
Resolved version: 0.2.3
Cloning https://github.com/IBM-Swift/BlueSSLService.git
HEAD is now at caff269 Apparently on Linux, simultaneous reads and writes are not thread safe. Used the previously introduced SSLReadWriteDispatcher struct that allows for sync'ing access when doing reads and writes. This effectively forces reads and write to occur serially.  Also on Linux, added a check to see if the remote connection has gone away prior to issuing a SSL_shutdown() request.  This should alleviate the problem of apps receiving a SIGPIPE when attempted to shutdown SSL after the remote has already terminated.
Resolved version: 0.12.20
Cloning https://github.com/IBM-Swift/OpenSSL.git
HEAD is now at ae27a1d Update README.md
Resolved version: 0.3.1
Cloning https://github.com/IBM-Swift/CEpoll.git
HEAD is now at 111cbcb IBM-Swift/Kitura#435 Added a README.md file
Resolved version: 0.1.0
Cloning https://github.com/IBM-Swift/SwiftyJSON.git
HEAD is now at 5ca3f00 Merge pull request #30 from IBM-Swift/issue_939
Resolved version: 15.0.5
Cloning https://github.com/IBM-Swift/Kitura-TemplateEngine.git
HEAD is now at d876e99 An alternative implementation of PR https://github.com/IBM-Swift/Kitura-TemplateEngine/pull/11 (#12)
Resolved version: 1.6.0
Compile Swift Module 'Socket' (3 sources)
Compile Swift Module 'LoggerAPI' (1 sources)
Compile Swift Module 'KituraTemplateEngine' (1 sources)
Compile Swift Module 'SwiftyJSON' (2 sources)
Compile Swift Module 'HeliumLogger' (2 sources)
Compile Swift Module 'SSLService' (1 sources)
Compile CHTTPParser utils.c
Compile CHTTPParser http_parser.c
Linking CHTTPParser
Compile Swift Module 'KituraNet' (35 sources)
Compile Swift Module 'Kitura' (42 sources)
Compile Swift Module 'kitura_angular' (1 sources)
Linking ./.build/debug/kitura-angular
kitura-angular $
```

## Run
This command option can then be used to launch the command in an interactive session in Linux. The command running with Linux will be able to receive signals so you can do things like "Control-C" to the running process.
```
kitura-angular $ dockit run
Running  on Linux
  Note: Local port 8090 is mapping to port 8090 in container
	      Use -o option to open a browser session to this port or open manually
[2017-01-27T18:22:38.913Z] [VERBOSE] [Router.swift:67 init(mergeParameters:)] Router initialized
[2017-01-27T18:22:38.922Z] [VERBOSE] [Kitura.swift:70 run()] Starting Kitura framework...
[2017-01-27T18:22:38.923Z] [VERBOSE] [Kitura.swift:80 start()] Starting an HTTP Server on port 8090...
[2017-01-27T18:22:38.923Z] [INFO] [HTTPServer.swift:85 listen(on:)] Listening on port 8090
^C
kitura-angular $
```
- The default command is .build/debug/<local-dir-name>. You can override the command to run by specifying it at the end of the run command.
- If you run dockit -o run, it will open a local browser to the specified port.

## Debug
This command option will launch the target command with the debugger (lldb).
The debugger session is interactive.
```
kitura-angular $ dockit debug
Debugging  on Linux
(lldb) target create ".build/debug/kitura-angular"
Current executable set to '.build/debug/kitura-angular' (x86_64).
(lldb) breakpoint set -f main.swift -l 24
Breakpoint 1: where = kitura-angular`kitura_angular.(closure #1) + 35 at main.swift:24, address = 0x0000000000415213
(lldb) run
Process 17 launched: '/root/app/.build/debug/kitura-angular' (x86_64)
[2017-01-27T18:47:14.370Z] [VERBOSE] [Router.swift:67 init(mergeParameters:)] Router initialized
[2017-01-27T18:47:14.717Z] [VERBOSE] [Kitura.swift:70 run()] Starting Kitura framework...
[2017-01-27T18:47:14.717Z] [VERBOSE] [Kitura.swift:80 start()] Starting an HTTP Server on port 8090...
[2017-01-27T18:47:14.718Z] [INFO] [HTTPServer.swift:85 listen(on:)] Listening on port 8090
[2017-01-27T18:47:22.203Z] [VERBOSE] [HTTPIncomingMessage.swift:245 parsingCompleted()] HTTP request from=172.17.0.1; proto=http;
[2017-01-27T18:47:22.221Z] [ERROR] [IncomingSocketManager.swift:124 process(epollDescriptor:)] epollWait failure. Error code=4. Reason=Interrupted system call
[2017-01-27T18:47:22.236Z] [ERROR] [IncomingSocketManager.swift:124 process(epollDescriptor:)] epollWait failure. Error code=4. Reason=Interrupted system call
[2017-01-27T18:47:22.247Z] [ERROR] [IncomingSocketManager.swift:124 process(epollDescriptor:)] epollWait failure. Error code=4. Reason=Interrupted system call
[2017-01-27T18:47:22.263Z] [ERROR] [IncomingSocketManager.swift:124 process(epollDescriptor:)] epollWait failure. Error code=4. Reason=Interrupted system call
[2017-01-27T18:47:22.263Z] [ERROR] [IncomingSocketManager.swift:124 process(epollDescriptor:)] epollWait failure. Error code=4. Reason=Interrupted system call
[2017-01-27T18:47:22.321Z] [VERBOSE] [HTTPIncomingMessage.swift:245 parsingCompleted()] HTTP request from=172.17.0.1; proto=http;
[2017-01-27T18:47:22.321Z] [VERBOSE] [HTTPIncomingMessage.swift:245 parsingCompleted()] HTTP request from=172.17.0.1; proto=http;
[2017-01-27T18:47:22.322Z] [VERBOSE] [HTTPIncomingMessage.swift:245 parsingCompleted()] HTTP request from=172.17.0.1; proto=http;
[2017-01-27T18:47:22.393Z] [VERBOSE] [HTTPIncomingMessage.swift:245 parsingCompleted()] HTTP request from=172.17.0.1; proto=http;
[2017-01-27T18:47:22.465Z] [VERBOSE] [HTTPIncomingMessage.swift:245 parsingCompleted()] HTTP request from=172.17.0.1; proto=http;
Process 17 stopped
* thread #5: tid = 24, 0x0000000000415213 kitura-angular`(request=<unavailable>, response=<unavailable>, next=<unavailable>) + 35 at main.swift:24, name = 'kitura-angular', stop reason = breakpoint 1.1
    frame #0: 0x0000000000415213 kitura-angular`(request=<unavailable>, response=<unavailable>, next=<unavailable>) + 35 at main.swift:24
   21  	
   22  	router.get("/timestamps") {
   23  	    request, response, next in
-> 24  	    response.send(json: JSON(timestamps))
   25  	    next()
   26  	}
   27  	
(lldb)
```

## Network Ports
The script will attempt to directly map ports on the local system to those within the container.  By default, many Kitura projects leverage port 8090.  
### Overriding Port Number
If your application is listening on a different port, you can override the mapped port with the -p <port> option.
The script will also set the environment variable PORT to this same port within the container.

The following overrides the default port to 8000:
```
kitura-angular $ dockit -p 8000 run
Running  on Linux
  Note: Local port 8000 is mapping to port 8000 in container
	      Use -o option to open a browser session to this port or open manually
[2017-01-27T18:32:52.309Z] [VERBOSE] [Router.swift:67 init(mergeParameters:)] Router initialized
[2017-01-27T18:32:52.317Z] [VERBOSE] [Kitura.swift:70 run()] Starting Kitura framework...
[2017-01-27T18:32:52.317Z] [VERBOSE] [Kitura.swift:80 start()] Starting an HTTP Server on port 8000...
[2017-01-27T18:32:52.318Z] [INFO] [HTTPServer.swift:85 listen(on:)] Listening on port 8000
^C
kitura-angular $
```
### Conflicts on Port Number
Often times when doing local dev/debug, you end up having a conflict on ports.  If the script detects that another process is listening on the specified port (8090 by default), it will prompt the user to either terminate those processes or specify an alternate port.
```
$ dockit run
Another process already listening on port 8090
Either stop the following processes or override with the -p <port> option
com.docke 28748 pbohrer   22u  IPv4 0xe49b7948ac0869f      0t0  TCP *:8090 (LISTEN)
com.docke 28748 pbohrer   23u  IPv6 0xe49b794a4d0ed67      0t0  TCP localhost:8090 (LISTEN)
```
