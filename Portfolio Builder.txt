import tkinter as tk
from tkinter import messagebox, filedialog
from PIL import Image, ImageTk
from reportlab.pdfgen import canvas

# Create the main application window
app = tk.Tk()
app.title("Portfolio Creator")
app.geometry("500x600")
app.resizable(False, False)

# Apple-like fonts and colors
FONT_TITLE = ("Helvetica", 20, "bold")
FONT_LABEL = ("Helvetica", 12)
FONT_BUTTON = ("Helvetica", 12, "bold")
COLOR_BG = "#f2f2f2"

# Create a function to handle the form submission
def submit_form():
    # Get the user input
    name = entry_name.get()
    number = entry_number.get()
    email = entry_email.get()
    qualification = entry_qualification.get()
    skills = entry_skills.get("1.0", tk.END)

    # Validate the input
    if not name or not number or not email or not qualification or not skills:
        messagebox.showerror("Error", "Please fill in all the fields.")
        return

    # Open file dialog to select a photo
    filename = filedialog.askopenfilename(filetypes=[("Image files", "*.jpg *.jpeg *.png")])
    if not filename:
        messagebox.showerror("Error", "Please select a photo.")
        return

    # Create the portfolio PDF
    pdf_filename = "portfolio.pdf"
    generate_portfolio_pdf(pdf_filename, name, number, email, qualification, skills, filename)
    messagebox.showinfo("Success", f"Portfolio created successfully.\nPDF saved as {pdf_filename}")

# Function to generate the portfolio PDF
def generate_portfolio_pdf(filename, name, number, email, qualification, skills, photo_filename):
    c = canvas.Canvas(filename)

    # Set up the page
    c.setFont(FONT_TITLE[0], FONT_TITLE[1])
    c.drawCentredString(300, 700, "Portfolio")
    c.setFont(FONT_LABEL[0], FONT_LABEL[1])
    c.drawString(50, 650, f"Name: {name}")
    c.drawString(50, 620, f"Number: {number}")
    c.drawString(50, 590, f"Email: {email}")
    c.drawString(50, 560, f"Qualification: {qualification}")
    c.drawString(50, 530, f"Skills:")
    c.setFont(FONT_LABEL[0], FONT_LABEL[1] - 2)
    c.drawString(50, 500, skills)

    # Draw the photo
    photo = Image.open(photo_filename)
    photo.thumbnail((200, 200))
    photo.save("thumbnail.jpg")
    c.drawImage("thumbnail.jpg", 400, 500)

    c.showPage()
    c.save()

# Create the form labels
label_name = tk.Label(app, text="Name:", font=FONT_LABEL, bg=COLOR_BG)
label_name.pack(pady=(30, 5))
label_number = tk.Label(app, text="Number:", font=FONT_LABEL, bg=COLOR_BG)
label_number.pack()
label_email = tk.Label(app, text="Email:", font=FONT_LABEL, bg=COLOR_BG)
label_email.pack()
label_qualification = tk.Label(app, text="Qualification:", font=FONT_LABEL, bg=COLOR_BG)
label_qualification.pack()
label_skills = tk.Label(app, text="Skills:", font=FONT_LABEL, bg=COLOR_BG)
label_skills.pack()
label_photo = tk.Label(app, text="Upload Photo:", font=FONT_LABEL, bg=COLOR_BG)
label_photo.pack(pady=(30, 5))

# Create the form entry fields
entry_name = tk.Entry(app, font=FONT_LABEL)
entry_name.pack()
entry_number = tk.Entry(app, font=FONT_LABEL)
entry_number.pack()
entry_email = tk.Entry(app, font=FONT_LABEL)
entry_email.pack()
entry_qualification = tk.Entry(app, font=FONT_LABEL)
entry_qualification.pack()
entry_skills = tk.Text(app, width=40, height=5, font=FONT_LABEL)
entry_skills.pack()
button_upload = tk.Button(app, text="Browse...", font=FONT_BUTTON, command=submit_form)
button_upload.pack(pady=(10, 30))

# Start the application
app.mainloop()
