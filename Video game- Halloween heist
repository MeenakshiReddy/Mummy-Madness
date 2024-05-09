import tkinter as tk
import threading
import pyttsx3
import math  # Importing math module
import random  # Importing random module

# Replace the following image paths with your actual file paths
MUMMY_IMAGE_PATH = r"C:\Users\namra\Downloads\rsz_4mm_1_1.png"
YOUR_IMAGE_PATH = r"C:\Users\namra\Downloads\human image.png"
DESERT_IMAGE_PATH = r"C:\Users\namra\Downloads\bg12345.png"
START_BUTTON_IMAGE_PATH = r"C:\Users\namra\Pictures\continue.png"
NEXT_BUTTON_IMAGE_PATH = r"C:\Users\namra\Pictures\continue.png"
TITLE_IMAGE_PATH = r"C:\Users\namra\Downloads\rsz_1halloween_hiest_1.png"
BACKGROUND_IMAGE_PATH = r"C:\Users\namra\Downloads\bg123.png"  # Replace with actual background image path
INSTRUCTIONS_BG_IMAGE_PATH = r"C:\Users\namra\Downloads\bg123.png"  # New background image for instructions page 2

class CustomMessageBox(tk.Toplevel):
    def __init__(self, message):
        super().__init__()
        self.title("Game Over")
        self.geometry("300x300")
        label = tk.Label(self, text=message, font=("Arial", 20, "bold"), fg="white")
        label.pack(expand=True)


class MummyChaseGUI(tk.Tk):
    def __init__(self, mummy_positions):
        super().__init__()

        self.title("Mummy Chase Simulation")
        self.geometry("1400x1400")

        self.mummy_image = tk.PhotoImage(file=MUMMY_IMAGE_PATH)
        self.your_image = tk.PhotoImage(file=YOUR_IMAGE_PATH)

        self.mummy_positions = mummy_positions
        self.your_position = (0, 0)
        self.time_steps = 0
        self.points_collected = 0  # Initialize points collected

        self.canvas = tk.Canvas(self, width=1000, height=1000, bg="black")
        self.canvas.pack(side=tk.TOP, fill=tk.BOTH, expand=True)

        self.desert_image = tk.PhotoImage(file=DESERT_IMAGE_PATH)
        self.canvas.create_image(0, 0, image=self.desert_image, anchor=tk.NW)

        self.draw_grid()
        self.draw_characters()
        self.draw_points()  # Draw points on the grid

        self.bind("<Left>", self.move_left)
        self.bind("<Right>", self.move_right)
        self.bind("<Up>", self.move_up)
        self.bind("<Down>", self.move_down)

        self.game_started = False

    def draw_grid(self):
        for x in range(-6, 8):
            for y in range(-6, 8):
                x0, y0 = 115 * (x + 6), 50 * (y + 6)
                x1, y1 = 115 * (x + 7), 50 * (y + 7)
                self.canvas.create_rectangle(x0, y0, x1, y1, outline="white")

    def draw_characters(self):
        self.canvas.delete("characters")
        for mummy_pos in self.mummy_positions:
            self.canvas.create_image(115 * (mummy_pos[0] + 6) + 57, 50 * (mummy_pos[1] + 6) + 25, image=self.mummy_image, tags="characters")

        self.canvas.create_image(115 * (self.your_position[0] + 6) + 57, 50 * (self.your_position[1] + 6) + 25, image=self.your_image, tags="characters")

    def draw_points(self):
        for x in range(-6, 8):
            for y in range(-6, 8):
                if (x, y) != self.your_position and (x, y) not in self.mummy_positions:
                    # Randomly generate points (between 1 to 3) for each grid
                    points = random.randint(1, 5)
                    self.canvas.create_text(115 * (x + 6) + 57, 50 * (y + 6) + 25, text=str(points), fill="yellow", font=("Arial", 15, "bold"))

    def move_towards_you(self, mummy_pos):
        x, y = mummy_pos
        dx = self.your_position[0] - x
        dy = self.your_position[1] - y
        distance = math.sqrt(dx ** 2 + dy ** 2)

        if distance == 0:
            return (x, y)

        move_x = x + dx / distance
        move_y = y + dy / distance

        return (int(round(move_x)), int(round(move_y)))

    def move_left(self, event):
        self.your_position = (self.your_position[0] - 1, self.your_position[1])
        self.collect_points()  # Collect points when moving
        if not self.game_started:
            self.start_game()
        self.draw_characters()

    def move_right(self, event):
        self.your_position = (self.your_position[0] + 1, self.your_position[1])
        self.collect_points()
        if not self.game_started:
            self.start_game()
        self.draw_characters()

    def move_up(self, event):
        self.your_position = (self.your_position[0], self.your_position[1] - 1)
        self.collect_points()
        if not self.game_started:
            self.start_game()
        self.draw_characters()

    def move_down(self, event):
        self.your_position = (self.your_position[0], self.your_position[1] + 1)
        self.collect_points()
        if not self.game_started:
            self.start_game()
        self.draw_characters()

    def start_game(self):
        self.game_started = True
        self.simulate_time_step()

    def simulate_time_step(self):
        new_mummy_positions = []
        for mummy_pos in self.mummy_positions:
            new_mummy_pos = self.move_towards_you(mummy_pos)
            new_mummy_positions.append(new_mummy_pos)

        new_human_pos = self.your_position

        for i, mummy_pos in enumerate(new_mummy_positions):
            self.mummy_positions[i] = mummy_pos

        self.your_position = new_human_pos
        self.time_steps += 1
        self.draw_characters()

        if self.your_position in self.mummy_positions:
            self.show_steps_message()
        else:
            self.after(300, self.simulate_time_step)

    def show_steps_message(self):
        message = f"CAUGHT IN {self.time_steps} STEPS!!\nPoints collected: {self.points_collected}"
        CustomMessageBox(message)
        self.speak_steps_message(message)

    def speak_steps_message(self, message):
        engine = pyttsx3.init()
        engine.say(message)
        engine.runAndWait()

    def collect_points(self):
        x, y = self.your_position
        item = self.canvas.find_closest(115 * (x + 6) + 57, 50 * (y + 6) + 25)  # Find the closest text item (point)
        tags = self.canvas.gettags(item)
        if "characters" not in tags:
            points = int(self.canvas.itemcget(item, "text"))  # Get the points from the text item
            self.points_collected += points
            self.canvas.delete(item)  # Remove the collected points from the canvas

