TPAC 2024 - Purposeful Permissions Breakout
===========================================

Attendees
* Alexandra 
* Nick Doty
* Serge
* Simon Pieters (Mozilla)
* [Many other people]
    
Presentation
------------
    
*  Alexandra: Hello! We're interested in enriching permissions prompts with additional information to help users make informed decisions.
    * Current permission requests generally look like "[origin] wants to use your location", might happen without context, might be confusing to users.
    * Might wonder what's being done with the data, retention, sale, etc.
    * Answers might be found in privacy policies, but it's a large document that might not be granular enough to understand this specific request.
    * Some websites do prepare users for a request, but not structured, not consistent.
    * Some exanmples, like government credentials, are particularly interesting.
* Nick: Regulations around use cases and registration for credential usage. Drivers license, passport, etc. Want to avoid a context-free request for this information. Legal requirement for more detailed explanation.
* Serge: Privacy as contextual integrity. Nissenbaum, "Privacy in Context". Challenges in translating this framework into something operational, but the gist is that privacy is the appropriate flow of information within contexts according to contextual norms. Canonical example is that most people find it acceptable to share medical information with a doctor for treatment purposes, but would find it objectionable if the doctor used that information to gossip with other doctors about your condition.
    * Browser decisions today are framed around the data type. Context is often inferred from users based on cues, and they're often wrong. Mapping site's request for location data, users assume that the location was used to put your location on the map. Surprised to find the data might be sold to data brokers, etc. This distinction is hidden by the existing prompts.
    * If users are asked to make these decisions, it's incumbent upon developers to state the purpose. Key piece of information users use to reason about these things.
* Alexandra: Assume that users have habituations now, make fast decisions on the way to doing the thing they want to do. Based on the website, what they want to do, prior experience, etc. Users might not know where to look for additional information,
    * Adding purpose/usage information would be percieved as valuable by users.
    * Useful to know that location, for instance, is used for multiple purposes beyond core site functionality.
    * Data use information has to come from the website, as they know the usage.
    * Browsers could surface the information, but users might assume that the data browsers render is verified in some form even if the browser is just a messenger.
    * Prior art: P3P.
* Serge: Morbid curiosity about who's familiar with P3P? Was a standard for machine-readable policy language. Sites could define a policy using XML, put it in a well-known location, and browsers could make decisions about whether the policy conflicts with user preferences/configuration. Could generate warnings, or could take other actions. Point was to create a formal language. Thing to steal from P3P: a lot of time was spent in the group defining tags for data collection purposes. Lots of prior art there.
    * Very small text from the P3P spec around the `<purpose>` XML tag: <https://www.w3.org/TR/P3P/#PURPOSE>
* Alexandra: Players in the ecosystem have distinct roles:
    * Websites need to declare their usage intent.
    * Browsers could show the infromation and create aggregate reports.
    * Regulators/governments could use these reports to augment assessments of websites' practices
    * Standards community defines and evolves the declaration format.
* Alexandra: Different granularities available:
    * Could link to the privacy policy in the prompt.
    * Free text from the site.
    * Labels/enumerations like P3P and app stores.
    * Combinations of the above.
    * `.well-known` files, potentially with external verifications.
