---
title: 'How to write a Haskell web service (from scratch)'
date: 2015-10-21T00:00:00-07:00
tags: ["haskell", "scotty", "rest"]
summary: "A hands-on guide to running a Haskell based web service on docker. No previous knowledge of haskell or docker required."
---

_This is a hands-on guide to running a Haskell based web service on docker. No previous knowledge of haskell or docker is required._

> The complete source for this tutorial is available on github: https://github.com/parambirs/hello-scotty/tree/with-json-types

Over the last couple of weeks, I spent some of my free time trying to run a basic Haskell+Docker web service. As it is with every new language or platform, I had to set up and understand a lot of things to get my app up and running. I'm sharing my experience here, hoping that it will help others who might be inclined to get into the Haskell world.

We’ll go through the following 4 steps:
- Set up the development environment
- Build and package a simple app using `cabal`
- Run a basic Haskell web app using Scotty
- Dockerize your scotty-webapp

As of this writing, I have less than 100 hours of Haskell development experience. Therefore, this is a pretty basic guide and focuses on the development tools rather than the language. It was possible for a newbie like me to build and run a basic Haskell web app using docker without much difficulty. Hopefully it'll encourage more people to experiment with Haskell for their side projects!

---

# 1. Set up the development environment


_You may skip this step if you already have `ghc` and `cabal` installed on your system._

## Install the Haskell Platform (GHC)

GHC is the Glasgow Haskell Compiler package which provides you the compiler (`ghc`) as well as a REPL (`ghci`). Cabal is Haskell’s package manager and works similar to `npm` (NodeJS), `sbt` (Scala) and `maven` (Java).

```
% brew update
% brew install ghc cabal-install
```

## Haskell REPL (`ghci`)

Next, we’ll start the Haskell REPL which is called `ghci` and get comfortable with a bit of Haskell syntax.

```
% ghci
```

Let’s define two simple functions that convert temperature values between celsius and fahrenheit:

```
Prelude> let f2c f = (f — 32) * 5 / 9
Prelude> let c2f c = (c * 9/5) + 32
```

Now, we can call these functions to get converted temperatures:

```
Prelude> f2c 800
426.6666666666667
Prelude> c2f 100
212.0
Prelude> c2f 37
98.6
Prelude> <Ctrl+D> — To exit
```

## Run Haskell Scripts (`runhaskell`)

We’ll move these function definitions to a haskell source file and define a main method that prints the converted temperature value.

Copy the following code into a file named `c2f.hs`:

```haskell
c2f c = (c * 9/5) + 32
main = do
  print (c2f 37)
```

_Note: We used `let` for defining functions inside `ghci`. When writing Haskell source files, you don’t require `let`._

Run the script from the command line using `runhaskell`:

```
% runhaskell c2f.hs
98.6
```

## Build an executable

`runhaskell` makes the development cycle easier when you are writing a Haskell application. But you can also create executable files using the `ghc` compiler. Let’s convert our script to a binary and then run it:

```
% ghc c2f.hs
[1 of 1] Compiling Main ( c2f.hs, c2f.o )
Linking c2f …

% ls
c2f c2f.hi c2f.hs c2f.o

% ./c2f
98.6
```

## Load your source files into `ghci`

You can load external source files into `ghci` by using the `:load` or `:l` command. Let’s start a `ghci` session and load our `Main.hs` file so that we can use the `c2f` function inside the REPL:

```
% ghci
GHCi, version 7.10.2: http://www.haskell.org/ghc/ :? for help
Prelude> :load c2f.hs
[1 of 1] Compiling c2f ( c2f.hs, interpreted )
Ok, modules loaded: c2f.
*Main> c2f 37
98.6
```

Now that our development environment is set up, we'll move on to building a simple app using cabal which is Haskell's package manager.

---

# 2. Build and package a simple app using cabal

## Introducing Cabal

Cabal (Common Architecture for Building Applications and Libraries) is a system for building and packaging Haskell libraries and programs. Think of it as Haskell’s equivalent of Python’s `pip`, Node’s `npm` and Scala’s `sbt`.

Before we get started, make sure that you have `cabal` installed and on the system path:

```
% cabal — version
cabal-install version 1.22.6.0
using version 1.22.4.0 of the Cabal library
```

