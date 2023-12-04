# home-chain
On-chain governance for your community

## Introduction:
**Home-line** is a recently built online community management system.
- 4 years of development work
- fully open source
- community driven
- goal is to promote liquid democracy in small communities,
- make community management convenient and fun.

Currently used in Condominium buildings in Hungary (around 500 active users atm).
Condominiums are a perfect fit for such system. They are a community of flat owers. They have lots of issues to track, decisions to make and agree on budgets. Trancparency of the money movements is very immportant. They can benefit higly from the online liquid democracy process. Decision making is much easier that way, compared to long annual owners meetings.
Hence the name **Home-line**. "Manage your home online from home, conveniently."

But **Home-line** can manage all types of communities (namely company, foundation, simple co-operative)
and supports multiple languages (currently hun and eng, but new translation files can be added easily)

The system is designed to fully handle all aspects of a community's management from one place
- Discussion board
- Decision making
- Issue tracking
- Accounting
- Document store

### Live Demo:
[https://demo.honline.hu](https://honline.hu)
(press DEMO button at the top)

### Code: (fully open source)
[https://github.com/edmo/lidehouse](https://github.com/edemo/Lidehouse)
(Lide house means liquid democracy house)

## The proposed project:
Home-line is a liquid democracy **online** governance system.
Let's convert it to a parachain, to make it an **on-chain** governance system.
Hence the new name Home-chain.

"Home" can be your condo, but it can also be your ditrict or your country.
The problems that need to be managed are very similar on all scales.
- Discussion board (Discussing topics the people are interested in, making suggestions on what to change)
- Decision making (Vote on proposals and budgets)
- Issue tracking (Track the timeline and current status of the implementation of an accepted proposal)
- Accounting (Create and track budgets, create official reports for the citizens and for the tax man)
- Document store (Store the official documents of the community in one place, where everyone can access them)
So lets run our home, our community on a liquid democracy platform, on the chain.

Why make it a DAO?
- Decentralized execution makes it less dependant on the officials (the condominum manager)
  Communities (Condominums are other types) benefit largely from decentralization. A corrupt offical cannot  silence the comments and proposals of the opposing members if the system is run on a chain instead of a server run by the offical himself.
- Anonymous voting is possible
  In such a way that nobody see each other's votes and the manager does not see anybody's vote either, while everybody can still check that their vote is correctly counted into the results. The current online version only supports public votinigs and the manager has to know everybody's votes because of the client-server architecture, he is the one adding them up.
- Communities can start issuing their own local currency
  The community can decide to create a locally accepted currency that the members can use to pay each other for services provided by each other for each other. Example: I have time to babysit your kids tonight,  I receive some ocal tokens, and next week you help giving my son some math lessons.
  Local economies can be born, where the local currency is baced by local's services.

### Why a parachain?
Polkadot is a perfect fit for such a governance and community  management sytem
- Small communities can benefit from the strong security provided by the large Polkadot ecosystem
- Home-chain can use elements of Polkadot OpenGov (tracks, approval curves, etc)
  The current version uses a simple delegation system, and number of vote evalulation techniques, like single choice, multiple chopice, grading and Condorcet method, but the tracks and approval curves of OpenGov could enhance the sophisticatedness of the decision making process.
- Hopefully some features of Home-chain can be integrated into pallets and offer other para-chains the opportunity to use these community management tools as well. Sharing is caring.
  Think of Home-chain as a very smart wallet, that in addition to the traditonal governance, has features such as fully compliant accounting reports and issue tracking. Think of it as mixing the features of Nova wallet with a sophisticated CRM system, enjoying the benefits of both from within the same app.

## Technical implementation:

Home-line is a meteorjs system. [https://www.meteor.com](https://www.meteor.com)

It is a server based SPA (Single Page Appilcation)
- Business object are a json objects, database is a mongo db
  example:
- Users can call Methods on the db
  example:

How to convert the mmeteorjs app into a parachain:
It is pretty straightforward to convert a meteorjs app to a chain!
- The UI of the current Home-line appication can be kept as it is
- But the users instead of calling methods on the server they submit their method calls to the network
- Para-chain blocks are built from these method calls
- Para-chain validators execute the code in the method for the desired state transitions

The process is pretty straightforward, each method call has to be converted into a contract call.
If the prize is won, the resources will be used to start the development work.

-----------------------------------------------------------------

# AI extension to Liquid Democracy 

How can AI assist a liquid democracy system:

Fighting voter apathy:
AI can know us and our wishes better than ourselves by analizing our activity.
For exampe on Social media, just by looking at a dozen likes, it can make recommendations what we will like in the future very accurately.
Similarly an AI agent looking at a few of our votes in the liquid democracy system can give very good recomendation who should we delegate to, by finding those whose voting record matches ours.

Warning !!

1. Only for public votings.
   No system should get access to our private voting records!

2. It is not a replacement to the democratic voting process.
   Some say there is no voting needed in the future because AI can make the decisions for us. This is NOT what this propsal is about! AI should not make decisions instead of the humans, because
   - The process then would become a black box, we could not reason about it. So it can be easily hijacked.
   - We should not make people get lazy and get used to not being involved in the  governance process. That would increase voter apathy instead of decreasing it.
   The proposed system should only recommend the user those delegates that best match the user's voting record, and hence make the selection of the best delegate easier. Once we have a good delegate, we still need to follow the governance process with one eye so to speak. Our delegate might change its prefernces, even can become corrupted later, in which case we need to switch delegates. But switching is also easier with the  recommendations.