* Alexandra: Different form factors!
    * Pull on-demand where users can pull data.
    * Putting more metadata in prompts.
    * In the content area, perhaps alongside PEPC (<https://github.com/WICG/PEPC>) elements.
* Alexandra: Challenges/opportunities.
    * Each of these approaches has pros and cons.
    * <missed it>
    * Free text: abusable and unstctured, but easy to adopt.
    * Labels: Seems like users might have limited understanding of these broad categories, subcategories might help. Standardizable and consistent. Scannable at a glance. Easy to compare across sites. Browsers still not able to verify independently. Websites might take issue with generalized labels for their specific practices.
    * Adoption:
        * Why would sites declare? In the UXR session earlier, when users get context around permission requests, they tend to grant more often.
        * How can websites communicate change in their usage practices?
        
Discussion
----------

Don (Raptive): A lot of sites are in a position where they feel like they've obtained consent for processing by certain parties. They have a consent string, IAB Tech Labs global privacy platform. Long strong explaining which parties have permission for which usage. Possible to integrate asking users for permissions in this way with the consent dialog that they've already been exposed to? Avoids two-steps which may be out of sync.

Nick: Consent management platforms. Tools and vendors some sites use to get what they believe is legally-verifiable consent. Community has concerns about whether those dialogs reflect consent, but they aren't cases where something is needed from the browser. They just pass on the user's response. Perimssion prompts are capabilities the browser provides. Could be that we should think more broadly. When websites ask for consent, even if not for a given capability, perhaps we should make it easier for them to provide context.

Don: Consent prompts: in cases where the user said no via in-page prompts, should the browser ask again?

Serge: Already the state of the world. My understanding of the target of this proposal is augment current permission prompts, not expand their scope. In status quo, prompts for specific data types exist.

Don: Extract data from GPP string, know about downstream uses, better inform the browser's UX.

Serge: Expected uses of the data, wouldn't need a prompt at all.

Boaz: Consentful tech. Planned Parenthood's definition of consent, applies it to web flows. Freely given, informed, reversable, enthusiastic, and specific. This could help with the informed part. Dark patterns: "Better say yes, or you'll be bad!" Labeled versions mitigate this. Free text can be coercive. <https://consentfultech.io>. Claim verification: would be interested in thinking more about what we could do here. Also, we don't want to prompt a lot: "specific" might conflict with this.

Charlie Harrison: Motivation for this makes the prompts more specific, say what's happening with the data, will lead to more adoption. I feel nervous that any of the solutions will be abused by bad actors, not just free text. If you could get the website to spoof the lock icon, people would feel safer: if there's something in the enums that are like the lock icon, scammy/malicious sites will spoof it. I like the goals of this work, but I worry about spam and fraud and people accepting permissions more often than they ought to.

Serge: This was a criticism of P3P. "Sites will lie." Lies are legally actionable, just like lying about your privacy policy. Material misrepresentations in public disclosure (tricking browser) are legally actionable. Regulators, plaintiff's attorneys, etc.

Alexandra: Abuse in permissions happens mostly with notifications. No user data shared, could be descoped from this effort entirely. Might be ways to make the declarations less abusable: more specific subpurposes could help. But I agree, there is a risk of abuse.

Charlie: I need to think harder about permission prompt spam I get on shady sites. If tied to functionality users can see, that feels like a decent mitigation. Would want to avoid happy-feel-good strings/enums.

Nick: Range of actors we care about: sites operated by companies that follow law and are worried about consequences. Might not need to worry about lying, but gives them a path to restrict their work. Other sites are malware...

Charlie: We've spent years clawing back freeform text from potentially-trusted dialogs (`alert()`, etc).

Alexandra: Free text is abusable. But also not the case that we need to show purposes in all cases. If we detect scammyness, we can simply not show the prompt.

Nick Steele (1Password): Extensions getting into identity management space. Consent is within our remit. Credential managers want to think about consent more categorically, streamline it on many sites. Extensions reject all cookies, for example. We're thinking about consent on sites in ways that allow us to streamline things in categorical manners for users. Would like to ask users what they're comfortable giving out "never"/"always"/"sometimes", and do our best to reflect that preference in their interactions on the web. Goes hand in hand with identity wallets' work.

Matt Miller (Cisco): I see the value in empowering websites to be more expressive about their use of data. Appreciate that as an end user. Home Depot wants my location data, what else are they going to do with it beyond show me directions to the nearest store? Websites have the ability to present more meaningful permission prompts, they might present _more_ permission prompts. Is there an effort to combat that? If I have MIDI device, I migth want to share it all the time, but might not want to share location ever. Fatigue, need to give users more agency.

Mike: Notification session later today might be interesting.

Boaz: Could you say "Yes" to showing me where the stores are, but "No" to other aspects of usage. More granular consent. Map out all the ways data can be used, model consent flow based upon that.

Serge: Yes.

Alexandra: In principle yes, but would need to figure out how to represent. Overwhelming users in the prompt.

Serge: Open research question here. Primary interface should be succicint, but secondary UI could be interesting for the users who are interested. Informative, succient prompt: what information goes there? Migth be a specific set of types and purposes that 99% of users don't care about, could avoid prompting for those. Can't make those decisions with just the data type.

Boaz: In this example "claims that..." it does X and Y, would be nice to be able to uncheck some of those specific purposes.

Alexandra: Generally agree, but squeezing more granular decisions in this prompt is hard.

Boaz: How many claims could we make here?

Alexandra: Open question for UXR.

Nick: We could surely design a system in which permission grants include a list of purposes.

Boaz: Would be amazing!

Nick: Complicated for developers.

Boaz: That's the logic that this whole social system we're talking about depends upon. Implies an organization capacity to track usage. System should be designed to support that.

Serge: Was argued about at length during P3P. User preferences and site policies, etc. Wasn't appetite for that at the time, but maybe not.

Tim (Okta): Devil's advocate: my parents don't understand that putting a credit card into their phone is more secure than a physical card. How are they supposed to understand this in general? Image upload of a physical card vs didital mediated access?

Nick Doty: Should be clear. I don't have the goal of either increasing allow or deny rates. Those are the success metrics. Presenting information in context: for the good actor case, it increases confidence. Gives the user good reasons to make a decision they want. If you're trying to prove your identity, extra context helps. Puts some burden on the websites to set up that prompt, and be accountable. Not likely to shift the user towards other means.

Matt: Is this system intended to head off future cookie banners? What's the carrot/stick to implement these more detailed prompts? Might be desire to leave the door open. Cookie banners are popular problems. Lifing it to the browser is appealing for consistency, etc. Would we want a general purpose governnmennt-mandated consent collection system?

Alexandra: Initially out of scope, focused on permission prompts as they exist.

Nick: Some implementers and browers are trying to handle those prompts. There might be interest.

Simon (Mozilla): For dialogs/permission prompts for location, etc. The website should explain usage to the user before the prompt. At the time the prompt is shown, it might already be too late.

Nick Doty: One example in the deck: sites have the ability to explain things to users in HTML prior to asking. If there's something like PEPC that initiates a capability request, browsers could similarly confirm that text around the button was shown to users. Sites should explain in that context. Privacy relevant information would be on the website prior to the prompt, as opposed to in the prompt.

Simon: Would then need to check a box or click a button?

Nick: You saw text, clicked the button, then browser confirms your intent. This also speaks to Charlie's point. Not in the browser UI, in the website.

Charlie: Does this require us to do anything? Implementable today.

Nick: Would want the browser to make sure the explanation was there.

Charlie: Confirmation that _something_ was shown, Would the browser do anything else?

Nick: Different levels. Could markup text, could make sure specific pieces of text. Could record context.

Charlie: Let's say we build visibility detection for permission prompts. What's the incentive for adoption if JS allows prompting? Would need some additional incentive to push developers. This is otherwise only a burden on developers, no carrot.

Nick: Browsers would need to say that this is necessary in some situations.

Serge: Browsers could say that purposes were required to get fine-grained location, for instance.

Michael (Google): If a user accepts a permission prompt and a site then adds more usage. Would we prompt again?

Serge: Yes!

Alexandra: Not sure if the browser decides, or whether website should be able to choose to reprompt. Currently not possible for a site to do. Might be worth adding.

Michael: Could also decide to just use previously-granted purposes?

Alexandra: Right, makes this the website's decision.

Serge: If a site uses data for purposes beyond its declarations, that's legally actionable.

Theo (Google): Zooming out a bit: I really like this work's ability to construct norms on the platform. Setting norms around cross-site tracking, etc. This is a natural progression of that. At the moment, the platform is silent about valid usage of things like location information. Rather than putting the emphasis on the user to make the right decision, we can change that calculus as a platform. Could deny precise location for some subset of purposes. Or only allow certain purposes to be declared when requesting. Long term. this isn't consent management, but a way for the platform to express an opinion about valid usage of data.

Boaz: Browser negotiating on behalf of users. Groups of users negotiating? Data unions/trusts. Could be a flow in which groups of users say "None of us will give you permission unless you reduce usage.

Theo: Browser can facilitate. Balance the playing field.

Nick: Getting to an interesting idea in which the browser is the one deciding which things are appropriate, but governments/auditors/org you trust could also step in.

Charlie: Aggregate reports. Is that going along here? Creating transparency log?

Alexandra: Such a declaration system would enable that.

Charlie: Who would publish?

Alexandra: Browsers? Similar to CrUX.

Boaz: Audit of all the times of who I gave what to? What they claimed at the time? Or in my data union with others pooling their data.

Tim: Already a pattern for wallets: can see transaction from the past. Receipts.

Nick: Not entirely new, but possibly helpful. Wallets: we've been talking about single pieces of software here, but in identity space, passing requests from a website through the browser to a wallet application. A little dangerous if you lose the context inbetween. Would be nice to have a standardized way to communicate this context between apps.

Tim: Second user agent? Wallet. Supposed to act in users' interest as well. Passkeys, WebAuthN with an authenticator? What's the responsibility split?

Boaz: Is the browser an authorized agent under California law?

Don Marti: No. Separate concept. Browser is _you_, authorized agent is someone acting on your behalf.

Boaz: Might be worth expanding the scope to change that.

Don: Lots that browsers and agents can do together.

Nick: Next steps. Things to bring to incubation? Vounteers? And research questions.

Serge: Starting to think about research questions in this space:
    - Enumeratred purposes or free text?
    - What do users need to make informed decisions?
    - Will users explore secondary UI?
    - Which designs result in the narrowest gap between stated preferences and observed behavior?
    
Nick: I think there's enough interest that we should write something up. Mike started on something around enumerations. I'm happy to write something about the in-content style. Could look at both, and bring them into a CG.

Kaustubha (Google): Thinking about this in other contexts: FedCM. Related Website Sets, group of sites indicating data sharing between sites. Would be interested in reusing any list of enumerations you come up with.

Tim: Other thing: passkeys -> digitial credentials. No org is doing research across web and app platforms. Missing.

Lee: App->Wallet might not enable some of the consent screens. Trying to look at the whole thing end-to-end. FIDO covers both sides for passkeys, doesn't really exist for other credential types.

[done]

(Nicholas Steele, 1password, interested in collaborating on extensions
