---
{"dg-publish":true,"tags":["AI","docker","docker-compose","container"],"permalink":"/developer/artificial-intelligence/ollama-with-docker-compose/","dgPassFrontmatter":true}
---

Get the [Ollama](https://ollama.com/)app running on your local machine, but containerized by [[developer/Home Lab/Docker\|Docker]] for better flexibility in deployment and portability

I'm running this on my [[developer/Hardware/MacBook Pro M1 Laptop\|MacBook Pro M1 Laptop]]

### Create a Directory
```bash
mdkir ~/docker/ollama-container
cd ~/docker/ollama-container
```

### CPU Only (no GPU)
```bash
docker run -d -v ./ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
```

### Installing Models
```bash
docker exec -it ollama ollama run codellama
docker exec -it ollama ollama run codellama:7b-instruct
```

### Create an Alias
If using a [[developer/Linux/Linux\|Linux]] machine with [[developer/Linux/Bash Scripting\|Bash]]. This will create a command line shortcut for a more convenient execution of this container
```bash
# setup the alias
alias ollama="docker exec -it ollama ollama"

# using the alias
ollama run codellama
```

### Exit Chatbot
```bash
/bye
```

## Code Editor Extension
currently trying to get this hooked up with [rjmacarthy/twinny](https://github.com/rjmacarthy/twinny) [[developer/VS Code\|VS Code]] extension, but having some trouble. Currently following this [issue](https://github.com/rjmacarthy/twinny/issues/74)to find out how to use it. For now I just installed the native **Ollama** app to my desktop (which pretty much defeats the purpos of this writeup for me lol)

---
## Credits
- [ollama/ollama - Docker Image | Docker Hub](https://hub.docker.com/r/ollama/ollama
- [How to run Ollama on Docker - YouTube](https://www.youtube.com/watch?v=ZoxJcPkjirs)
- [[developer/developer_box📦\|developer_box📦]]