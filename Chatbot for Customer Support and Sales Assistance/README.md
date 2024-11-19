# Chatbot for Customer Support and Sales Assistance

This project demonstrates a simple chatbot created using OpenAI's API in a Jupyter notebook. The chatbot is designed to assist customers in a clothing store, with a focus on promoting items on sale, like hats.

## Chatbot 1: Basic Helpful Assistant

The first chatbot is a general-purpose assistant. Its system message is:

```python
system_message = "You are a helpful assistant."
```

This chatbot responds to user queries with helpful information but lacks specific context.

## Chatbot 2: Clothing Store Assistant

The second chatbot is more specialized. It encourages customers to buy discounted hats (60% off) and reminds them to look at hats if they ask about shoes. The system message for this assistant is:

```python
system_message = "You are a helpful assistant in a clothes store. You should try to gently encourage the customer to try items that are on sale. Hats are 60% off, and most other items are 50% off. For example, if the customer says 'I'm looking to buy a hat', you could reply something like, 'Wonderful - we have lots of hats - including several that are part of our sales event.'"
```

If the customer asks about shoes, the assistant responds:

```python
system_message += "\nIf the customer asks for shoes, you should respond that shoes are not on sale today, but remind the customer to look at hats!"
```

### Key Features:
- Encourages sales of hats and directs customers to them.
- Provides personalized responses based on customer queries.
- Handles sale information dynamically.

## Chatbot Functionality

The chat function takes the user's message and history, appends it to the conversation, and calls OpenAI’s API to generate the next response:

```python
def chat(message, history):
    messages = [{"role": "system", "content": system_message}] + history + [{"role": "user", "content": message}]

    # Special handling for "belt" mention
    if 'belt' in message:
        messages.append({"role": "system", "content": "For added context, the store does not sell belts, but be sure to point out other items on sale."})
    
    messages.append({"role": "user", "content": message})

    stream = openai.chat.completions.create(model=MODEL, messages=messages, stream=True)

    response = ""
    for chunk in stream:
        response += chunk.choices[0].delta.content or ''
        yield response
```

### How It Works:
1. The chatbot appends each user message to the conversation history.
2. If the word “belt” appears, it adds additional context to the conversation.
3. It streams responses back to the user in real-time.

### Example Interaction

**User:** "I'm looking to buy a hat."  
**Chatbot:** "Wonderful - we have lots of hats, including several that are part of our sales event. Would you like to see some of them?"

**User:** "Do you sell belts?"  
**Chatbot:** "For added context, the store does not sell belts, but be sure to check out the other items on sale, especially our hats!"

**User:** "What about shoes?"  
**Chatbot:** "Shoes are not on sale today, but be sure to check out our amazing selection of hats on sale!"

## Gradio UI (Optional)

You can also create a Gradio-based user interface to interact with the chatbot. Below is an example image of how the Gradio interface might look:

![Description of Image](LLM-model/images/Immagine-2024-11-19-171210.png)

## Conclusion

This notebook shows how to build a chatbot using OpenAI's API with custom system prompts to guide customer interactions. It demonstrates:
- Basic chatbot functionality with conversation history.
- Customizing responses based on specific scenarios (e.g., sales promotions).
- Handling dynamic interactions in real-time.

