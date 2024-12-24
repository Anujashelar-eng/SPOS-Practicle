import tkinter as tk
from tkinter import ttk

# Main Application Class
class ComplexGUIApp(tk.Tk):
    def __init__(self):
        super().__init__()
        self.title("Redesigned Complex GUI")
        self.geometry("800x600")
        container = tk.Frame(self)
        container.pack(fill="both", expand=True)
        self.frames = {}

        for F in (HomeScreen, SettingsScreen, DataScreen):
            frame = F(container, self)
            self.frames[F] = frame
            frame.grid(row=0, column=0, sticky="nsew")

        self.show_frame(HomeScreen)

    def show_frame(self, cont):
        self.frames[cont].tkraise()

# Home Screen Class
class HomeScreen(tk.Frame):
    def __init__(self, parent, controller):
        super().__init__(parent)
        tk.Label(self, text="Welcome to Home Screen", font=("Arial", 18)).pack(pady=10,padx=10)
        ttk.Button(self, text="Go to Settings", command=lambda: controller.show_frame(SettingsScreen)).pack()
        ttk.Button(self, text="Go to Data Screen", command=lambda: controller.show_frame(DataScreen)).pack()

# Settings Screen Class
class SettingsScreen(tk.Frame):
    def __init__(self, parent, controller):
        super().__init__(parent)
        tk.Label(self, text="Settings", font=("Arial", 18)).pack(pady=10)
        tk.Label(self, text="Select Theme:").pack(pady=5)
        theme_var = tk.StringVar(value="Light")
        ttk.OptionMenu(self, theme_var, "Light", "Dark").pack(pady=5)
        ttk.Button(self, text="Go to Home", command=lambda: controller.show_frame(HomeScreen)).pack(pady=20)

# Data Screen Class
class DataScreen(tk.Frame):
    def __init__(self, parent, controller):
        super().__init__(parent)
        tk.Label(self, text="Data Screen", font=("Arial", 18)).pack(pady=10)
        columns = ('ID', 'Name', 'Value')
        data_tree = ttk.Treeview(self, columns=columns, show='headings')
        for col in columns:
            data_tree.heading(col, text=col)
        sample_data = [(1, 'Temperature', '22C'), (2, 'Humidity', '60%'), (3, 'Pressure', '1012 hPa')]
        for data in sample_data:
            data_tree.insert('', tk.END, values=data)
        data_tree.pack(pady=20)
        ttk.Button(self, text="Go to Home", command=lambda: controller.show_frame(HomeScreen)).pack(pady=20)

# Run the application
if __name__ == "__main__":
    app = ComplexGUIApp()
    app.mainloop()
