---
name: init-web-workspace
description: Scaffolds a creative coding HTML5/Canvas boilerplate in the workspace and starts a background HTTP server on port 8080.
category: web-development
---

# Web Workspace Initialization Workflow

## 1. When to use this skill
Use this skill whenever asked to "initialize a web project", "setup a live-coding canvas", or "start the web server".

## 2. Guardrails & Constraints
- Always use the `write_file` tool to create the files in the `/workspace` directory.
- **CRITICAL:** You MUST append `&` when running the Python server command so it detaches and runs in the background. If you do not, the terminal will hang indefinitely.

## 3. Execution Steps
1. **Scaffold the HTML:** Use the `write_file` tool to create an `index.html` file. Write a standard HTML5 boilerplate that includes a full-screen `<canvas id="mainCanvas"></canvas>` element and a `<script>` tag with a basic requestAnimationFrame render loop.
2. **Start the Server:** Use the `terminal` tool to execute `python3 -m http.server 8080 &`. 
3. **Verify:** Use the `terminal` tool to quickly run `ps aux | grep http.server` to confirm the background process successfully started and did not instantly crash.
4. **Report:** Inform the user that the boilerplate is ready and the server is live. Tell them to navigate to `http://<YOUR_PI_IP>:8080` in their workstation's browser to see the live canvas.
