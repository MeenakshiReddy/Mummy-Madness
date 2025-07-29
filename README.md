# Halloween Heist 🎃👻

A thrilling Halloween-themed chase game built with Python and Tkinter where you must escape from pursuing ghosts while collecting points!

## 🎮 Game Overview

Halloween Heist is an interactive grid-based game where you play as a human character trying to survive as long as possible while being chased by intelligent ghosts (mummies). Navigate through a spooky desert landscape, collect points scattered across the grid, and see how long you can survive before getting caught!

## ✨ Features

- **Interactive Gameplay**: Use arrow keys to move your character around a 14x14 grid
- **Intelligent AI**: Ghosts use pathfinding algorithms to chase you strategically
- **Point Collection System**: Collect randomly distributed points (1-5 per grid cell) as you move
- **Text-to-Speech**: Audio narration for instructions and game over messages
- **Custom Graphics**: Halloween-themed sprites and backgrounds
- **Multiple Game Screens**: Title screen, instruction pages, and main game interface
- **Customizable Difficulty**: Set the number of ghosts and their starting positions

## 🚀 Getting Started

### Prerequisites

Make sure you have Python installed on your system along with the following packages:

```bash
pip install pyttsx3
```

Note: `tkinter`, `math`, `threading`, and `random` are part of Python's standard library.

### Required Assets

Before running the game, you'll need to prepare the following image files and update their paths in the code:

- `MUMMY_IMAGE_PATH`: Ghost/mummy sprite image
- `YOUR_IMAGE_PATH`: Human player character image  
- `DESERT_IMAGE_PATH`: Game background (desert theme)
- `START_BUTTON_IMAGE_PATH`: Start button image
- `NEXT_BUTTON_IMAGE_PATH`: Continue/Next button image
- `TITLE_IMAGE_PATH`: Title screen background
- `BACKGROUND_IMAGE_PATH`: Instructions background
- `INSTRUCTIONS_BG_IMAGE_PATH`: Instructions page 2 background

### Installation

1. Clone this repository:
```bash
git clone <repository-url>
cd halloween-heist
```

2. Update the image file paths in the code to match your asset locations

3. Run the game:
```bash
python halloween_heist.py
```

## 🎯 How to Play

1. **Start the Game**: Launch the application to see the title screen
2. **Read Instructions**: Navigate through the instruction pages to learn the controls
3. **Set Up Game**: 
   - Enter the number of ghosts you want to face
   - Specify their starting positions using the format: `x1,y1;x2,y2;x3,y3`
4. **Play**:
   - Use arrow keys (↑↓←→) to move your character
   - Collect yellow point numbers scattered across the grid
   - Avoid getting caught by the pursuing ghosts
   - Try to survive as long as possible!

## 🎮 Game Controls

- **↑ Arrow Key**: Move up
- **↓ Arrow Key**: Move down  
- **← Arrow Key**: Move left
- **→ Arrow Key**: Move right

## 🏆 Scoring System

- Points are randomly distributed across the grid (1-5 points per cell)
- Collect points by moving your character over them
- Your final score is displayed when the game ends
- Challenge yourself to beat your high score!

## 🔧 Technical Details

### Game Architecture

The game is built using object-oriented programming with the following main classes:

- **`TitlePage`**: Displays the game title and start button
- **`InstructionPage1`**: Shows basic game instructions with audio narration
- **`InstructionPage2`**: Allows players to configure game settings
- **`MummyChaseGUI`**: Main game interface and logic
- **`CustomMessageBox`**: Custom game over dialog

### Key Features

- **Grid-based Movement**: 14x14 coordinate system with smooth character positioning
- **AI Pathfinding**: Ghosts use distance calculations to move optimally toward the player
- **Real-time Updates**: Game state updates every 300ms for smooth gameplay
- **Audio Integration**: Text-to-speech for enhanced accessibility
- **Error Handling**: Input validation for ghost positions and game configuration

## 🛠️ Customization

You can easily customize the game by modifying:

- **Grid Size**: Change the range values in `draw_grid()` method
- **Game Speed**: Adjust the delay in `self.after(300, self.simulate_time_step)`
- **Point Values**: Modify the range in `random.randint(1, 5)`
- **Audio Speed**: Change the TTS rate in `engine.setProperty("rate", 230)`

## 🐛 Troubleshooting

### Common Issues

1. **Image Loading Errors**: Ensure all image file paths are correct and files exist
2. **Audio Issues**: Make sure `pyttsx3` is properly installed
3. **Input Format Errors**: Use the exact format for ghost positions: `x,y;x,y;x,y`

### System Requirements

- Python 3.6 or higher
- Windows/macOS/Linux with GUI support
- Audio output device (for text-to-speech features)

## 🤝 Contributing

Feel free to contribute to this project by:

- Adding new features or game modes
- Improving the AI behavior
- Creating better graphics or animations
- Optimizing performance
- Adding sound effects or background music

## 📝 License

This project is open source. Feel free to use, modify, and distribute as needed.

## 🎃 Happy Halloween!

Enjoy playing Halloween Heist and see how long you can survive the ghost chase! 👻🏃‍♂️

---

*Built with Python, Tkinter, and lots of Halloween spirit! 🎃*