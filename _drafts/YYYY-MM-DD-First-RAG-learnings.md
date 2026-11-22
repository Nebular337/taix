# My first experiment with RAG

## Starting point 
I'd heard about RAG systems and a few variations thereof across various parts of AI Twitter. How to bring elements from a knowledge base into the context window of your LLM to improve the quality of outputs. 

It's a sound concept - for the world of tax it means that you can bring in up to date tax law every time, even if it changed after the LLM's training cut off or to make sure that it gets the rules from the right jurisdiction. For this experiment, the world of tax showed a commercial value but i wanted something more personal. 

My thesis therefore combined the fashion at the time of using LLMs as a life coach/therapist thing and the want to explore RAG. It would therefore be a chatbot using a similar set up to what I'd built previously but adding a knowledge base made of Tim Ferriss' 4 Hour XXXX books and the blogs of Sam Altman and Paul Graham. The user presents a question, the program finds relevant extracts from the knowledge base, uses these to supplement a response from the GPT API and then facilitate further discussion (but not additional retrieval) in a chat bot style. 

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
-- Otherwise everything is broken into chunks of no more than 450 tokens using full sentences - we break the text down to individual sentences and then build the chunk up from those whole sentence building blocks (subject to breaks at the end of chapters and blog posts, enforced by each chapter and blog post being its own file). This is all done in chunk_corpus.py.
-- To manage breaks in context (e.g where the sentence before the chunk started gave some important context for the new chunk) I included an overlap of 60 tokens.
-- I can't remember the specifics a year later but I seem to remember experimenting with the chunks slightly, possibly starting at 300 tokens and working up until I started to get some useful answers. I don't remember testing the overlap, so the 60 may have just been 20% of what I started with and then didn't change!

### Choosing the embedding model
- The model chosen was gemini-embedding-001. From memory this was because it was relatively new at the time and most importantly available for free for the amount of content I needed to chunk.
- The initial test run of embedding was done in embed.py which includes some time management aspects looking to avoid rate limits on the free API and the embeddings were simply saved to a .txt file.
- Once I understood how the API worked a bit better, embed_to_PG.py, we ran the full embedding process with the vectors saved into a postgres SQL database along with a document id for the underlying document. 


### Choosing the chat model