class InstructionPage1(tk.Tk):
    def __init__(self):
        super().__init__()

        self.title("Mummy Madness - Instructions - Page 1")
        self.geometry("1400x1400")

        # Add background image
        self.background_image = tk.PhotoImage(file=BACKGROUND_IMAGE_PATH)
        self.background_label = tk.Label(self, image=self.background_image)
        self.background_label.place(x=0, y=0, relwidth=1, relheight=1)

        instructions = """
        Welcome to Halloween Heist!!
        Instructions:
        - Use the arrow keys to move the human character.
        - Avoid getting caught by the ghosts!
        - Collect points by moving over grids containing points.
        Good luck and have fun!
        """

        self.instructions_label = tk.Label(self.background_label, text=instructions, font=("Times", 18), wraplength=1000, justify=tk.LEFT, bg="black", fg="white")
        self.instructions_label.place(relx=0.18, rely=0.15, anchor=tk.CENTER)

        # Replace with actual button image path
        self.next_button_image = tk.PhotoImage(file=NEXT_BUTTON_IMAGE_PATH)
        self.next_button = tk.Button(self.background_label, image=self.next_button_image, bd=0, command=self.next_page)
        self.next_button.place(relx=0.5, rely=0.7, anchor=tk.CENTER)

        # Delay the reading of instructions by 100 milliseconds after the window is opened
        self.after(100, self.read_instructions)

    def next_page(self):
        self.destroy()
        instruction_page2 = InstructionPage2()
        instruction_page2.mainloop()

    def read_instructions(self):
        message = """
        Welcome to Halloween Heist!!

        Instructions:
        - Use the arrow keys to move the human character.
        - Avoid getting caught by the ghosts!
        - Collect points by moving over grids containing points.
        Good luck and have fun!
        """

        # Read instructions in a separate thread
        threading.Thread(target=self.speak_instructions, args=(message,)).start()

    def speak_instructions(self, message):
        engine = pyttsx3.init()
        engine.setProperty("rate", 230)  # Adjust the speaking speed (words per minute)
        engine.say(message)
        engine.runAndWait()

