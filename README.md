import tkinter as tk
from tkinter import filedialog
import cv2
from PIL import Image, ImageTk

def open_video():
    file_path = filedialog.askopenfilename(
        title="Select Video",
        filetypes=(("MP4 files", "*.mp4"), ("All files", "*.*"))
    )
    if file_path:
        play_video(file_path)

def play_video(path):
    cap = cv2.VideoCapture(path)

    def update_frame():
        ret, frame = cap.read()
        if ret:
            frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
            img = ImageTk.PhotoImage(Image.fromarray(frame))
            lbl.config(image=img)
            lbl.image = img
            lbl.after(20, update_frame)  # ~50 fps
        else:
            cap.release()

    update_frame()

root = tk.Tk()
root.title("Simple Video Player")

btn = tk.Button(root, text="Open Video", command=open_video)
btn.pack()

lbl = tk.Label(root)
lbl.pack()

root.mainloop()