## Create a cabal app

Cabal makes it easy to build, package and test your application. It’s also straightforward to add dependencies on other libraries that cabal will fetch for you.

It is possible that different Haskell apps have dependencies on different versions of the same library. To prevent conflicts in your global Haskell environment, it’s a good idea to create a sandbox for your app. Cabal will then install the dependent libraries inside the sandbox.

```
% mkdir cabal-example
% cd cabal-example
% cabal sandbox init
Writing a default package environment file to
/Users/psingh/tmp/haskell/eac-articles/cabal
example/cabal.sandbox.config
Creating a new sandbox at
/Users/psingh/tmp/haskell/eac-articles/cabal-example/.cabal-sandbox
```

Now, we can initialise our app. The easiest way to create a cabal app is using the `cabal init` command. Follow the prompts and cabal will generate the config file for you. Here’s an example interaction on my system:

```
% cabal init
Package name? [default: cabal-example]
Package version? [default: 0.1.0.0]
Please choose a license:
* 1) (none)
2) GPL-2
3) GPL-3
4) LGPL-2.1
5) LGPL-3
6) AGPL-3
7) BSD2
8) BSD3
9) MIT
10) ISC
11) MPL-2.0
12) Apache-2.0
13) PublicDomain
14) AllRightsReserved
15) Other (specify)
Your choice? [default: (none)] 9
Author name? [default: Parambir Singh]
Maintainer email? [default: user@domain.com]
Project homepage URL?
Project synopsis?
Project category:
* 1) (none)
2) Codec
3) Concurrency
4) Control
5) Data
6) Database
7) Development
8) Distribution
9) Game
10) Graphics
11) Language
12) Math
13) Network
14) Sound
15) System
16) Testing
17) Text
18) Web
19) Other (specify)
Your choice? [default: (none)]
What does the package build:
1) Library
2) Executable
Your choice? 2
What is the main module of the executable:
* 1) Main.hs (does not yet exist)
2) Main.lhs (does not yet exist)
3) Other (specify)
Your choice? [default: Main.hs (does not yet exist)] 1
What base language is the package written in:
* 1) Haskell2010
2) Haskell98
3) Other (specify)
Your choice? [default: Haskell2010] 1
Include documentation on what each field means (y/n)? [default: n] n
Source directory:
* 1) (none)
2) src
3) Other (specify)
Your choice? [default: (none)] 1
Guessing dependencies…
Generating LICENSE…
Generating Setup.hs…
Generating cabal-example.cabal…
Warning: no synopsis given. You should edit the .cabal file and add 
one.
You may want to edit the .cabal file and add a Description field.
```

You will now have a `cabal-example.cabal` file in your source directory. Remember that we selected `Main.hs` as our main module during the `cabal init` process. Add a `Main.hs` file now to the root of your source folder with the following content:

```haskell
c2f c = (c * 9/5) + 32
main = do
  print (c2f 37)
```

## Running your cabal app (cabal run)

To run your app, use the `cabal run` command:

```
% cabal run
Package has never been configured. Configuring with default flags. If this fails, please run configure manually.
Resolving dependencies…
Configuring cabal-example-0.1.0.0…
Preprocessing executable ‘cabal-example’ for cabal-example-0.1.0.0…
[1 of 1] Compiling Main ( Main.hs, dist/build/cabal-example/cabal-example-tmp/Main.o )
Linking dist/build/cabal-example/cabal-example …
Running cabal-example…
98.6
```

## Other useful cabal commands

### `cabal clean`

Removes the generated artifacts (executables etc.)

```
% cabal clean
cleaning…
cabal build
Compiles and links your source code to generate the executable.
% cabal build
Package has never been configured. Configuring with default flags. If this fails, please run configure manually.
Resolving dependencies…
Configuring cabal-example-0.1.0.0…
Building cabal-example-0.1.0.0…
Preprocessing executable ‘cabal-example’ for cabal-example-0.1.0.0…
[1 of 1] Compiling Main ( Main.hs, dist/build/cabal-example/cabal-example-tmp/Main.o )
Linking dist/build/cabal-example/cabal-example …
Run the generated executable:
% ./dist/build/cabal-example/cabal-example
98.6
```

