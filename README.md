# home-chain
On-chain governance for your local community

## Introduction:
**Home-line** is a recently built online community management system.
- 4 years of development work
- fully open source
- community driven
- goal is to promote liquid democracy in small communities,
- make community management convenient and fun.

Currently used in Condominium buildings in Hungary (around 500 active users atm).
Condominiums are a perfect fit for such system. They are a community of flat owers. They have lots of issues to track, decisions to make and agree on budgets. Trancparency of the money movements is very immportant. They can benefit higly from the online liquid democracy process. Decision making is much easier that way, compared to long annual owners meetings.
(Hence the name **Home-line**. "Manage your home online from home, conveniently.")

But **Home-line** can manage all types of communities (namely company, foundation, simple association, permaculture co-operative aka. basket community, stc) and supports multiple languages (currently hun and eng, but new translation files can be added easily)

The system is designed to fully handle all aspects of a community's management from one place
- Discussion board
- Decision making
- Issue tracking
- Accounting
- Document store

### Live Demo:
[English language demo](https://demo.honline.hu/demo?lang=en)
-- [Hungarian language demo](https://demo.honline.hu/demo?lang=hu)

### Code: (fully open source)
[https://github.com/edmo/lidehouse](https://github.com/edemo/Lidehouse)
(Lide house means liquid democracy house)

### Full feature documentation (only in hungarian):
[Home-line documentation - HUN](https://docs.google.com/document/d/1rCKTzCmbFJiX7F2QaLujmd8mTVmuEUWlsuqxf8_81SY)

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

### Why make it a DAO?
- Decentralized execution makes it less dependant on the elected officials (like the condominum manager). - 
  Communities (Condominums are other types) benefit largely from decentralization. A corrupt offical cannot  silence the comments and proposals of the opposing members if the system is run on a chain instead of a server ran by the offical himself.
- Anonymous voting is possible. -
  In such a way that nobody see each other's votes and the manager does not see anybody's vote either, while everybody can still check that their vote is correctly counted into the results. The current online version only supports public votings and the manager has to see everybody's votes because of the client-server architecture, since he is the one adding them up.
- Communities can start issuing their own local currency. - 
  The community can decide to create a locally accepted currency that the members can use to pay each other for services provided by each other for each other. Example: I have time to babysit your kids tonight,  I receive some local tokens, and next week you help giving my son some math lessons.
  Local economies can be born, where the local currency is backed by the local's services.

### Why a parachain?
Polkadot is a perfect fit for such a governance and community  management sytem
- Small communities can benefit from the strong security provided by the large Polkadot ecosystem
- Home-chain can use elements of Polkadot OpenGov (tracks, approval curves, etc) - 
  The current version uses a simple delegation system, and number of vote evalulation techniques, like single choice, multiple chopice, ranked votes and Condorcet method, but the tracks and approval curves of OpenGov could enhance the sophisticatedness of the decision making process.
- Hopefully some features of Home-chain can be integrated into pallets and offer other para-chains the opportunity to use these community management tools as well. Sharing is caring. - 
  Think of Home-chain as a very smart wallet, that in addition to the traditonal governance, has features such as fully compliant accounting reports and issue tracking. Think of it as mixing the features of Nova wallet with a sophisticated CRM system, enjoying the benefits of both from within the same app.
- The functionality of the app can change without the hassle of fork management. - Such a complex CRM app gets new functionalities very frequently. It is critical to develop in a framework where it causes no problem to update all node's code versions conveniently, possibly as frequently as daily.

## Technical implementation:

Home-line is a meteorjs system. - See [https://www.meteor.com](https://www.meteor.com)

It is a server based SPA (Single Page Appilcation)
- Database is a mongodb. Business data objects are json objects
   
  Example (a "Voting topic" object in the db):
```
{
    "_id" : "HZLyywrw9Sppt7Z7g",
    "communityId" : "y38GnfKaWTgsmxrfB",
    "category" : "vote",
    "title" : "Tároló folyosó lezárása",
    "text" : "Tisztelt Tulajdonsok! Kértünk be árajánlatokat a tároló folyosó kulccsal zárhatóvá tételére. Kérem, hogy szavazzanak hogy melyik megoldást válasszuk.",
    "vote" : {
        "procedure" : "online",
        "effect" : "legal",
        "type" : "choose",
        "allowAddChoices" : false,
        "choices" : [
            "Nem kell zárható legyen",
            "Kulccsal zárható legyen (13 900 Ft + 2800 Ft/kulcs)",
            "Proxy-val zárható legyen (371 221 Ft)",
            "Tartózkodom",
            "Több információra lenne szükségem"
        ]
    },
    "opensAt" : ISODate("2023-11-17T00:00:00.000+0000"),
    "closesAt" : ISODate("2023-12-02T22:59:59.000+0000"),
    "status" : "deciding",
    "createdAt" : ISODate("2023-11-17T13:38:25.605+0000")
}
```
 
- Users can call so called "Methods" to create or update objects in the database. Methods can be called only if user has the appropriate permissions.
  
  Example (Method for modifying a "Comment" object):
```
export const update = new ValidatedMethod({
  name: 'comments.update',
  validate: new SimpleSchema({
    _id: { type: String, regEx: SimpleSchema.RegEx.Id },
    modifier: { type: Object, blackbox: true },
  }).validator(),

  run({ _id, modifier }) {
    const doc = checkExists(Comments, _id);
    const topic = checkExists(Topics, doc.topicId);
    checkModifier(doc, modifier, Comments.modifiableFields);
    checkPermissions(this.userId, `comments.update`, doc); // can only update your own comment
    Comments.update(_id, modifier, { selector: { category: doc.category } });
  },
});
```

### How to convert the meteorjs app into a parachain:
It is pretty straightforward to convert a meteorjs app to a chain! (This is the main reason meteor was choosen as the framework for this app)
- The UI of the current Home-line appication can be kept as it is
- But the users instead of calling methods on the server they submit their method calls to the network
- Para-chain blocks are built from these method calls
- Para-chain validators execute the code in the method for the desired state transitions

The process is pretty straightforward. Subtrate provides all the node functionalty, while each js method call has to be converted into a contract call in the runtime.

If the prize is won, the resources will be used to start the development work and make the conversion.

### Identity management in Home-chain
Identity will be managed by a modified version of the Identity pallet of Substrate. 

There  is no root registrar needed for Home-chain. Anybody is allowed to launch a community, for example a new condominium, and once she does, she becomes the admin of that community. Admin can invite or add new members (with chosen role and permissions) to the community and at the same time register an identity for the new members. Eg. community "Building: Danube street 10" is created by Alice. So Alice can now give out identity to Bob, as "Bob, owner of flat 101" in  Danube street 10. Note that all this idenitity tells others, is that in that specific community called "Building: Danube street 10", Bob proved to the admins that he is called Bob and he owns flat number 101. If he happens to be a dual citizen, with a slightly different passport name "Bobby" and shows that to another building admin in the other country, he might very well get a second identity registered there with the name "Bobby, owner of flat 506" in that community. Identities are relevant only in the scope of a community, and community admins work as identity registrarat the same time. The outside world can decide how strong they trust an identity given out by a certain community. If that community is just a small building with 10 flats, the assurance is obviously lower than if that community is the "Republic of Hungary" (which's registered identities we call legal Passports).  So Identity management is fully decentralized in Home-chain.

The functionality will be put into a modified pallet called "Decentralized identity" pallet, and will be submitted to Substrate dev team.

-----------------------------------------------------------------

# AI extension to Liquid Democracy 

How can AI assist our on-chain liquid democracy system:

### Fighting voter apathy:
AI can know us and our wishes better than we know ourselves (as scary as it sounds) by analizing our activity.

For exampe on Social media, just by looking at a dozen likes, AI can make recommendations very accurately about what else we will like in the future. So it can suggest us very relevant content.

Similarly an AI looking at a few of our votes in the liquid democracy system can give us very good recommendations on who should we delegate our voting power to, simply by finding those users whose voting record matches ours most.

In Liquid Democracy systems the user always has the choice to either vote directly on proposals, or 
they can choose to particate in certain types of votings via a delegate, handing over the voting power to someone they trust in that area. Delegates can be selected separately for different topic categories.

Since most of us do not have the time to cast direct votes on all proposals, if we don't know anyone competent to delegate to, it usually causes voter apathy. Neither a direct vote, nor a delegate is chosen. A large part of the voting power stays away from particpating at all, leaving the decision in the hands of the few. 

But if we could select very good delegates very easily, and all it would take is to cast a couple votes ourselves. Now this could change the attitude!

## Warning !!
1. Only for public votings.
   
   No system (human or AI) should get access to our private voting records!

   Although it is imaginable, that certain delegate candidates would intentionally de-anonimize their own private voting records in the hope of getting more voting power from others. In that case a strictly local running AI could read our votes and match that to de-anonymized voting records.
   
3. It is not a replacement to the democratic voting process.
   
   Some say there is no voting needed in the future because AI can make the best decisions for us better than ourselves. This is NOT what this propsal is about! AI **should not the make decisions** instead of humans, because
   - The process then would be black box, we could not reason about it. So it could be easily hijacked. Without us even realizing.
   - We should not encourage people to be lazy and get used to not being involved in the governance process at all. That would increase voter apathy on the long run instead of decreasing it. We all know that since GPS navigation became popular, people started to not be interested in, and what is even worse loose the ability of, reading raw maps. "If you don't use it, you'll loose it" - the saying goes (meaning skills).
     
   The proposed system should only recommend the user those delegates that best match the user's voting record, and hence make the selection of the best delegate easier. Once we have a good delegate, we still need to follow the governance process with one eye so to speak. Our delegate might change its preferences with time, or can even become corrupted, in which case we need to switch delegates. But switching hopefully becomes much easier with the provided recommendations.
