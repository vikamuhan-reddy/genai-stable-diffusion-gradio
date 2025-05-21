## Prototype Development for Image Generation Using the Stable Diffusion Model and Gradio Framework

### AIM:
To design and deploy a prototype application for image generation utilizing the Stable Diffusion model, integrated with the Gradio UI framework for interactive user engagement and evaluation.

### PROBLEM STATEMENT:
The problem is to develop a prototype that generates high-quality images from text prompts using the Stable Diffusion model. It should include an interactive, user-friendly interface built with Gradio for real-time input and output. The goal is to create a seamless and engaging user experience for text-to-image generation.

### DESIGN STEPS:

#### STEP 1:
Identify and install all necessary software tools and libraries required for image generation and UI development.Ensure the system is compatible with GPU acceleration for improved performance during image processing.Set up access to the Stable Diffusion model through a reliable source or platform.

#### STEP 2:
Load and configure the pre-trained Stable Diffusion model to accept textual input and output corresponding image results.Optimize the model pipeline for efficient image generation, considering both speed and quality.Ensure appropriate handling of model resources to prevent performance bottlenecks.

#### STEP 3:
Design a clean and simple user interface using the Gradio framework.Integrate a text input field for users to enter prompts and a display area for showing generated images.Add descriptive titles and instructions to guide users effectively through the interface.

### STEP 4:
Test the full pipeline to ensure smooth operation from user input to image generation and display.Validate the output images for quality and consistency with the given prompts.Refine the user experience based on feedback or observed issues during testing.

### STEP 5:
Deploy the application for public or internal access using local hosting or a cloud-based platform.Ensure security, accessibility, and scalability of the deployed system.Monitor performance and make iterative improvements as needed.

### PROGRAM:
```py
import os
import io
import IPython.display
from PIL import Image
import base64 
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']

import requests, json
def get_completion(inputs, parameters=None, ENDPOINT_URL=os.environ['HF_API_TTI_BASE']):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }   
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))

import gradio as gr 

#A helper function to convert the PIL image to base64
#so you can send it to the API
def base64_to_pil(img_base64):
    base64_decoded = base64.b64decode(img_base64)
    byte_stream = io.BytesIO(base64_decoded)
    pil_image = Image.open(byte_stream)
    return pil_image

def generate(prompt):
    output = get_completion(prompt)
    result_image = base64_to_pil(output)
    return result_image

gr.close_all()
demo = gr.Interface(fn=generate,
                    inputs=[gr.Textbox(label="Your prompt")],
                    outputs=[gr.Image(label="Result")],
                    title="Image Generation with Stable Diffusion",
                    description="Generate any image with Stable Diffusion",
                    allow_flagging="never",
                    examples=["the spirit of a tamagotchi wandering in the city of Vienna","a mecha robot in a favela"])

demo.launch(share=True, server_port=int(os.environ['PORT1']))
```

### OUTPUT:
<img width="1050" alt="Screen Shot 1947-02-31 at 07 28 00" src="https://github.com/user-attachments/assets/811ce6e8-b9b8-48d6-943d-ea0e5992d266" />



### RESULT:
The prototype enables high-quality image generation from text prompts using the Stable Diffusion model. It features an intuitive Gradio interface and is ready for deployment and further development.
