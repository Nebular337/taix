# My first experiment with RAG

## Starting point 
I'd heard about RAG systems and a few variations thereof across various parts of AI Twitter. How to bring elements from a knowledge base into the context window of your LLM to improve the quality of outputs. 

It's a sound concept - for the world of tax it means that you can bring in up to date tax law every time, even if it changed after the LLM's training cut off or to make sure that it gets the rules from the right jurisdiction. For this experiment, the world of tax showed a commercial value but i wanted something more personal. 

My thesis therefore combined the fashion at the time of using LLMs as a life coach/therapist thing and the want to explore RAG. It would therefore be a Q&A with a knowledge base made of Tim Ferriss' 4 Hour XXXX books and the blogs of Sam Altman and Paul Graham. The user presents a question, the program finds relevant extracts from the knowledge base, uses these to supplement a response from the Gemini API. 

## The plan
The key pieces needed for the project were quickly identified as: 
- Source, cleanse and format the knowledge base
- identify a suitable embedding model 
- identify a chat model 
- confirm approach to chunking and context length
- establish a means of storing the vectors.
- create the RAG enhanced chat bot
- implement it all for a budget as close to zero as possible!

### The knowledge base
- There was no real formality to choosing the knowledge sources, it was a case of finding a few self help books that I was somewhat familiar with to start with, and then had the idea of spicing it up with a couple of blogs as well. It was all for personal use - I own copies of the books and .txt versions were easily available online. Likewise I found a couple of github repo's that already had .txt files of enough of the two blogs for the purpose I needed.  
- Some of the sources still needed cleaning up, i did this through a mixture of manual changes by hand and, for some more repetitive changes a couple some python code (splitting the individual posts and chapters into separate .txt files, reducing and simplifying line breaks and whitespace, forming the correct JSON structure including book titles/chapter name/author name - blogsplit.py, chaptersplit.py and jsonification.py). Then build_manifest.py gives me a single catalogue of the clean content files.  
- Once i was happy that the knowledge base was clean and structured, it was time to do something with it - chunking.
-- First up the blogs, for any blog less than 800 tokens (using tiktokeniser to count) we use the full blog post.
-- Otherwise everything is broken into chunks of no more than 450 tokens using full sentences (unless it was a single very long sentence, then the chunk just became the sentence)  - we break the text down to individual sentences and then build the chunk up from those whole sentence building blocks (subject to breaks at the end of chapters and blog posts, enforced by each chapter and blog post being its own file). This is all done in chunk_corpus.py.
-- To manage breaks in context (e.g where the sentence before the chunk started gave some important context for the new chunk) I included an overlap of 60 tokens.
-- I can't remember the specifics a year later but I experimented with the chunk lengths slightly, including a test of 300 tokens and working up until I started to get some useful answers. One of my test scripts also includes a 75 token overlap so I obviously also changed that around too. 

### Choosing the embedding model
- The model chosen was gemini-embedding-001. From memory this was because it was relatively new at the time and most importantly available for free for the amount of content I needed to chunk.
- The initial test run of embedding was done in embed.py which includes some time management aspects looking to avoid rate limits on the free API and the embeddings were simply saved to a JSONL file.
- Once I understood how the API worked a bit better, using embed_to_PG.py I ran the full embedding process with the vectors saved into a postgres SQL database along with a document id for the underlying document. 

### Choosing the chat model
- For similar, budget driven reasons, Gemini 2.5 flash and flash-lite were chosen as the chat models to use for the project.

### The RAG tool
- After a couple of quick experiments using only the embedding vector of the question itself, it became apparent that an extra step was needed as questions would often return no results (in part due to the limited number of sources admittedly). I was aware of HyDE as an improvement approach for RAG.
- To implement this, the Flash-Lite model was used to create 150 token examples of the types of responses that could be expected from a business or health book on the topic given by the user. Given the knowledge base, this prompt was focused on business and health and encouraged the use of domain specific terminology. The Lite model was chosen due to the simplicity of the task required, being a case of essentially "take these initial seed words and produce more of them that are similar". The examples returned were then joined to increase the chance of overlapping terminology with the corpus. 
- The combined examples were then turned into an embedding vector using the same gemini-embedding-001 model used for the underlying knowledge base. Cosine distance between that returned vector and the vectors of the knowledge base was then used to identify the most similar chunks to the synthetic responses. An arbitrary cosine distance of 0.35 (i.e. cosine similarity of 0.65) was used as a hard cut off.
- I had then intended that the 20 closest matching results (or all of those within the cut off if less than 20) would be ranked using google's semantic ranker API to determine which knowledge chunks would go into the final context window. In practice the cosine distance test tended to produce few enough results that I didn't need to filter out and rank the chunks.  
- The identified chunks along with a simple prompt were then passed to Gemini 2.5 Flash to provide the final user answer.

## Learning
- This project gave a great insight into some of the complexities of managing a knowledge base - which has given me a lot more context at work when discussing how the firm can best use its own knowledge.
- I gained a greater understanding of how simple RAG works and its limitations (e.g where questions are often quite different to the response, which is almost always true in tax where the client's question is typically something helpful like "I'm doing x, what tax issues are there?" and don't tend to include specific references to parts of the relevant tax law!). HyDE is obviously just one of a number of different ways to retrieve knowledge from a corpus of information but for this project it was a great way of reinforcing how RAG works and doesn't work.
- Some more experience of using APIs to access information and parse the responses to extract the desired information.






