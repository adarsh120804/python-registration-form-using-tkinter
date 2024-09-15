import tkinter as tk
from tkinter import messagebox

# Function to handle form submission
def submit_form():
    first_name = entries["first_name"].get()
    last_name = entries["last_name"].get()
    age = entries["age"].get()
    gender = gender_var.get()
    phone = entries["phone"].get()
    email = entries["email"].get()
    password = entries["password"].get()
    confirm_password = entries["confirm_password"].get()

    # Validation
    if not first_name or not last_name or not age or not gender or not phone or not email or not password or not confirm_password:
        messagebox.showerror("Input Error", "All fields are required!")
        return
    if "@" not in email or "." not in email:
        messagebox.showerror("Input Error", "Please enter a valid email address!")
        return
    if not phone.isdigit() or len(phone) != 10:
        messagebox.showerror("Input Error", "Please enter a valid 10-digit phone number!")
        return
    if not age.isdigit() or int(age) <= 0:
        messagebox.showerror("Input Error", "Please enter a valid age!")
        return
    if password != confirm_password:
        messagebox.showerror("Input Error", "Passwords do not match!")
        return

    messagebox.showinfo("Registration Successful", "You have been registered successfully!")

# Function to minimize the window when ESC is pressed
def minimize_window(event):
    root.iconify()  # Minimizes the window

# Create the main window
root = tk.Tk()
root.title("Next24Technology Registration Form")

# Get screen dimensions
screen_width = root.winfo_screenwidth()
screen_height = root.winfo_screenheight()

# Set window size to full size of the screen
root.geometry(f"{screen_width}x{screen_height}")

# Define colors
header_color = "#8B8589"  # Taupe gray
footer_color = "#8B8589"  # Taupe gray
form_bg_color = "#F4F4F4"  # Light grey
button_color = "#0066cc"  # Medium blue
button_hover_color = "#004d99"  # Darker blue

# Create a frame for the header with taupe gray background
header_frame = tk.Frame(root, bg=header_color, pady=10)
header_frame.grid(row=0, column=0, sticky="ew")

title_label = tk.Label(header_frame, text="Next24Technology Registration Form", font=("Helvetica", 20, "bold"), fg="#000000", bg=header_color)
title_label.pack()

# Create a frame for the main content
main_frame = tk.Frame(root)
main_frame.grid(row=1, column=0, sticky="nsew")

# Create a frame for the form that fills the main content area
form_frame = tk.Frame(main_frame, bg=form_bg_color, padx=20, pady=20, relief="solid", bd=2)
form_frame.pack(padx=20, pady=20, fill="both", expand=True)

# Create a frame for all fields that fills the form_frame
fields_frame = tk.Frame(form_frame, bg=form_bg_color)
fields_frame.pack(padx=10, pady=10, fill="both", expand=True)

# Dictionary to store the entry fields
entries = {}

# Create gender variable
gender_var = tk.StringVar(value="")

# Add fields in the specified order
fields = [
    ("First Name:", "first_name"),
    ("Last Name:", "last_name"),
    ("Age:", "age"),
    ("Gender:", "gender"),
    ("Phone Number:", "phone"),
    ("Email Address:", "email"),
    ("Password:", "password"),
    ("Confirm Password:", "confirm_password")
]

# Add labels and entry fields to fields_frame
for index, (label_text, var_name) in enumerate(fields):
    tk.Label(fields_frame, text=label_text, bg=form_bg_color, font=("Helvetica", 12)).grid(row=index, column=0, sticky="w", pady=5, padx=5)
    if var_name == "gender":
        # Gender Radio Buttons
        gender_frame = tk.Frame(fields_frame, bg=form_bg_color)
        gender_frame.grid(row=index, column=1, pady=5, padx=5, sticky="ew")
        
        tk.Radiobutton(gender_frame, text="Male", variable=gender_var, value="Male", bg=form_bg_color, font=("Helvetica", 10)).pack(side="left", padx=2)
        tk.Radiobutton(gender_frame, text="Female", variable=gender_var, value="Female", bg=form_bg_color, font=("Helvetica", 10)).pack(side="left", padx=2)
        tk.Radiobutton(gender_frame, text="Other", variable=gender_var, value="Other", bg=form_bg_color, font=("Helvetica", 10)).pack(side="left", padx=2)
    else:
        entry = tk.Entry(fields_frame, font=("Helvetica", 12), width=50, bd=2, relief="solid")
        if var_name in ["password", "confirm_password"]:
            entry.config(show="*")
        entry.grid(row=index, column=1, pady=5, padx=5, sticky="ew")
        entries[var_name] = entry

# Password Guidelines
guidelines = [
    "• At least 8 characters long",
    "• Contains both uppercase and lowercase letters",
    "• Includes at least one number",
    "• Includes at least one special character (e.g., @, #, $)"
]

guidelines_label = tk.Label(form_frame, text="Password Guidelines:", bg=form_bg_color, font=("Helvetica", 12, "bold"))
guidelines_label.pack(anchor="w", padx=10, pady=5)

for guideline in guidelines:
    tk.Label(form_frame, text=guideline, bg=form_bg_color, font=("Helvetica", 10)).pack(anchor="w", padx=20, pady=2)

# Submit Button
submit_button = tk.Button(form_frame, text="Register", command=submit_form, font=("Helvetica", 14, "bold"), bg=button_color, fg="#ffffff", height=2, width=20)
submit_button.pack(pady=20)

# Button hover effects
def on_enter(event):
    submit_button.config(bg=button_hover_color)

def on_leave(event):
    submit_button.config(bg=button_color)

submit_button.bind("<Enter>", on_enter)
submit_button.bind("<Leave>", on_leave)

# Footer with taupe gray background
footer_frame = tk.Frame(root, bg="#555555" , pady=10)
footer_frame.grid(row=2, column=0, sticky="ew")

footer_label = tk.Label(footer_frame, text="© 2023 Next24Technology. All rights reserved.", font=("Helvetica", 10), fg="#FFFFFF", bg="#555555")
footer_label.pack()

# Configure row and column weights
root.grid_rowconfigure(1, weight=1)  # Make the main_frame expand
root.grid_rowconfigure(2, weight=0)  # Footer frame stays at the bottom
root.grid_columnconfigure(0, weight=1)  # Make the column expand

main_frame.grid_rowconfigure(0, weight=1)  # Make the form_frame expand
main_frame.grid_columnconfigure(0, weight=1)  # Make the column expand

# Bind the ESC key to minimize the window
root.bind("<Escape>", minimize_window)

# Start the GUI event loop
root.mainloop()
