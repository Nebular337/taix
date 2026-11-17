### "What I've done so far"


I'm not a software engineer, architect or anything like that. I've been a UK tax professional for 9 years and then spent the last year helping tax people get to grips with how AI can help them. That's a role I moved into because I was already interested in the world of AI and following it closely. So closely, that I'd already done some of my own experimentation. 

What I'd experimented with until that point were largely creating small projects with AI doing lots of the grunt work whilst I still tended to go into the underlying source code to understand it, learn and bug fix: 
- using GPT-4 to create some basic websites, such as one to help kids learn to read their first words, another to explain the eligibility of new childcare support. These were simple front-end only javascript sites. At that point GPT-4 wasn't great and I was very keen to understand the underlying syntax so there were chunks of code that I wrote or fixed in here.
- creating a simple flask app that acted as an AI chat bot to talk to a Gemini model through their API.
- creating tools for my wife's business to query and call the BrickLink API to help with valuing bulk lego with outputs into the webpage and then adapting it to also output into a google sheet for tracking inventory.
- building a simple catalogue website for the lego business with the google sheet of inventory as the data back end. this included helping my wife improve the data quality and teaching her how to maintain the data quality in future.
- a voice to text model wrapper, deployed on google cloud so I didn't have to use the awful microsoft dictation tool at work and could, instead, use something built this decade!
- a local RAG tool using the OpenAI embedding model and GPT models that would take a user query for "life advice", use a lightweight GPT-nano or mini model to produce an example response, use the API to obtain a vector for that created response and then search a knowledge base comprising several self-help style books and the back catalogue of a couple of blogs to help inform a final response from a final OpenAI LLM call.

 I learnt different things from each of these, some of that depended on how much heavy lifting agentic coding did, some of it was understanding more about deployment and different tools, some of it was just getting horrifically lost between all of Google's different developer services that have nearly identical names! 

 A couple of these might get their own posts about what I learned from them but really these were the starting point, and I've learned a lot more since then...
