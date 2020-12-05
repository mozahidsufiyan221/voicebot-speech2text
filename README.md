# voicebot-speech2text
https://mozahidsufiyan222-96951.medium.com/voicebot-ivr-build-your-own-call-centre-with-twilio-python-flask-554bd7e1e87f
Interactive-Voice-Response-bot(IVR) and Natural Language Processing
Image for post
Every company or organization are pretending to the clients. So the client satisfaction is very important. Therefore, maintaining a call center is necessary. But because of the cost, which takes to establish a physical call center is high, Most of the people are not establish a call center for their businesses. So to fulfill this requirement, Jeff Lawson and his colleges have introduced Twilio in 2008.
Twilio is a cloud communications platform as a service company based in San Francisco, California. Twilio allows software developers to programmatically make and receive phone calls, send and receive text messages, and perform other communication functions using its web service APIs.
From this tutorial, we are going to set up a simple call center application from the ready-made project which was implemented by me using Twilio and Python Flask. In here what I am doing is guide you to change configurations of the application, So you can run your application.
Below is a screenshot of a completed project. This tutorial will direct you to build up an operational client application. So you can customize your web page to your requirements.
Python Ngrok configuration
Before starting the tutorial you should have to make sure your machine is fulfilling the following requirements.
Python 3.6.7, Pip Install flask twilio
Ngrok (ngrok secure introspectable tunnels to localhost webhook development tool and debugging tool.)
A Twilio Account Signed Up (Twilio is a cloud communications platform as a service company based in San Francisco, California. Twilio allows software developers to programmatically make and receive phone calls, send and receive text messages, and perform other communication functions using its web service APIs.)
Here is the project repository stored in GitHub. Clone the project into your local machine. And open it up with your favorite IDE. For this tutorial, I have used JetBrainPycharm.
Before starting the configurations you should have some basic idea regarding the following properties.
Buy and configure a phone number
In the Twilio Console, you can search for and buy phone numbers in countries around the world. Numbers that have the Voice capability can make and receive voice phone calls from just about anywhere on the planet.
Image for post
Once you purchase a number, you’ll need to configure that number to send a request to your web application. This callback mechanism is called a webhook. This can be done on the number’s configuration page.
Image for post
A Twilio phone number in — you can get one here
Your REST API Key information needed to create an Access Token — create one here.
Image for post
Let’s start the Ngrok configurations.
Download Ngrok file
Please download Ngrok from the site below for windows.
Image for post
Connect your Ngrok account
Running this command will add your authtoken to the default ngrok.yml configuration file. This will grant you access to more features and longer session times. Running tunnels will be listed on the status page of the dashboard.
$ ngrok authtoken 1l4Pz3EQAYufyrrHKYBAo4yQoDg
Write Python to Handle the Incoming Phone Call
Now comes the fun part-writing code that will handle an incoming HTTP request from Twilio! Our code will dictate what happens when our phone number receives a call by responding with TwiML.
Respond to an incoming call with TwiML
On your server, you can respond to a Twilio webhook request with TwiML. TwiML is a set of XML tags that tell Twilio how to handle an incoming call.
The TwiML generated by server code
In order for the webhooks in this code sample to work, Twilio must be able to send your web application an HTTP request over the Internet. Of course, that means your application needs to have a URL or IP address that Twilio can reach.
In production, you probably have a public URL, but you probably don’t during development. That’s where ngrok comes in. ngrok gives you a public URL for a local port on your development machine, which you can use to configure your Twilio webhooks as described above.
Once ngrok is installed, you can use it at the command line to create a tunnel to whatever port your web application is running on. For example, this will create a public URL for a web application listening on port 5000.
ngrok http 5000
After executing that command, you will see that ngrok has given your application a public URL that you can use in your webhook configuration in the Twilio console.
Image for post
Grab your ngrok public URL and head back to the phone number you configured earlier. Now let’s switch it from using a TwiML Bin to use your new ngrok URL. Don’t forget to append the URL path to your actual TwiML logic! (“http://<your ngrok subdomain>.ngrok.io/voice” for example)
Image for post
Bridge the URL
You should have to run it with ngork to locally check it. So that, Open the terminal and run ngrok http 5000 command. So the terminal will display ngrok forwarded URL. Copy it and navigate to it from the browser. Refer the following screenshot.
Image for post
ngrok session status
so take e.g https://33be109c3ce2.ngrok.io from above and paste below within Twilio, in your case different HTTPS link will provided by Ngrok
Twilio application configurations
Webhook Description
Go to #phone numbers as shown below from three dots provided in the circle.
Image for post
Image for post
This is the final step. All the application side changes are almost over. Now you should have to set up the Twilio configurations. Paste HTTPS link provided by Ngrok on the below-given description on phone number and scroll down and you will see the voice and update the voice URL with ngrok forwaded URL + /voice.
e.g https://33be109c3ce2.ngrok.io/voice
Image for post
similarly, you fill the same HTTPS link into the messaging section, if you just using voice then you don't need to provide.
Respond to incoming calls in your web application
In this article, I’ll show you how to use Programmable Voice to respond to incoming phone calls in your Python web application. Code on your server can decide what a caller hears when they dial the number you’ve bought or ported to Twilio.
The code snippets in this guide are written using the Flask web framework and the Twilio Python SDK. Let’s get started!
Image for post
Twilio makes answering a phone call as easy as responding to an HTTP request. When a Twilio phone number receives an incoming call, Twilio will send an HTTP request to your web application, asking for instructions on how to handle the call. Your web application will respond with an XML document containing TwiML. That TwiML contains the instruction that Twilio will follow to say some arbitrary text, play an MP3 file, make a recording and much more.
Create Python responses to incoming calls
In the example above, we returned pre-defined TwiML in response to the incoming call. The real power of using webhooks like this is executing dynamic code (based on the information Twilio sends to your application) to change what you present to the user on the other end of the phone call. You could query your database, reference a customer’s phone number in your CRM, or execute any kind of custom logic before determining how to respond to your user.
from flask import Flask
from twilio.twiml.voice_response import VoiceResponse

app = Flask(__name__)


@app.route("/voice", methods=['GET', 'POST'])
def voice():
    """Respond to incoming phone calls with a 'Hello world' message"""
    # Start our TwiML response
    resp = VoiceResponse()

    # Read a message aloud to the caller
    resp.say("hello world!", voice='alice')

    return str(resp)

if __name__ == "__main__":
    app.run(debug=True)
Hopefully, Now your application is running without issue. If you faced any issues post those in the comment section.
Image for post
References
What is a Webhook?
Webhooks are user-defined HTTP callbacks. They are usually triggered by some event, such as receiving an SMS message or an incoming phone call. When that event occurs, Twilio makes an HTTP request (usually a POST or a GET) to the URL configured for the webhook.
To handle a webhook, you only need to build a small web application that can accept the HTTP requests. Almost all server-side programming languages offer some framework for you to do this. Examples across languages include ASP.NET MVC for C#, Servlets and Spark for Java, Express for Node.js, Django and Flask for Python, and Rails and Sinatra for Ruby. PHP has its own web app framework built in, although frameworks like Laravel, Symfony and Yii are also popular.
Whichever framework and language you choose, webhooks function the same for every Twilio application. They will make an HTTP request to a URI that you provide to Twilio. Your application performs whatever logic you feel necessary — read/write from a database, integrate with another API or perform some computation — then replies to Twilio with a TwiML response with the instructions you want Twilio to perform.
What is TwiML?
TwiML is the Twilio Markup Language, which is just to say that it’s an XML document with special tags defined by Twilio to help you build your SMS and voice applications. TwiML is easier shown than explained. Here’s some TwiML you might use to respond to an incoming phone call:
<?xml version="1.0" encoding="UTF-8"?>
<Response>
    <Say>Thanks for calling!</Say>
</Response>
And here’s some TwiML you might use to respond to an incoming SMS message:
<?xml version="1.0" encoding="UTF-8"?>
<Response>
    <Message>We got your message, thank you!</Message>
</Response>
Every TwiML document will have the root <Response> element and within that can contain one or more verbs. Verbs are actions you’d like Twilio to take, such as <Say> a greeting to a caller, or send an SMS <Message> in reply to an incoming message. For a full reference on everything you can do with TwiML, refer to our TwiML API Reference.
