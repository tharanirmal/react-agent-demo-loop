# ReAct Agent Demo Loop

A ReAct agent that reasons, acts with a search tool, reads the result, and repeats until it can answer. It uses the Gemini API and runs in Google Colab.

The agent answers questions from a small home coffee brewing guide. It only answers from those documents, and says so when they don't cover the question.

Open [`agent_loop_demo.ipynb`](agent_loop_demo.ipynb) to see the code and the output of a full run.

## How the loop works

1. **Reason:** Gemini reads the question and decides whether it needs to search.
2. **Act:** it calls `search_documents` with a query it writes itself.
3. **Observe:** the search result goes back to Gemini as part of the conversation.
4. **Repeat or answer:** it searches again, or writes the final answer. The loop stops after 8 steps at most.

## What was filled in

| Starter cell | Change |
| --- | --- |
| Documents ("User input needed") | Added 5 documents on coffee brewing, each with at least 4 sentences |
| Model (TODO) | Replaced the placeholder with `gemini-3.5-flash-lite` |
| Questions (TODO) | Added 3 questions, and kept the starter's 2 |

## Errors fixed

| Problem | Fix |
| --- | --- |
| `search_recipes` was called but never defined | Changed the calls to `search_documents` |
| The system prompt never reached the agent loop | Added it to the loop's config |
| `google-genai` was imported but not installed | Added it to the install cell |
| `model_content.parts` could be empty, though this did not happen in the run | Added a check as a precaution |

The loop also prints any reasoning text the model writes before it acts.

## Results from the run

| Question | What the agent did | Answer |
| --- | --- | --- |
| French press steep time and grind | 1 search | 4 minutes, coarse grind |
| Espresso or cold brew: finer grind and shorter brew | 2 searches | Espresso: fine, 25 to 30 seconds. Cold brew: coarse, 12 to 18 hours |
| Water for 30 g of pour-over coffee | 1 search, then a calculation | 480 g |
| Opened beans in the fridge, and when to grind | 2 searches in one step | Avoid the fridge, and grind just before brewing |
| How many ghosts eat carrots every day | 1 search | Said the documents don't cover it |
| Most popular food in Canada | 1 search | Said the documents don't cover it |

All cells ran without an error, and the final check cell passed.

## Limitations

- The model wrote a `Thought:` line on only 2 of the 6 questions. On the others, its reasoning shows only through the searches it chose.
- The search tool matches keywords, including parts of words, so it can return unrelated documents. For the ghost question it returned the pour-over document, and the agent still said the documents don't cover it.
- The install cell prints a pip dependency warning, and `google-generativeai` prints a notice that it's no longer supported. Neither stopped the run. The starter uses that package only to set up the model name.

## Run it yourself

1. Open the notebook in Colab.
2. Create a key on the [AI Studio API keys page](https://aistudio.google.com/apikey).
3. In Colab, open **Secrets** from the key icon and add it with the name `GOOGLE_API_KEY`.
4. Turn on **Notebook access** for that secret.
5. Choose **Runtime > Run all**.

Never put an API key in a code cell.

## Sources

- Starter notebook: [aditi-govindu/gemini-recipe-agent](https://github.com/aditi-govindu/gemini-recipe-agent)
- Original agent loop demo: [xiaosuhu/midas-ai-in-research](https://github.com/xiaosuhu/midas-ai-in-research/blob/v1.0-dev/docs/notebooks/agent_loop_demo.ipynb)
