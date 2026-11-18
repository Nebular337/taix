### My first experiment with RAG

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