### `cabal install`

Installs the generated artifact into the cabal sandbox. This is somewhat similar to `mvn install` in Java world.

```
% cabal install
Resolving dependencies…
Notice: installing into a sandbox located at
/Users/psingh/tmp/haskell/eac-articles/cabal-example/.cabal-sandbox
Configuring cabal-example-0.1.0.0…
Building cabal-example-0.1.0.0…
Installed cabal-example-0.1.0.0
```

### `cabal repl`

Starts a `ghci` session for your project (i.e. with your modules loaded). You can easily test your functions here.

```
% cabal repl
Preprocessing executable ‘cabal-example’ for cabal-example-0.1.0.0…
GHCi, version 7.10.2: http://www.haskell.org/ghc/ :? for help
[1 of 1] Compiling Main ( Main.hs, interpreted )
Ok, modules loaded: Main.
*Main> c2f 37 — call the c2f function defined in our Main.hs file
98.6
*Main> main — yes, this is the main function we defined in Main.hs
98.6
*Main>
```

### cabal update

Downloads the latest package list for cabal. It’s useful to run `cabal update` before running `cabal init` or `cabal install` to make sure that cabal has knowledge about the latest packages

```
% cabal update
Downloading the latest package list from hackage.haskell.org
```

## Next…

We’ll write a simple web app using the Scotty web framework for Haskell.

---

# 3. Run a basic Haskell web app using Scotty


## Introducing [Scotty](https://github.com/scotty-web/scotty)

There are many web frameworks available for Haskell: Yesod, Snap, Scotty, etc. I chose Scotty over others as it seemed easier to get started with.

We’ll write a simple web service that responds to various HTTP request types (GET, PUT, POST, DELETE). We’ll see how to get request headers, path parameters and form fields and how to respond with plain-text, html or JSON response.

## Initialise Cabal

Let’s initialise a cabal app for our web service. I’ve mostly chosen the default options. Two notable exceptions include:
license: 9) MIT
source directory: 2) server

```
% mkdir scotty-webapp-example
% cd scotty-webapp-example
% cabal sandbox init
Writing a default package environment file to
/Users/psingh/tmp/haskell/eac-articles/scotty-webapp-example/cabal.sandbox.config
Using an existing sandbox located at
/Users/psingh/tmp/haskell/eac-articles/scotty-webapp-example/.cabal-sandbox
% cabal init
Package name? [default: scotty-webapp-example]
Package version? [default: 0.1.0.0]
...
...
```

## Write the server code

Since we told `cabal` earlier that our main module for the executable will be `Main.hs`, and that it will live inside the `server` folder, let’s add `server/Main.hs` file to our source.

```haskell
{-# LANGUAGE OverloadedStrings #-}
import Web.Scotty
import Network.HTTP.Types
 
main = scotty 3000 $ do
  get "/" $ do                         -- handle GET request on "/" URL
    text "This was a GET request!"     -- send 'text/plain' response
  delete "/" $ do
    html "This was a DELETE request!"  -- send 'text/html' response
  post "/" $ do
    text "This was a POST request!"
  put "/" $ do
    text "This was a PUT request!"
```

As you can probably figure out, this code will start a server on port 3000 and:

- For a `GET` request on the `/` path, the server will respond with an HTML response with the content “This was a GET request!”
- For a `DELETE` request on the `/` path, the server will respond with a ‘plain-text’ response with the content “This was a DELETE request!”

## Add dependencies

Let’s add a dependency for `scotty` and `http-types` libraries in our cabal file.

This is how the build-depends segment of the scotty-webapp-example.cabal files looks right now:

```haskell
build-depends: base >=4.8 && <4.9
```

Change it to:

```haskell
build-depends: base >=4.8 && <4.9
             , scotty
             , http-types
```

Next, run `cabal install` to add the dependencies into your sandbox followed by `cabal run` to run the server.

