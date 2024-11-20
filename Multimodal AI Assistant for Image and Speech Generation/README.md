# Multimodal AI Assistant for Image and Speech Generation  

## Description  
This project combines advanced AI techniques for image generation and text-to-speech synthesis. Using FluxPipeline for surreal-style image generation and SpeechT5 for lifelike voice synthesis, the application delivers a powerful and versatile AI experience.  

## Features  
1. **Image Generation**:  
   - Generates AI art using the FluxPipeline (`black-forest-labs/FLUX.1-schnell`) model.  
   - Produces high-quality surreal-style visuals from user-defined text prompts.  

2. **Text-to-Speech (TTS) Synthesis**:  
   - Leverages `microsoft/speecht5_tts` to create natural-sounding speech.  
   - Includes customizable voice embeddings for speaker personalization.  

## Technologies Used  
### Libraries and Models  
- **PyTorch**: For tensor computations and GPU acceleration.  
- **Diffusers**: For image generation using the FluxPipeline model.  
- **Transformers**: For TTS synthesis with SpeechT5.  
- **Datasets**: For loading voice embeddings (e.g., `Matthijs/cmu-arctic-xvectors`).  
- **Soundfile**: To save audio output as WAV files.  

### Models  
1. **FluxPipeline**:  
   - Model: `black-forest-labs/FLUX.1-schnell`  
   - Application: Generates surreal-style images with detailed prompt control.  
   - GPU Support: Optimized using bfloat16 for efficiency.  

2. **SpeechT5**:  
   - Model: `microsoft/speecht5_tts`  
   - Application: Converts text to speech with speaker embedding customization.  

### Key Processes  
#### Image Generation  
- Define text prompts to guide the visual style of generated images.  
- Use a reproducible random generator to ensure consistent results.  
- Parameters like guidance scale and inference steps allow control over image refinement.  

#### Text-to-Speech  
- Supports speaker embeddings for personalized voice synthesis.  
- Outputs high-quality WAV audio files.  

## Installation  

### Prerequisites  
- Python 3.8+  
- GPU with CUDA support (recommended).  

### Dependencies  
Install the required Python libraries:  
```bash  
pip install torch diffusers transformers datasets soundfile  
```  
Ensure **FFmpeg** is installed for audio handling.  

### Usage  

#### 1. Image Generation  
```python  
from diffusers import FluxPipeline  
pipe = FluxPipeline.from_pretrained("black-forest-labs/FLUX.1-schnell", torch_dtype=torch.bfloat16).to("cuda")  
image = pipe(prompt="A futuristic class learning AI in Salvador Dali style", num_inference_steps=4).images[0]  
image.save("surreal.png")  
```  

#### 2. Text-to-Speech  
```python  
from transformers import pipeline  
synthesiser = pipeline("text-to-speech", "microsoft/speecht5_tts", device="cuda")  
speech = synthesiser("Hello, AI world!", forward_params={"speaker_embeddings": speaker_embedding})  
sf.write("speech.wav", speech["audio"], samplerate=speech["sampling_rate"])  
```  

## Notes for Future Reference  
- **Model Optimization**: Consider fine-tuning `FluxPipeline` or `SpeechT5` for domain-specific use cases.  
- **Speaker Embeddings**: Ensure proper dataset preprocessing for embedding compatibility.  
- **Performance**: Experiment with inference step parameters to balance speed and quality.  
- **Extensibility**: Integrate more advanced pipelines or APIs for additional features, such as real-time speech streaming or multi-language support.  

## License  
This project is under the MIT License.  

## Author  
Cherki Meziane  
