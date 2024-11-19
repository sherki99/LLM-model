# Airline AI Assistant

The **Airline AI Assistant** leverages **Large Language Models (LLMs)** to provide real-time flight ticket pricing based on user queries. The assistant integrates with external tools to fetch ticket prices dynamically and respond to user inquiries.

## Features

- **Real-Time Ticket Pricing**: Provides ticket prices for return flights to specified cities.
- **LLM Integration**: Uses OpenAI’s GPT model to process natural language inputs and invoke the relevant tools.
- **Tool-based Functionality**: Integrates custom functions to fetch external data, like ticket prices.
- **Expandable**: The assistant can be extended with additional tools, such as flight availability or weather information.

## How It Works

1. **User Inquiry**: The user asks about the price of a ticket to a specific city.
2. **LLM Processing**: The GPT model processes the request and triggers the appropriate tool.
3. **Tool Call**: The `get_ticket_price` function is called with the destination city.
4. **Data Retrieval**: The tool fetches the ticket price from the dataset or external API.
5. **Response**: The LLM responds with the price of the ticket to the specified city.

## Code Overview

### Main Function

The `chat` function handles user queries, processes them, and invokes the tool if needed:

```python
def chat(message, history):
    messages = [{"role": "system", "content": system_message}] + history + [{"role": "user", "content": message}]
    response = openai.chat.completions.create(model=MODEL, messages=messages, tools=tools)
    
    if response.choices[0].finish_reason == "tool_calls":
        message = response.choices[0].message
        response, city = handle_tool_call(message)
        messages.append(message)
        messages.append(response)
        response = openai.chat.completions.create(model=MODEL, messages=messages)
    
    return response.choices[0].message.content
```

### Tool Call Handler

The `handle_tool_call` function processes the tool call, retrieves the ticket price, and returns the response:

```python
def handle_tool_call(message):
    tool_call = message.tool_calls[0]
    arguments = json.loads(tool_call.function.arguments)
    destination_city = arguments.get('destination_city')
    ticket_price = get_ticket_price(destination_city)
    
    response = {
        "role": "tool",
        "content": json.dumps({
            "destination_city": destination_city,
            "price": ticket_price
        }),
        "tool_call_id": tool_call.id
    }
    
    return response, destination_city
```

### Launch Interface

The **Gradio** interface is used to allow users to interact with the assistant:

```python
gr.ChatInterface(fn=chat, type="messages").launch()
```

## Dependencies

Install the required dependencies:

```bash
pip install openai gradio
```

## Conclusion

The **Airline AI Assistant** provides a simple, powerful solution for retrieving flight ticket prices using AI and external tools. It is designed to be extendable and easily adaptable for future enhancements.
