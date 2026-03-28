---
name: validate-glsl-playwright
description: Uses a headless Playwright browser to render WebGL canvases and explicitly intercept GLSL shader compilation errors.
category: testing
---

# GLSL Playwright Validation Workflow

## 1. When to use this skill
Use this skill whenever asked to test, validate, or debug the rendering of a LICHEN canvas, WebGL context, or GLSL shader.

## 2. Guardrails & Constraints
- **CRITICAL:** Do not use JSDOM or static string validation for these tests. You must write and execute a Playwright script.
- **CRITICAL:** You must launch the Chromium browser with hardware acceleration disabled to prevent crash loops in the headless container. Use these exact launch arguments: `args: ['--use-gl=egl', '--use-angle=swiftshader']`.

## 3. Execution Steps
1. **Write the Test Runner:** Use the `write_file` tool to create a temporary test script (e.g., `run-glsl-test.js`). The script must:
   - Import Playwright and launch Chromium with the required WebGL-friendly flags.
   - Attach a `page.on('console', msg => {...})` event listener. 
   - The listener must explicitly flag any console message containing "ERROR: 0:" or "WARNING: 0:" as a shader compilation failure.
   - Load the target local HTML file using `page.goto('file://' + absolutePath)`.
   - Wait briefly (e.g., 500ms) to allow the `requestAnimationFrame` loop to attempt its first render pass.
   - Close the browser cleanly.
2. **Execute the Test:** Run the script using `node run-glsl-test.js` via the `terminal` tool.
3. **Analyze the Output:** Read the terminal output. If the intercepted shader compilation errors triggered, analyze the traceback to identify exactly which line of the GLSL string failed (e.g., a missing semicolon or a type mismatch).
4. **Report & Fix:** Provide a concise technical breakdown of why the shader failed to compile. If the solution is obvious, use the `write_file` tool to autonomously patch the user's source code and re-run the test to confirm the fix.

