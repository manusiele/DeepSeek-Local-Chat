# 🤖 DeepSeek Local Chat

A beautiful, lightweight web interface for chatting with DeepSeek AI models running locally on your machine. Built as a learning project to understand **RAG (Retrieval-Augmented Generation)** and local LLM deployment.

![DeepSeek Local Chat](https://img.shields.io/badge/AI-DeepSeek-purple)
![License](https://img.shields.io/badge/license-MIT-blue)
![Platform](https://img.shields.io/badge/platform-Windows-lightgrey)

## 🎯 Project Purpose

This project was created to:
- Learn and understand **RAG (Retrieval-Augmented Generation)** concepts
- Deploy and interact with large language models locally
- Build practical AI applications without relying on cloud APIs
- Maintain complete privacy and control over AI conversations
- Understand the fundamentals of local LLM inference

## ✨ Features

- 🎨 Beautiful gradient UI with smooth animations
- 💬 Real-time chat with typing indicators
- 🔒 100% Private - runs entirely on your machine
- ⚡ Fast local inference
- 🎯 Easy setup and deployment
- 📚 Foundation for RAG implementation

## 📚 What is RAG?

**RAG (Retrieval-Augmented Generation)** is a technique that enhances AI responses by:
1. **Retrieving** relevant information from a knowledge base
2. **Augmenting** the AI's context with this information
3. **Generating** more accurate, contextual responses

This project provides the foundation for implementing RAG by:
- Setting up a local LLM that can process custom context
- Creating an interface for real-time interaction
- Establishing a pipeline for future document retrieval integration

## 🚀 Quick Start

### Prerequisites

- Windows 10/11 (64-bit)
- 8GB+ RAM (16GB recommended)
- 6GB+ free disk space
- Modern web browser

### Installation Steps

**1. Download the DeepSeek Model**
   - Get the GGUF model file (~5GB)
   - Example: `DeepSeek-R1-Distill-Qwen-7B-Uncensored.i1-Q5_K_S.gguf`
   - Place it in a folder like `C:\Users\YourName\Ai-Model\`

**2. Download llama.cpp**
   - Visit [llama.cpp releases](https://github.com/ggerganov/llama.cpp/releases)
   - Download `llama-b####-bin-win-cpu-x64.zip`
   - Extract to a `llama` folder inside your model directory

**3. Clone This Repository**

```bash
git clone https://github.com/manusiele/DeepSeek-Local-Chat.git
```

Copy `chatbot.html` to your model directory.

**4. Folder Structure**

Your setup should look like this:

```
Ai-Model/
├── llama/
│   ├── llama-server.exe
│   └── (other files...)
├── DeepSeek-R1-Distill-Qwen-7B-Uncensored.i1-Q5_K_S.gguf
└── chatbot.html
```

**5. Start the Server**

Open Command Prompt:

```cmd
cd C:\path\to\Ai-Model\llama
llama-server.exe -m "..\DeepSeek-R1-Distill-Qwen-7B-Uncensored.i1-Q5_K_S.gguf" --port 8080
```

Keep this window open.

**6. Launch the Chat Interface**

Simply open `chatbot.html` in your browser and start chatting!

## 🎓 Understanding llama.cpp

### What is llama.cpp?

[**llama.cpp**](https://github.com/ggerganov/llama.cpp) is a C++ implementation of LLaMA inference with incredible optimization for consumer hardware. It's the backbone of this project.

**Key Features:**
- ⚡ **Ultra-fast inference** - Optimized C++ implementation
- 🎯 **CPU-friendly** - Runs efficiently on consumer CPUs
- 📊 **GPU acceleration** - Optional NVIDIA/AMD GPU support
- 💾 **Quantization support** - GGUF format for reduced memory usage
- 🔄 **HTTP server** - Built-in REST API via `llama-server`
- 🧵 **Multi-threaded** - Configurable CPU thread usage
- 🎮 **Context management** - Adjustable context windows up to 131,072 tokens

### llama-server API

This project uses `llama-server.exe` which provides:

**Completion Endpoint** (`POST /completion`)
```json
{
  "prompt": "Your message",
  "n_predict": 512,
  "temperature": 0.7,
  "top_p": 0.9,
  "repeat_penalty": 1.1,
  "top_k": 40
}
```

**Key Parameters:**
- `n_predict` - Maximum tokens to generate
- `temperature` - Randomness (0.0=deterministic, 1.0=creative)
- `top_p` - Nucleus sampling (0.0-1.0)
- `top_k` - Keep top K most likely tokens
- `repeat_penalty` - Prevent repetition

### Components Overview

**llama.cpp Infrastructure**
- Open-source C++ implementation for running LLMs locally
- Efficient inference engine optimized for consumer hardware
- Supports GGUF quantized model format for reduced memory footprint
- HTTP server mode for easy integration with web interfaces

**DeepSeek-R1 Model**
- 7B parameter language model from DeepSeek
- Quantized to Q5_K_S (5-bit quantization for balance of quality/speed)
- Uncensored version for unrestricted learning and experimentation
- ~5GB disk space after quantization

**Web Interface**
- Pure HTML/CSS/JavaScript (no heavy frameworks)
- Direct HTTP API communication with llama-server
- Real-time streaming responses
- Foundation for RAG implementation

### Next Steps for RAG Implementation

To extend this into a full RAG system, you can:

1. **Add Document Processing**
   - Parse PDFs, text files, or web pages
   - Split documents into chunks
   - Create embeddings for semantic search

2. **Implement Vector Database**
   - Store document embeddings
   - Enable similarity search
   - Retrieve relevant context based on queries

3. **Enhance Context Window**
   - Inject retrieved documents into prompts
   - Manage token limits effectively
   - Cite sources in responses

4. **Build Knowledge Base UI**
   - Upload and manage documents
   - View indexed content
   - Search your knowledge base

## 🛠️ Advanced Configuration

### Performance Optimization

The `details.txt` file includes multiple configuration profiles:

**For Fast Modern CPUs (8+ cores)**
```cmd
llama-server.exe -m "..\model.gguf" -c 8192 -t 8 -b 1024 --mlock -ub 8192 -n 2048
```
- `-c 8192` - Context window (supports up to 131,072 for this model)
- `-t 8` - Use 8 CPU threads
- `-b 1024` - Large batch size for faster processing
- `--mlock` - Lock model in RAM (prevents disk swapping)
- `-ub 8192` - User batch size for multiple requests
- `-n 2048` - Predict tokens in advance

**For GPU Acceleration (NVIDIA)**
```cmd
llama-server.exe -m "..\model.gguf" -c 8192 -t 6 -b 512 -ngl 35 --mlock
```
- `-ngl 35` - Offload 35 layers to GPU (adjust based on VRAM)
- Can achieve 50-100+ tokens/second

**For Slower PCs**
```cmd
llama-server.exe -m "..\model.gguf" -c 4096 -t 3 -b 256 --mlock --no-mmap
```
- Reduced context window and batch size
- More stable on limited hardware

### Customizing AI Behavior

Edit parameters in `chatbot.html`:

```javascript
const response = await fetch('http://localhost:8080/completion', {
  method: 'POST',
  body: JSON.stringify({
    prompt: userMessage,
    n_predict: 512,        // Response length
    temperature: 0.7,      // Creativity (0.0 = focused, 1.0 = creative)
    top_p: 0.9,           // Diversity of responses (0.0-1.0)
    top_k: 40,            // Keep top K tokens
    repeat_penalty: 1.1   // Prevent repetition
  })
});
```

**Parameter Guide:**
- `n_predict`: Higher = longer responses (uses more tokens)
- `temperature`: 0.0-0.3 for factual answers, 0.7-1.0 for creative content
- `top_p`: Lower = more focused, Higher = more diverse
- `top_k`: Lower = more deterministic, Higher = more random
- `repeat_penalty`: 1.0 = no penalty, >1.0 = discourage repetition

## 🛠️ Customization

### Adjusting AI Behavior

Edit the API call in `chatbot.html`:

```javascript
n_predict: 512,        // Response length
temperature: 0.7,      // Creativity (0.0 = focused, 1.0 = creative)
top_p: 0.9,           // Diversity of responses
```

### Server Configuration

Common parameters for `llama-server.exe`:

- `--port 8080` - Change server port
- `--ctx-size 4096` - Adjust context window
- `--threads 4` - Set CPU threads to use

## 🐛 Troubleshooting

**Server won't start**
- Verify you're in the `llama` folder
- Check the model path is correct

**Browser can't connect**
- Ensure server is running
- Verify it's on port 8080
- Check firewall settings

**Slow responses**
- Close other applications
- Use a smaller/more quantized model
- Increase thread count if you have more CPU cores

## 🤝 Contributing

This is a learning project! Contributions are welcome:
- Report bugs or issues
- Suggest improvements
- Share your RAG implementations
- Add documentation

## 📖 Additional Resources

- [llama.cpp Documentation](https://github.com/ggerganov/llama.cpp)
- [Understanding RAG](https://www.pinecone.io/learn/retrieval-augmented-generation/)
- [DeepSeek Models](https://huggingface.co/deepseek-ai)
- [Building RAG Systems](https://www.langchain.com/)

## 📝 License

MIT License - feel free to use this for learning and building!

## 🙏 Acknowledgments

- llama.cpp team for the incredible inference engine
- DeepSeek for the open-source model
- The open-source AI community

---

**Built with ❤️ for learning AI and RAG concepts**

*Questions or suggestions? Open an issue or reach out!*