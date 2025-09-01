## What it does

**FiloDataBroker** is a platform that provides data owners with an additional monetisation channel for their data for use in LLM applications.

The service enables export of data using specific tools, storage of public and private data chunks as separate files on Filecoin, NFT-gated access to encrypted private data, and on-chain datasets registry. The system includes automatic dataset preservation fees and guaranteed 7-day storage availability for the data access buyers (LLM applications).

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

Additionally, there are some concept-specific obstacles:

1. No well-established standard for integrating micro-payments into AI chat workflow
2. SQL-like querying of CSV data requires loading the whole dataset into an MCP server memory

We've managed to mitigate all that by either implementing or planning appropriate workarounds. 

## Technologies we used

1. **Filecoin**: PandoraService, PDP payment rails, Filecoin Web Services, Synapse SDK, USDFC.
1. **FilCDN**: for realtime data retrieval
1. **ERC721** for NFT-gated access on the Filecoin EVM
1. **Lit Protocol** for encryption keys storage
1. **MCP protocol** for integration with AI applications
1. **AlaSQL** for querying CSV data with SQL

## How we built it

The idea was proposed in the "call for startups" document shared during the PL_Genesis hackathon. Originally, it was proposed as a WordPress plugin but we've decided to go with more generic CSV data source for the hackathon-grade PoC implementation.

We split the implementation into three projects:

1. EVM backend - on-chain datasets registry + custom PandoraService fork with fine grained control over the datasets storage lockup period
1. CLI tool - can be executed in the same infrastructure where original data resides, guides user to select public/private data chunks, encrypts private data with encryption keys securely stored with Lit protocol and uploads both to the Filecoin via Synapse SDK
1. MCP server - reads dataset registry from on-chain registry, provide tools for searching for and in the datasets, uses Synapse SDK to fetch the data and Lit protocol to retrieve encryption keys for decrypting

## What we learned

- PDP deals system is a powerful framework for customising data storate SLAs for virtually any project needs
- Synapse SDK is a great interface for building on top of the Filecoin Onchain Cloud
- CLI tool will not satisfy target audience, so we need to provide more conventien tool (possibly, with web UI)
- To find PMF we need to narrow down the target audience

## What's next for

We must find the PMF - that's our priority #1 right now.

To reach the potential target audience, we are planning to create a WordPress plugin that will support exporting data using a convenient web UI for non-technical users. The first version of the plugin will not have all features, but will enable us to establish a direct communication channel with the target audience.

We also need to find a way to accept micro-payments on the MCP server, instead of asking LLM application users to grant our MCP server access to their EOA private key.

We want to log the data access events on-chain for auditing purposes.
