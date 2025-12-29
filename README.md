import tkinter as tk
from tkinter import messagebox
from datetime import datetime

# File to store keystrokes
LOG_FILE = "keystrokes_log.txt"

class KeyLoggerApp:
    def __init__(self, root):
        self.root = root
        self.root.title("Keystroke Logging Demo (Educational)")
        self.root.geometry("500x350")

        self.logging = False

        # Title label
        tk.Label(
            root,
            text="Keystroke Logging Demonstration",
            font=("Arial", 14, "bold")
        ).pack(pady=10)

        # Info label
        tk.Label(
            root,
            text="Keystrokes are logged ONLY inside this window.",
            fg="gray"
        ).pack()

        # Text box to type in
        self.text_area = tk.Text(root, height=8, width=50)
        self.text_area.pack(pady=10)
        self.text_area.bind("<Key>", self.capture_key)

        # Buttons
        btn_frame = tk.Frame(root)
        btn_frame.pack(pady=10)

        tk.Button(btn_frame, text="Start Logging", command=self.start_logging).grid(row=0, column=0, padx=10)
        tk.Button(btn_frame, text="Stop Logging", command=self.stop_logging).grid(row=0, column=1, padx=10)
        tk.Button(btn_frame, text="Exit", command=root.quit).grid(row=0, column=2, padx=10)

        # Status label
        self.status_label = tk.Label(root, text="Status: Not Logging", fg="red")
        self.status_label.pack(pady=5)

    def start_logging(self):
        self.logging = True
        self.status_label.config(text="Status: Logging Enabled", fg="green")

        with open(LOG_FILE, "a") as f:
            f.write("\n\n--- Logging Started: {} ---\n".format(datetime.now()))

    def stop_logging(self):
        self.logging = False
        self.status_label.config(text="Status: Logging Stopped", fg="red")

        with open(LOG_FILE, "a") as f:
            f.write("--- Logging Stopped: {} ---\n".format(datetime.now()))

    def capture_key(self, event):
        if not self.logging:
            return

        key = event.keysym

        # Handle special keys
        if key == "space":
            key = " "
        elif key == "Return":
            key = "\n"
        elif key == "BackSpace":
            key = "[BACKSPACE]"
        elif len(key) > 1:
            key = f"[{key}]"

        with open(LOG_FILE, "a") as f:
            f.write(key)

# Run the app
if __name__ == "__main__":
    root = tk.Tk()
    app = KeyLoggerApp(root)
    root.mainloop()
