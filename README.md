# Add a Call Whisper using Node and the Vonage Voice API

This app used the Vonage Voice APIs to demonstrate how add a "whisper" (a
spoken announcement to only one caller before connecting) to a call using the
`onAnswer` feature.

## Prerequisites

You will need:

* A [free Vonage account](https://dashboard.nexmo.com/sign-up)
* The [Vonage CLI](https://github.com/Vonage/vonage-cli) installed
* A way to run a local server on a public port, for example [Ngrok](https://ngrok.com/).


## Installation

```sh
git clone https://github.com/Vonage-Community/sample-voice-nodejs-call-whisper
cd node-call-whispher
npm install
```

## Setup

### Buy Numbers & Create Application

To run this application we need to buy 2 numbers, set up an application, and tie the numbers to this application.

1. create an application
```sh
voange apps create "Whisper System"
```

2. Add voice capabilities to the application
```sh
vonage apps capabilities update voice 00000000-0000-0000-0000-000000000000 --voice-inbound-url http://your.domain/answer_inbound --voice-event-url http://your.domain/event
```

3. Buy 2 numbers (run this command for each number):
```sh
vonage numbers buy US 16127779311
```

4. Link the numbers to the application ID (once for each number):

```sh
vonage apps numbers link 00000000-0000-0000-0000-000000000000 16127779311
```

### Run Server

The next step is to set up all of our variables in a `.env` file. You can
start off by copying the example file.

```sh
mv .env.example .env
```

Fill in the values in `.env` as appropriate, where `INBOUND_NUMBER_1` and
`INBOUND_NUMBER_2` are the numbers you just purchased, `CALL_CENTER_NUMBER` is
the number you want them to direct to. Finally, `DOMAIN` is the public
domain or hostname your server is available on.

With this in place you can start the server.

```sh
npm start
```

The application should be available on <http://localhost:5000>. For this to
work full though, make sure to expose your server on a public domain (e.g.
`your.domain` in the example above) using a tool like
[Ngrok](https://ngrok.com/).

## Using the App

To use the app you need two phones, or a phone and something like WhatsApp to
make the first call.

With your server running, call either of the 2 numbers you purchased. Vonage
will then make a call to `http://your.domain/answer_inbound` which puts the
inbound call on hold. A call is then made to the `CALL_CENTER_NUMBER`(your
other phone) where a message is played regarding the nature of the call before
both parties are connected to the same conference.

## More Information

For a more detailed writeup, see the
[tutorial on the Vonage Developer Portal](https://developer.vonage.com/en/voice/voice-api/guides/call-whisper).
