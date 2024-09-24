<%*
/*
```js*/
try {
  if (window.ewm) { //window.ewm is populated by the Excalidraw Writing Machine script
    const systemMessage = 
      "You are an expert in academic writing with a strong command of structure, clarity, and logical argumentation. " +
      "Your task is to transform the provided draft into a well-organized, insightful academic article. " +
      "Ensure that the final article is clear, coherent, and follows a logical structure aligned with the draft. " +
      "Integrate all provided illustrations, key ideas and insights from the draft while enhancing them with examples and smoother transitions. " +
      "Return the article in Markdown format, preserving all provided image embed links exactly as they are to avoid any disruption. " +
      "DO NOT MODIFY THE IMAGE LINKS!";
    const userPrompt = 
      "Write a well-structured academic article based on the draft provided below. " +
      "Adhere closely to the content and logical flow of the draft, but extend and embellish the text by " +
      "including relevant examples, adding transitions and connecting text to ensure smooth progression of the argument, and " +
      "enhancing details that improve readability and clarity. If provided in the draft, include the provided image links in the final article. " +
      "Maintain a professional and insightful tone suitable for an academic audience. " +
      "Begin the article with an Abstract and end with a Conclusions section. The Conclusions should " +
      "provide practical, actionable takeaways and a call to action for the reader." +
      "\n\n---\n\n" + window.ewm;
    const model =  "chatgpt-4o-latest"; //"chatgpt-4o-latest", "gpt-4o-mini"; pricing: https://openai.com/api/pricing/
    const maxReturnTokens = 16384; //For info on limits, see: https://platform.openai.com/docs/models
    const maxOutgoingCharacters = 40000;
    
    const response = await tp.ai.chat(
      userPrompt,
      model,
      systemMessage,
      maxReturnTokens,
      maxOutgoingCharacters
    );

    window.ewmResponse = {systemMessage, userPrompt, model, maxReturnTokens, maxOutgoingCharacters, response};

    if (response) {
      tR += response;
    }
  }
} catch (e) {
  new Notice(`Error processing the article: ${e}`);
  console.log(e);
}
/*
```
*/

%>

# Original Draft

<<<REPLACE ME>>>