class InstructionPage2(tk.Tk):
    def __init__(self):
        super().__init__()

        self.title("Mummy Madness - Instructions - Page 2")
        self.geometry("1400x1400")

        # Add background image
        self.background_image = tk.PhotoImage(file=INSTRUCTIONS_BG_IMAGE_PATH)
        self.background_label = tk.Label(self, image=self.background_image)
        self.background_label.place(x=0, y=0, relwidth=1, relheight=1)

        self.num_mummies_label = tk.Label(self, text="Number of ghosts:", font=("Arial", 20))
        self.num_mummies_label.place(relx=0.8, rely=0.2, anchor=tk.CENTER)

        self.num_mummies_entry = tk.Entry(self, font=("Arial", 20), width=6)
        self.num_mummies_entry.place(relx=0.8, rely=0.3, anchor=tk.CENTER)

        self.mummy_positions_label = tk.Label(self, text="Enter Positions (x1,y1;x2,y2;x3,y3):", font=("Times", 25))
        self.mummy_positions_label.place(relx=0.8, rely=0.45, anchor=tk.CENTER)

        self.mummy_positions_entry = tk.Entry(self, font=("Times", 20), width=30)
        self.mummy_positions_entry.place(relx=0.8, rely=0.55, anchor=tk.CENTER)

        # Replace with actual button image path
        self.start_button_image = tk.PhotoImage(file=NEXT_BUTTON_IMAGE_PATH)
        self.start_button = tk.Button(self, image=self.start_button_image, bd=0, command=self.start_game)
        self.start_button.place(relx=0.8, rely=0.8, anchor=tk.CENTER)

        # Delay the reading of instructions by 100 milliseconds after the window is opened
        self.after(100, self.read_instructions)

    def start_game(self):
        num_mummies = int(self.num_mummies_entry.get())
        mummy_positions_str = self.mummy_positions_entry.get()
        mummy_positions = []

        try:
            for mummy_pos_str in mummy_positions_str.split(';'):
                x, y = map(int, mummy_pos_str.split(','))
                mummy_positions.append((x, y))

            if len(mummy_positions) == num_mummies:
                self.destroy()
                gui = MummyChaseGUI(mummy_positions)
                gui.mainloop()
            else:
                self.show_error_message("Invalid number of mummy positions!")
        except ValueError:
            self.show_error_message("Invalid input format for mummy positions!")

    def show_error_message(self, message):
        CustomMessageBox(message)

    def read_instructions(self):
        message = """
        Enter the number of ghosts and their positions in the following format:
        """

        engine = pyttsx3.init()
        engine.setProperty("rate", 205)  # Adjust the speaking speed (words per minute)
        engine.say(message)
        engine.runAndWait()

class TitlePage(tk.Tk):
    def __init__(self):
        super().__init__()

        self.title("Mummy Madness - Title")
        self.geometry("1400x1400")

        # Replace with actual image path
        self.background_image = tk.PhotoImage(file=TITLE_IMAGE_PATH)
        self.background_label = tk.Label(self, image=self.background_image)
        self.background_label.pack(fill=tk.BOTH, expand=True)

        # Replace with actual button image path
        self.start_button_image = tk.PhotoImage(file=START_BUTTON_IMAGE_PATH)
        self.start_button = tk.Button(self, image=self.start_button_image, bd=0, command=self.start_instructions)
        self.start_button.place(relx=0.48, rely=0.85, anchor=tk.CENTER)

    def start_instructions(self):
        self.destroy()
        instruction_page1 = InstructionPage1()
        instruction_page1.mainloop()

if __name__ == "__main__":
    title_page = TitlePage()
    title_page.mainloop()
