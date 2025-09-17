## What it does

**FiloDataBroker** is a platform that provides data owners with an additional monetisation channel for their data for use in LLM applications.

The service enables export of data using specific tools, external data storage infrastructure using Filecoin, and data access monetization.

## The problem it solves

- No compensation for data scraping - while LLM applications present summarised content to the end users, the data owners lose visitors and thus a potential revenue. For example, see report from  the [DoubleVerify](https://doubleverify.com/ai-crawlers-and-scrapers-are-contributing-to-an-increase-in-general-invalid-traffic/).
- High load from web crawlers in the era of AI - e.g. one user reported 30TB of bandwidth consumed in a month according to the [InMotion Hosting Blog](https://www.inmotionhosting.com/blog/ai-crawlers-slowing-down-your-website/).
- General frustration about LLMs taking over the internet - data owners have no choice but to fight AI crawlers by completely blocking access to their sites, forcing the latter to apply tricks to bypass that. See these discussions for example: [AI is killing the WordPress web publishing industry](https://www.reddit.com/r/Wordpress/comments/1mq75y3/ai_is_killing_the_wordpress_web_publishing/) and [I have blocked Scrapy bot because it almost killed my CPU](https://www.reddit.com/r/Wordpress/comments/1l2yktq/i_have_blocked_scrapy_bot_because_it_almost/)

## Challenges we ran into

Since the Filecoin Onchain Services (formely Filecoin Web Services) are in their early age, some features were missing at the time of the PoC development:

1. No means for triggering files removal from Filecoin manually
2. No data encryption mechanism built-in to the protocol or SDK
3. No access control management for the data reading
4. No data access log for auditing purposes

## Technologies we used

1. **Filecoin**: PandoraService, PDP payment rails, Filecoin Web Services, Synapse SDK, USDFC.
1. **FilCDN**: for instant data availability
1. **LLMs.txt standard**: for integrating with LLM/AI applications
1. **HTTP 402**: for monetizing access to the data

## How we built it

The idea was proposed in the "call for startups" document shared during the PL_Genesis hackathon.

The system consists of several components:

1. FDB backend - integrates with Filecoin via Synapse SDK. Handles ACL, datasets management, billing and payout management with USDFC token on Filecoin blockchain.
2. WordPress plugin - provides UI for the website administrators to set up what data they want to share, integrates with the FDB backend for the data uploading, generates llms.txt file for LLM/AI applications
3. FDB CDN - wraps FilCDN by introducing monetization using HTTP 402 header.

## What we learned

- PDP deals system is a powerful framework for customizing data storate SLAs for virtually any project needs
- Synapse SDK is a great interface for building on top of the Filecoin Onchain Cloud
- To find PMF we need to narrow down the target audience to the clearly reachable community

## What's next for

We must find the PMF - that's our priority #1 right now.

We are planning to use the WordPress plugin for reaching our target audience directly. The first version of the plugin doesn't not have all features planned and we're planning to adding them incrementally.

We also need to find a way to accept micro-payments via HTTP protocol. Our current vision is to utilize HTTP 402 response header instructing LLM/AI applications how to pay with USDFC token. Further research is needed to clarify the actual implementation details.
