# Airline Multi-Agent AI Assistant

The **Airline Multi-Agent AI Assistant** is an enhanced version of the Airline AI Assistant. It leverages **multi-agent architecture** and advanced tools to deliver an immersive and dynamic user experience. The assistant not only provides real-time flight ticket prices but also integrates AI-generated visuals and text-to-audio capabilities, making it more interactive and engaging.

---

## Features

### 🛫 **Real-Time Ticket Pricing**
- Retrieves ticket prices for flights to user-specified destinations.

### 🎨 **AI Image Generation**
- Creates vibrant, **pop-art-style images** of destinations using OpenAI's **DALL-E 3**.

### 🎤 **Text-to-Audio Conversion**
- Converts responses into lifelike audio using **OpenAI’s Text-to-Speech (TTS)** with customizable voices.

### 🧠 **Multi-Agent Integration**
- Combines AI agents for handling text queries, generating images, and synthesizing audio seamlessly.

---

## How It Works

1. **User Inquiry**:  
   The user enters a query, such as asking about ticket prices or a description of a city.

2. **Agent Handling**:  
   - Text queries are processed by the language model (LLM).  
   - If the query involves generating an image, the **DALL-E 3** agent creates a city-themed pop-art image.  
   - Responses are converted into audio using the **TTS agent**.

3. **Dynamic Outputs**:  
   The system provides a response as text, audio, and an image (if applicable).

---

## Code Overview

### **Main Multi-Agent Functionality**
The `chat` function integrates multiple agents to handle text, image, and audio generation:

```python
def chat(history):
    messages = [{"role": "system", "content": system_message}] + history
    response = openai.chat.completions.create(model=MODEL, messages=messages, tools=tools)
    image = None

    if response.choices[0].finish_reason == "tool_calls":
        message = response.choices[0].message
        response, city = handle_tool_call(message)
        messages.append(message)
        messages.append(response)
        image = artist(city)  # Generates a pop-art-style image of the destination
        response = openai.chat.completions.create(model=MODEL, messages=messages)
    
    reply = response.choices[0].message.content
    talker(reply)  # Converts the response to audio
    history += [{"role": "assistant", "content": reply}]    
    return history, image
```

### **Key Agent Functions**

1. **Artist (Image Generation)**:  
   Generates a vibrant, pop-art-style image of a city:
   ```python
   def artist(city):
       image_response = openai.images.generate(
           model="dall-e-3",
           prompt=f"An image representing a vacation in {city}, showing tourist spots and everything unique about {city}, in a vibrant pop-art style",
           size="1024x1024",
           n=1,
           response_format="b64_json",
       )
       image_base64 = image_response.data[0].b64_json
       image_data = base64.b64decode(image_base64)
       return Image.open(BytesIO(image_data))
   ```

2. **Talker (Text-to-Audio)**:  
   Converts AI-generated responses into audio:
   ```python
   def talker(message):
       response = openai.audio.speech.create(
           model="tts-1",
           voice="onyx",  # Try replacing "onyx" with "alloy" for variety
           input=message
       )
       audio_stream = BytesIO(response.content)
       audio = AudioSegment.from_file(audio_stream, format="mp3")
       play_audio(audio)
   ```

---

## Gradio Interface

The assistant is powered by a Gradio interface for a user-friendly experience, enabling interaction with text, audio, and images:

```python
gr.ChatInterface(fn=chat, type="messages").launch()
```

---

## Installation

1. Install dependencies:
   ```bash
   pip install openai gradio Pillow pydub
   ```
2. Ensure FFmpeg is installed and available in your system PATH. Follow [FFmpeg Installation Instructions](https://ffmpeg.org/download.html).

---

## Conclusion

The **Airline Multi-Agent AI Assistant** goes beyond text-based interactions, offering a **multimodal experience** with AI-generated images and speech synthesis. It’s highly extendable, ready for new tools like weather forecasting or itinerary planning, and ideal for building future-ready AI applications.

---
