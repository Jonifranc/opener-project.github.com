---
layout: default
title: Tokenizer
sidebar: documentation
webservice: http://opener.olery.com/tokenizer
repository: https://github.com/opener-project/tokenizer

---



<div id='readme'></div>

Introduction
------------

The tokenizer tokenizes a text into sentences and words. 

### Confused by some terminology?

This software is part of a larger collection of natural language processing tools known as "the OpeNER project". You can find more information about the project at [the OpeNER portal](http://opener-project.github.io). There you can also find references to terms like KAF (an XML standard to represent linguistic annotations in texts), component, cores, scenario's and pipelines.

Quick Use Example
-----------------

Installing the tokenizer can be done by executing:

    gem install tokenizer

Please bare in mind that all components in OpeNER take KAF as an input and
output KAF by default.


### Command line interface

You should now be able to call the tokenizer as a regular shell
command: by its name. Once installed the gem normally sits in your path so you can call it directly from anywhere.

Tokenizing some text:

    echo "This is English text" | tokenizer -l en --no-kaf
    
Will result in

    <?xml version="1.0" encoding="UTF-8" standalone="no"?>
    <KAF version="v1.opener" xml:lang="en">
      <kafHeader>
        <linguisticProcessors layer="text">
          <lp name="opener-sentence-splitter-en" timestamp="2013-05-31T11:39:31Z" version="0.0.1"/>
          <lp name="opener-tokenizer-en" timestamp="2013-05-31T11:39:32Z" version="1.0.1"/>
        </linguisticProcessors>
      </kafHeader>
      <text>
        <wf length="4" offset="0" para="1" sent="1" wid="w1">This</wf>
        <wf length="2" offset="5" para="1" sent="1" wid="w2">is</wf>
        <wf length="7" offset="8" para="1" sent="1" wid="w3">English</wf>
        <wf length="4" offset="16" para="1" sent="1" wid="w4">text</wf>
      </text>
    </KAF>

  The available languages for tokenization are: English (en), German (de), Dutch (nl), French (fr), Spanish (es), Italian (it)

#### KAF input format

The tokenizer is capable of taking KAF as input, and actually does so by default. You can do so like this:

    echo "<?xml version='1.0' encoding='UTF-8' standalone='no'?><KAF version='v1.opener' xml:lang='en'><raw>This is what I call, a test!</raw></KAF>" | tokenizer

Will result in

    <?xml version="1.0" encoding="UTF-8" standalone="no"?>
    <KAF version="v1.opener" xml:lang="en">
      <kafHeader>
        <linguisticProcessors layer="text">
          <lp name="opener-sentence-splitter-en" timestamp="2013-05-31T11:39:31Z" version="0.0.1"/>
          <lp name="opener-tokenizer-en" timestamp="2013-05-31T11:39:32Z" version="1.0.1"/>
        </linguisticProcessors>
      </kafHeader>
      <text>
        <wf length="4" offset="0" para="1" sent="1" wid="w1">this</wf>
        <wf length="2" offset="5" para="1" sent="1" wid="w2">is</wf>
        <wf length="2" offset="8" para="1" sent="1" wid="w3">an</wf>
        <wf length="7" offset="11" para="1" sent="1" wid="w4">english</wf>
        <wf length="4" offset="19" para="1" sent="1" wid="w5">text</wf>
      </text>
    </KAF>

If the argument -k (--kaf) is passed, then the argument -l (--language) is ignored.

### Webservices

You can launch a language identification webservice by executing:

    tokenizer-server

This will launch a mini webserver with the webservice. It defaults to port 9292, so you can access it at <http://localhost:9292>.

To launch it on a different port provide the `-p [port-number]` option like this:

    tokenizer-server -p 1234

It then launches at <http://localhost:1234>

Documentation on the Webservice is provided by surfing to the urls provided above. For more information on how to launch a webservice run the command with the ```-h``` option.


### Daemon

Last but not least the tokenizer comes shipped with a daemon that can read jobs (and write) jobs to and from Amazon SQS queues. For more information type:

    tokenizer-daemon -h

Description of dependencies
---------------------------

This component runs best if you run it in an environment suited for OpeNER components. You can find an installation guide and helper tools in the [OpeNER installer](https://github.com/opener-project/opener-installer) and [an installation guide on the Opener Website](http://opener-project.github.io/getting-started/how-to/local-installation.html)

At least you need the following system setup:

### Dependencies for normal use:

* Perl 5
* MRI 1.9.3

### Dependencies if you want to modify the component:

* Maven (for building the Gem)


Language Extension
------------------

The tokenizer module is a wrapping around a Perl script, which performs the actual tokenization based on rules (when to break a character sequence). The tokenizer already supports a lot of languages. Have a look to the core script to figure out how to extend to new languages.

The Core
--------

The component is a fat wrapper around the actual language technology core. The core is a rule based tokenizer implemented in Perl. You can find the core technologies in the following repositories:

* [tokenizer-base](http://github.com/opener-project/tokenizer-base)

Where to go from here
---------------------

* [Check the project website](http://opener-project.github.io)
* [Checkout the webservice](http://opener.olery.com/tokenizer)

Report problem/Get help
-----------------------

If you encounter problems, please email <support@opener-project.eu> or leave an issue in the 
[issue tracker](https://github.com/opener-project/tokenizer/issues).


Contributing
------------

1. Fork it ( http://github.com/opener-project/tokenizer/fork )
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request


<div id='reference'></div>

## Reference

### Command Line Interface

#### Examples:

##### Provide subexamples

Tokenizing KAF:

```
echo "<?xml version='1.0' encoding='UTF-8' standalone='no'?><KAF version='v1.opener' xml:lang='en'><raw>This is what I call, a test!</raw></KAF>" | tokenizer
```

Tokenizing some text:

```
echo "This is English text" | tokenizer -l en --no-kaf
```

### Webservice

You can launch a webservice by executing:

```
tokenizer-server
```

After launching the server, you can reach the webservice at
<http://localhost:9292>.

The webservice takes several options that get passed along to [Puma](http://puma.io), the webserver used by the component. The options are:



    -b, --bind URI                   URI to bind to (tcp://, unix://, ssl://)
    -C, --config PATH                Load PATH as a config file
        --control URL                The bind url to use for the control server
                                     Use 'auto' to use temp unix server
        --control-token TOKEN        The token to use as authentication for the control server
    -d, --daemon                     Daemonize the server into the background
        --debug                      Log lowlevel debugging information
        --dir DIR                    Change to DIR before starting
    -e, --environment ENVIRONMENT    The environment to run the Rack app on (default development)
    -I, --include PATH               Specify $LOAD_PATH directories
    -p, --port PORT                  Define the TCP port to bind to
                                     Use -b for more advanced options
        --pidfile PATH               Use PATH as a pidfile
        --preload                    Preload the app. Cluster mode only
        --prune-bundler              Prune out the bundler env if possible
    -q, --quiet                      Quiet down the output
    -R, --restart-cmd CMD            The puma command to run during a hot restart
                                     Default: inferred
    -S, --state PATH                 Where to store the state details
    -t, --threads INT                min:max threads to use (default 0:16)
        --tcp-mode                   Run the app in raw TCP mode instead of HTTP mode
    -V, --version                    Print the version information
    -w, --workers COUNT              Activate cluster mode: How many worker processes to create
        --tag NAME                   Additional text to display in process listing
    -h, --help                       Show help



### Daemon

The daemon has the default OpeNER daemon options. Being:

    Usage: tokenizer-daemon <start|stop|restart> [options]

When calling tokenizer without <start|stop|restart> the daemon will start as a foreground process

~~~~
Daemon options:
    -i, --input QUEUE_NAME           Input queue name
    -o, --output QUEUE_NAME          Output queue name
        --batch-size COUNT           Request x messages at once where x is between 1 and 10
        --buffer-size COUNT          Size of input and output buffer. Defaults to 4 * batch-size
        --sleep-interval SECONDS     The interval to sleep when the queue is empty (seconds)
    -r, --readers COUNT              number of reader threads
    -w, --workers COUNT              number of worker thread
    -p, --writers COUNT              number of writer / pusher threads
    -l, --logfile, --log FILENAME    Filename and path of logfile. Defaults to STDOUT
    -P, --pidfile, --pid FILENAME    Filename and path of pidfile. Defaults to /var/run/tokenizer.pid
        --pidpath DIRNAME            Directory where to put the PID file. Is Overwritten by --pid if that option is present
        --debug                      Turn on debug log level
        --relentless                 Be relentless, fail fast, fail hard, do not continue processing when encountering component errors
~~~~

#### Environment Variables

These daemons make use of Amazon SQS queues and other Amazon services. The access to these services and other environment variables can be configured using a .opener-daemons-env file in the home directory of the current user.

It is also possible to provide the environment variables directly to the daemon.

For example:

```
AWS_REGION='eu-west-1' tokenizer start [other options]
```

We advise to have the following environment variables available: 

* AWS_ACCESS_KEY_ID
* AWS_SECRET_ACCESS_KEY
* AWS_REGION


### Languages

The tokenizer currently supports the following languages:

* Dutch (nl)
* English (en)
* French (fr)
* German (de)
* Italian (it)
* Spanish (es)
