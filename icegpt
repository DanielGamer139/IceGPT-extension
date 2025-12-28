(function(Scratch) {
  'use strict';

  class IceGPT {
    constructor(runtime) {
      this.runtime = runtime;
      this.lastResponse = "";
      this.history = [];
    }

    getInfo() {
      return {
        id: 'icegpt',
        name: 'IceGPT',
        blocks: [
          {
            opcode: 'ask',
            blockType: Scratch.BlockType.COMMAND,
            text: 'ask IceGPT [TEXT]',
            arguments: {
              TEXT: { type: Scratch.ArgumentType.STRING, defaultValue: 'Hello IceGPT!' }
            }
          },
          {
            opcode: 'quickAsk',
            blockType: Scratch.BlockType.REPORTER,
            text: 'quick ask IceGPT [TEXT]',
            arguments: {
              TEXT: { type: Scratch.ArgumentType.STRING, defaultValue: 'Say something' }
            }
          },
          {
            opcode: 'latestResponse',
            blockType: Scratch.BlockType.REPORTER,
            text: 'latest response'
          },
          {
            opcode: 'clearMemory',
            blockType: Scratch.BlockType.COMMAND,
            text: 'clear IceGPT memory'
          }
        ]
      };
    }

    async ask(args) {
      const prompt = args.TEXT;

      const body = {
        model: "llama-3.1-8b-instant",
        messages: [
          ...this.history,
          { role: "user", content: prompt }
        ]
      };

      try {
        const response = await fetch("https://penguin-ai-groq.danielmat639.workers.dev/v1/chat/completions", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify(body)
        });

        const data = await response.json();
        console.log("DEBUG:", data);

        if (data && data.choices && data.choices[0] && data.choices[0].message) {
          const reply = data.choices[0].message.content;

          this.lastResponse = reply;

          this.history.push({ role: "user", content: prompt });
          this.history.push({ role: "assistant", content: reply });
        } else {
          this.lastResponse = "IceGPT Error: " + JSON.stringify(data);
        }

      } catch (e) {
        this.lastResponse = "IceGPT Error: " + e;
      }
    }

    async quickAsk(args) {
      await this.ask(args);
      return this.lastResponse;
    }

    latestResponse() {
      return this.lastResponse;
    }

    clearMemory() {
      this.history = [];
      this.lastResponse = "";
    }
  }

  Scratch.extensions.register(new IceGPT());
})(Scratch);