```
% cabal install
Resolving dependencies…
Notice: installing into a sandbox located at
/Users/psingh/tmp/haskell/eac-articles/scotty-webapp-example/.cabal-sandbox
Configuring ansi-terminal-0.6.2.3…
Configuring appar-0.1.4…
…
…
% cabal run
Package has never been configured. Configuring with default flags. If this fails, please run configure manually.
Resolving dependencies…
Configuring scotty-webapp-example-0.1.0.0…
Preprocessing executable ‘scotty-webapp-example’ for
scotty-webapp-example-0.1.0.0…
[1 of 1] Compiling Main ( server/Main.hs, dist/build/scotty-webapp-example/scotty-webapp-example-tmp/Main.o )
Linking dist/build/scotty-webapp-example/scotty-webapp-example …
Running scotty-webapp-example…
Setting phasers to stun… (port 3000) (ctrl-c to quit)
```

The server will be running now on port 3000. Let’s verify that (using the excellent [http](https://github.com/jkbrzt/httpie) tool:

```
% http delete :3000
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Date: Mon, 14 Sep 2015 05:44:57 GMT
Server: Warp/3.1.3
Transfer-Encoding: chunked
This was a DELETE request!
```

Great!

## Handling more complex requests

Let’s add a few more handlers to handle different kinds of requests. Add the following to `server/Main.hs`:

```haskell
-- set a header:
post "/set-headers" $ do
 status status302  -- Respond with HTTP 302 status code
 setHeader "Location" "http://www.google.com.au"
-- named parameters:
get "/askfor/:word" $ do
  w <- param "word"
  html $ mconcat ["<h1>You asked for ", w, ", you got it!</h1>" ]
-- unnamed parameters from a query string or a form:
post "/submit" $ do  -- e.g. http://server.com/submit?name=somename
 name <- param "name"
 text name
-- match a route regardless of the method
matchAny "/all" $ do
 text "matches all methods"
-- handler for when there is no matched route
-- (this should be the last handler because it matches all routes)
notFound $ do
 text "there is no such route."
```

## Encode/Decode JSON

Most web services these days interact via JSON. Haskell provides a type safe way to encode/decode JSON strings using the [Aeson]() library.

### Defining the model

Let’s create an `Article` data type that could represent a news article for example. An article consists of 3 fields: anInteger id, a Text title and a Text bodyText. By making Article an instance of `FromJSON` and `ToJSON` typeclasses, we can use Aeson library for converting between JSON strings and Article objects. Add the following code to the file `server/Article.hs`:

```haskell
{-# LANGUAGE OverloadedStrings #-}
module Article where
import Data.Text.Lazy
import Data.Text.Lazy.Encoding
import Data.Aeson
import Control.Applicative
 
-- Define the Article constructor
-- e.g. Article 12 "some title" "some body text"
data Article = Article Integer Text Text -- id title bodyText
     deriving (Show)
 
 
-- Tell Aeson how to create an Article object from JSON string.
instance FromJSON Article where
     parseJSON (Object v) = Article <$>
                            v .:? "id" .!= 0 <*> -- the field "id" is optional
                            v .:  "title"    <*>
                            v .:  "bodyText"
 
 
-- Tell Aeson how to convert an Article object to a JSON string.
instance ToJSON Article where
     toJSON (Article id title bodyText) =
         object ["id" .= id,
                 "title" .= title,
                 "bodyText" .= bodyText]
```

We’ll need to add a couple of routes to our Scotty router function to handle encoding and decoding Article types:

```haskell
main = scotty 3000 $ do
 
  -- get article (json)
  get "/article" $ do
    json $ Article 13 "caption" "content" -- Call Article constructor and encode the result as JSON
 
  -- post article (json)
  post "/article" $ do
    article <- jsonData :: ActionM Article -- Decode body of the POST request as an Article object
    json article                           -- Send the encoded object back as JSON
```

We’ll also need to add a couple of dependencies to `scotty-webapp-example.cabal` file:

```haskell
build-depends: base >=4.8 && <4.9
             , scotty
             , http-types
             , text
             , aeson
```

### Test JSON encoding/decoding

Let’s fire up Scotty and see if it can handle JSON properly:

#### GET /article

```
% http get :3000/article
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Mon, 14 Sep 2015 06:51:17 GMT
Server: Warp/3.1.3
Transfer-Encoding: chunked
{
  “bodyText”: “content”,
  “id”: 13,
  “title”: “caption”
}
```

#### POST /article

```
% http post :3000/article id:=23 title=”new caption” bodyText=”some content”
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Mon, 14 Sep 2015 06:56:57 GMT
Server: Warp/3.1.3
Transfer-Encoding: chunked
{
  “bodyText”: “some content”,
  “id”: 23,
  “title”: “new caption”
}
```

## Next...

We know how to build & run a basic web service in Haskell that can talk JSON. In the next section we'll dockerize it!

---

# 4. Dockerize your scotty-webapp

This section assumes that you have docker installed on your system. Please go to https://docs.docker.com/installation/ if you need to install Docker on your machine.

## Write a Dockerfile

We'll base our docker image on the official haskell image. The image has GHC and Cabal available so we'll copy our source files and use cabal install to build our executable.

```Docker
FROM haskell:7.10
 
RUN cabal update
 
 
# Add .cabal file
ADD ./scotty-webapp-example.cabal /opt/server/scotty-webapp-example.cabal
 
 
# Docker will cache this command as a layer, freeing us up to
# modify source code without re-installing dependencies
RUN cd /opt/server && cabal install --only-dependencies -j4
 
 
# Add and Install Application Code
ADD ./server /opt/server/server
ADD ./LICENSE /opt/server/LICENSE
RUN cd /opt/server && cabal install
 
 
# Add installed cabal executables to PATH
ENV PATH /root/.cabal/bin:$PATH
 
 
# Default Command for Container
WORKDIR /opt/server
CMD ["scotty-webapp-example"]
```

## Start Docker

I’m assuming you have docker installed and set up correctly. On OSX, you need to run the following two commands to get docker up and running:

```
% boot2docker up
Waiting for VM and Docker daemon to start…
……………..ooooooooo
Started.
Writing /Users/psingh/.boot2docker/certs/boot2docker-vm/ca.pem
Writing /Users/psingh/.boot2docker/certs/boot2docker-vm/cert.pem
Writing /Users/psingh/.boot2docker/certs/boot2docker-vm/key.pem
To connect the Docker client to the Docker daemon, please set:
  export DOCKER_HOST=tcp://192.168.59.103:2376
  export DOCKER_CERT_PATH=/Users/psingh/.boot2docker/certs/boot2docker-vm
  export DOCKER_TLS_VERIFY=1
Or run: `eval “$(boot2docker shellinit)”`% `eval “$(boot2docker shellinit)”`
Writing /Users/psingh/.boot2docker/certs/boot2docker-vm/ca.pem
Writing /Users/psingh/.boot2docker/certs/boot2docker-vm/cert.pem
Writing /Users/psingh/.boot2docker/certs/boot2docker-vm/key.pem
```
## Build Docker Image

```
% docker build -t scotty-webapp-docker .
Sending build context to Docker daemon 136.6 MB
Step 0 : FROM haskell:7.10
…
…
Step 9 : CMD scotty-webapp-example
 — -> Running in 96b20daed3cc
 — -> cc46fe7ce920
Removing intermediate container 96b20daed3cc
Successfully built cc46fe7ce920
```

## Run the Docker Image

```
% docker run -i -t -p 3000:3000 scotty-webapp-docker
Setting phasers to stun… (port 3000) (ctrl-c to quit)
```

So, our web app is running inside the docker container!

Let’s test it now. On OSX, Docker runs inside a virtual machine. So, accessing localhost:3000 won’t work. Use the following command instead:

```
% open http://$(boot2docker ip):3000
```

We can also test our web app using the [httpie](https://httpie.io/) client:

```
% http $(boot2docker ip):3000
HTTP/1.1 200 OK
Content-Type: text/plain; charset=utf-8
Date: Mon, 14 Sep 2015 23:37:46 GMT
Server: Warp/3.1.3
Transfer-Encoding: chunkedThis was a GET request!% http $(boot2docker ip):3000/article
HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Date: Mon, 14 Sep 2015 23:38:27 GMT
Server: Warp/3.1.3
Transfer-Encoding: chunked{
  “bodyText”: “content”,
  “id”: 13,
  “title”: “caption”
}
```

## Source

The complete source for the final application is available on [github](https://github.com/parambirs/hello-scotty/tree/with-json-types)

---

# Finally

I hope you found this tutorial useful. Please let me know if something didn’t work for you or if I missed documenting any step.