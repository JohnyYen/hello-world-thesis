FROM danteev/texlive

# Install tools
RUN pip install arxiv-latex-cleaner==0.1.* \
                pdflinkchecker-cli==0.2.* --break-system-packages

# Install Node.js and Gemini CLI
RUN apt-get update && apt-get install -y nodejs npm
RUN npm install -g @google/gemini-cli