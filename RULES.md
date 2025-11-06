# Rules for All Agents

***STRICTLY FOLLOW THESE RULES TO ENSURE SAFE AND EFFECTIVE OPERATION WITHIN THE SUPERCODE FRAMEWORK.***

# Generation Rules:
1. NEVER overengineer solutions; prioritize simplicity and maintainability.
2. NEVER reach over 100 < x < 130 lines of code in a single generation. <!-- Specific to command, mcp, or agent implementations. -->
3. REMOVE `developer comments e.g <!-- comment || quantification expression -->` from final code generations.
4. ALWAYS store generated .md files in the `/opendocs/general` folder if not specified otherwise.
5. ALWAYS - the qa-engineer agent must store his files in the `/opendocs/qa` folder.
6. ALWAYS ensure generated code adheres to project coding standards and best practices.

# Agent Safety Rules:
1. ALWAYS execute shell commands via Desktop Commander MCP.
2. ALWAYS prioritize user safety and security when executing shell commands via MCPs.
3. IF UNSURE about the safety of a shell command, THEN SEEK CLARIFICATION before executing it.

# Bash/NPM Command Rules:
1. NEVER run `npm run build` in the root folder.
2. NEVER run `npm run lint` via Knowledge-Graph MCP.
3. NEVER run `npm run dev` via Knowledge-Graph MCP.
4. ALWAYS ensure the current working directory is correct BEFORE executing any bash/npm commands.
5. ALWAYS terminate any running development server BEFORE starting a new one.
6. ALWAYS terminate running servers before ending your session.
