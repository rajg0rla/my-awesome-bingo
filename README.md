#  Soc Ops  Social Bingo for Mixers

**Break the ice. Meet new people. Win at bingo.** 

Soc Ops is a delightful social bingo game designed for in-person mixers, conferences, and team events. Mark off squares by finding people who match each prompt, complete a line (5 in a row horizontally, vertically, or diagonally), and claim your victory! 

> Built with modern web tech (React 19 + TypeScript + Vite) and ready to deploy to GitHub Pages with zero configuration.

---

##  Features

-  **Randomized board**  Each game generates a unique 55 bingo card
-  **Persistent progress**  Your board is saved automatically. Return later and pick up where you left!
-  **Mobile-friendly**  Play on phones, tablets, or desktop
-  **Instant deployment**  Push to GitHub and your game is live on GitHub Pages
-  **Fully customizable**  Swap in your own questions, colors, and styling
-  **Fast & smooth**  Built with Vite for instant hot reload during dev

---

##  How to Play

1. **Load the game**  Open the app in your browser
2. **Start a game**  Click "Play Bingo" to begin
3. **Find matches**  Mingle with people at the mixer. When you find someone who matches a prompt (e.g., "Has worked at 3+ companies"), tap that square to mark it 
4. **Get 5 in a row**  Mark 5 squares horizontally, vertically, or diagonally (note: the center square is a FREE SPACE!)
5. **Win**  BINGO! 

---

##  Quick Start

### Prerequisites
- [Node.js 22](https://nodejs.org/) or higher

### Development
\\\ash
npm install
npm run dev
\\\
Your game will open in the browser with hot reload enabled. Edit questions, styles, or components and see changes instantly.

### Build & Deploy
\\\ash
npm run build
\\\
Push to \main\ and your game automatically deploys to GitHub Pages! (Make sure [Pages is configured](https://docs.github.com/en/pages/getting-started-with-github-pages) to deploy from GitHub Actions.)

---

##  Customize Your Game

Make it uniquely yours:

- **Questions**  Edit [src/data/questions.ts](src/data/questions.ts) to customize prompts
- **Styling**  Modify [Tailwind CSS](src/index.css) classes in components for your look & feel
- **Branding**  Update the title, colors, and fonts to match your event

See [.github/copilot-instructions.md](.github/copilot-instructions.md) for architecture details and development patterns.

---

##  Lab & Learning

Interested in how this game was built or want to extend it? 

 **[Follow the Lab Guide](.lab/GUIDE.md)** for a structured workshop on:
- Setting up AI-guided development
- Context engineering your codebase
- Design-first frontend work
- TDD workflows with AI agents
- Building custom question themes

---

##  Tech Stack

- **React 19**  Modern UI with hooks
- **TypeScript**  Type-safe components and logic
- **Vite**  Lightning-fast builds and dev server
- **Tailwind CSS v4**  Utility-first styling
- **Vitest**  Fast unit testing

---

##  License

MIT  Use, modify, and share freely!

---

**Ready to spread the vibes?** Clone this repo, customize your questions, and host an unforgettable mixer. Have fun! 
