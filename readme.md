# THIS IS NOT NGROK-NOTIFY package - THIS PACKAGE IS FORKED TO MAKE LOCALTUNNEL-NOTIFY PACKAGE
IT IS ORIGINALLY CREATED BY thisDavej https://thisdavej.com I only modified slightly to make this package work with localtunnel instead of ngrok.

Ngrok free plan has limited connections per minute and you need to create account, get token etc...
With localtunnel service you dont need an account or token you simply download (npm insall localtunnel) the localtunnel and forward your local server with (lt http 80).
Localtunnel is much more easy to use but it still need a way of getting the random url. This package does that. Gets the active server url and sends it using ``less secure app enabled`` gmail account.

Create ngrok tunnel to expose localhost to the web and notify by email with the ngrok URL

See my tutorial [here](https://thisdavej.com/how-to-host-a-raspberry-pi-web-server-on-the-internet-with-ngrok/) for a complete walkthrough including the steps below.

## Install

Linux/macOS

```
$ sudo npm install --unsafe-perm -g ngrok-notify
```

We need to install with the `--unsafe-perm` flag to enable the underlying `ngrok` package to run its postinstall script as root to download and save the ngrok binary in the global `node_modules` folder.  This extra flag is not necessary for Windows installations.

Windows

```
C:\> npm install -g ngrok-notify
```

## Configure

Create a directory to store the configuration files and navigate to it:

```shell
$ mkdir ngrok && cd ngrok
```

Copy the starter configuration files into this directory:

```shell
$ ngrok-notify init
```

Modify your Gmail account (or create a separate Gmail account for sending messages) and [configure it for less secure apps](https://support.google.com/accounts/answer/6010255).  This is needed so the underlying npm module used by ngrok-notify for sending email (Nodemailer) can send email using Gmail.  See also the Nodemailer notes on [using Gmail](https://community.nodemailer.com/using-gmail/).

Update the following configuration files:

- `config.yml` - Add Gmail account to use for sending messages and email recipients
- `.env` - Add password for the Gmail account used to send messages

## Use it

Host a web server on port 8080 (or port of your choice) using [http-server](https://www.npmjs.com/package/http-server), [Express](https://www.npmjs.com/package/express), etc.

Create an ngrok http tunnel to expose port 8080 running on the localhost to the world.

```shell
$ ngrok-notify http 8080 -e
```

An ngrok tunnel on the public Internet will be created and the ngrok URL will be printed to the console.  Additionally, `ngrok-notify` will send you an email with the ngrok URL.

The email can be especially handy if you use ngrok-notify in conjunction with a process manager such as [pm2](http://pm2.keymetrics.io/).  After rebooting your system, you will receive an email with the new ngrok URL without needing to log into the system and start the tunnel manually.

You can also use it to trigger a webhook, along with or instead of sending an email, by configuring an URL and method in the config file and executing the following

```shell
$ ngrok-notify http 8080 -w
```

## Usage

```
$ ngrok-notify --help

  Create ngrok tunnel to expose localhost to the web and notify with ngrok URL

  Usage: ngrok-notify PROTO PORT [-n]
         ngrok-notify init [-f]

  Positional arguments:
    PROTO           Protocol to use in the ngrok tunnel {http,tcp,tls}
    PORT            Port number of the localhost service to expose (e.g. 8080)
    init            Copy starter config files into directory for customizing
  Optional arguments:
     -e, --email     Send an email providing the URL of the ngrok tunnel
     -w, --webhook   Call a webhook providing the URL of the ngrok tunnel as POST params
     -h, --help      Show help
     -v, --version   Display version information
     -f, --force     Overwrite config files in directory if they exist.
  Notes
    Email messages and webhook are sent using the settings in the config.yml file and the
    Gmail password stored in the .env file.

  Examples
    Create ngrok tunnel to expose localhost web server running on port 8080.
    Email is sent with the ngrok URL since "--email" is included.
    $ ngrok-notify http 8080 --email

    Create ngrok tunnel to expose localhost web server running on port 8080,
    but don't send email and send url to webhook instead.
    $ ngrok-notify http 8080 --webhook
```

## Alternatives

This package leverages the excellent [ngrok](https://www.npmjs.com/package/ngrok) package and provides notification capabilities.  If you don't have a need for the notify feature, you may want to consider using the [ngrok](https://www.npmjs.com/package/ngrok) package directly instead.

## License

MIT © [Dave Johnson (thisDaveJ)](http://thisdavej.com)
