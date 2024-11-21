# Hedy

Hedy, named for ["the most beautiful woman in Europe", who moonlighted as a Nazi-fighting inventor, and in doing so co-invented one of the key technologies that enables pretty much all wireless communication on Earth][hedy lamarr], is an effort to create a publicly available cloud infrastructure that can run on every day home computing devices.

Imagine if all your devices that sit in your home idle most of the day vampiring power from the wall, could actually be making you money. 
Your printer, your roku, your router, your PS5, etc, could all be used to provide useful services for folks already buying things online.
And when someone buys their groceries through an app that routes through your smart fridge, you get a cut of that transaction. 

That's the idea behind Hedy. 
Indulge me as I layout how we can get there.

## A public cloud

If you've been developing app/website type things, whether backend, frontend, or any of the other jobs that make software possible, you've likely noticed that--at a high enough level--they're basically all the same damn thing.
You make your auth, and your purchase flow, and whatever you're selling probably has some blah blah blah, and you make some client applications and because those apps run on different machines than your backend there's some networking, and hosting, and so on, and because it's hosted on some machine you don't control you throw your code in a container. 

According to the first website that I duckducked, only about 0.4% of humans are programmers, and because that's not a lot, you get buku bucks for understanding what I just wrote, and putting it all together into "a stack."

Let me show you another stack:

![A stack going from Subgrade to Stone Base/sub-base to Asphalt Concrete Rich Bottom Base Layer to Asphalt Concrete Base Layer to Asphalt Concrete Intermediate layer to Asphalt Concrete Surface Layer](https://images.theconversation.com/files/592723/original/file-20240507-16-12l897.png?ixlib=rb-4.1.0&q=45&auto=format&w=1000&fit=clip)

And because asphalt's a pretty well-known term you might have guessed that this stack is a road.
What you may not know is that an asphalt concrete sub-layer is a multi-material aggregate composed of a foamed concrete--[Portland Cement][cement] is a common one--which needs to be rendered at a certain time and temperature before being paved in order to set correctly.

Of course roads are more widely understood, ubiquitous, and under-appreciated in large part because they've been around for millenia. 
The cloud as we know it, on the other hand, isn't even old enough to have a mid-life crisis yet. 
I'd bet that if we jumped back to ancient Sumer, and popped in some Sumerian water hole, we'd hear some opinions on roads.

_My_ opinion on the cloud is that much of the backend stuff should be built in a reusable way, and made easily available to developers.
I'm not alone in this belief.
Backend-as-a-Service (BaaS) platforms like Firebase and Supabase are doing just that.
Of course they charge you for that, and I think that's lame, so I built an alternative called [allyabase][allyabase].

Allyabase is in a nascient stage, but it's sufficient for building a number of things.
It doesn't handle hosting though, and that's what Hedy is for. 
Since allyabase doesn't store personally identifying information (pii), it can be deployed anywhere.

_I_ don't want to deploy it anywhere.
I want people to be able to deploy it wherever _they_ want, and I want them to get _paid_ for providing their unutilized computing power. 
Like electric panels selling power back to the grid, your old kindle could earn you a couple of gallons of gas a month or something. 

By making it open to the public and free to use, it becomes a public cloud anyone can use, just like roads.

Let's see how that can be done.

## The Stack

### Sessionless

So Hedy is an interesting project, but it's part of a much larger effort called Planet Nine, and the central software of Planet Nine is called The Stack.

The Stack is a set of protocols that enable things like Hedy to happen. 
There are six protocols in total, but I'll just talk about two here.

The first issue that needed to be solved was identity.
To do that I made [Sessionless][sessionless], the base of The Stack.

Sessionless is worth a read, but the tl;dr wrt to Hedy is that Sessionless creates a public key infrastructure that allows services to join (or compete with, I don't care) allyabase.
So say someone makes something that grabs the local weather for users, someone else can make an app that uses that too--**without sharing any pii**.

Now I hear you out there.
"If I don't grab pii, how can I ~harass~ market to my users?" 

Well nothing's stopping you from grabbing pii and storing it in some cloud of your own, there's just not room for it in Hedy.
In this weather app example, do you really need to send a user's email back and forth all the time?
The oft given reason for using a cloud like AWS's is scalability.
Do you need to scale user emails?

Sessionless lets you join and leave these services at will, multiple times, from wherever.
And it works in pretty much any client application you can think of.

### MAGIC

Marketplaces have been popular since almost the beginning of the internet.
Through twists and turns we've landed on an interesting model.
What started off with platforms like eBay and Etsy that connected buyers and sellers and processed their transactions has given way to the Shopify model of the platform fading to the background.

And now all shopping around the internet is the same: sign up, load up your cart, checkout, put in the cc deets, add your shipping address.
Shopify processes the payment, and that kicks off your fulfillment. 
With dropshipping that fulfillment's automated too, and now all people need to do is set up all these platforms. 

But if Shopify is processing all these payments, why in the world do _I_ as the user need to put in my info every. single. time. I buy something?

The problem is that the middle systems, the shops, don't have any way of agreeing on who you are.
There are ways of doing that like distributed identities.

The way I want to solve this is by separating the shop from the processor. 
Using Sessionless, a client can save a payment method with a processor, and then use it anywhere that the client interfaces with a shop. 
Of course the shop needs to get paid so you need a way of tying together the user, shop, and processor.

That's what [MAGIC][magic] does.
It lets you chain together authenticated requests with multiple devices.

The cool thing is that the shop doesn't need to be just a website with MAGIC. 
You would be able to purchase via anything that could kick off the chain. 
Check out the MAGIC repo for what that means.

## Paying for the public cloud

The rationale for the cloud is that a cloud managed by a giant tech company will be able to scale better than your startup's [Gilfoyle][gilfoyle] trying to keep a couple racks from melting in the garage.
And that's a good rationale, and if your concern is that your SaaS thing goes viral overnight, and you miss out on some sales because you don't have sophisticated autoscaling set up, this probably isn't the project for you.
If, on the other hand, you think that being able to deploy some hacked together CRUD app should be free and easy, then let's make it so.

Unlike roads, which are paid for collectively by the taxes that people pay, there is no public funding for a public cloud.
That means that running these public cloud machines would cost money due to extra electricity, and that means people won't do it.
But with MAGIC, we can just add any machine involved with a transaction to the transaction chain. 

## So how do we make this happen

Well that's what I was responding to the LinkedIn post for.
I'm fairly confident in my ability to put a machine I own on the internet safely with code that I've written. 
I am not at all confident in my ability to do that with other people's machines.

But one thing I do not want to have happen is have another huge ability gap in tech that makes money.
If Hedy only ends up running on self-hosting enthusiasts home labs, that would be a failure. 
So I'm asking for help from people like me, who are maybe not folks that have built rows of servers in a data center, but want to learn what it would take, so we can evangelize the effort.

The first step to that is saying hello. 
Come find me on discord at https://discord.gg/RQPTevuv. 
I'm planetnineisaspaceship there.




[hedy lamarr]: https://www.history.com/news/hedy-lamarr-inventor-frequency-hopping-wifi
[cement]: https://www.sciencedirect.com/topics/engineering/portland-cement
[allyabase]: https://github.com/planet-nine-app/allyabase
[sessionless]: https://github.com/planet-nine-app/sessionless
[magic]: https://github.com/planet-nine-app/MAGIC
[gilfoyle]: https://youtu.be/QMDf5MSE0LQ



