
    pip install pygame  
           
           import os
           import pygame
           import tkinter as tk
           from tkinter import filedialog, messagebox
           
           class MusicPlayer:
               def __init__(self, root):
                   self.root = root
                   self.root.title("Music Player")
                   self.root.geometry("350x260")
           
                   pygame.mixer.init()
           
                   self.is_paused = False
                   self.music_files = []
                   self.current_index = 0
           
                   # Create buttons
                   self.btn_select_folder = tk.Button(root, text="Select Folder", command=self.select_folder)
                   self.btn_play = tk.Button(root, text="Play", command=self.play_music)
                   self.btn_pause = tk.Button(root, text="Pause", command=self.pause_music)
                   self.btn_stop = tk.Button(root, text="Stop", command=self.stop_music)
           
                   # Place buttons on the window
                   self.btn_select_folder.grid(row=0, column=10,pady=10)
                   self.btn_play.grid(row=1, column=8, pady=10)
                   self.btn_pause.grid(row=1, column=10, pady=10)
                   self.btn_stop.grid(row=1, column=12, pady=10)
           
               def select_folder(self):
                   folder_selected = filedialog.askdirectory()
                   if folder_selected:
                       self.music_files = [os.path.join(folder_selected, f) for f in os.listdir(folder_selected) if f.endswith('.mp3')]
                       if self.music_files:
                           messagebox.showinfo("Folder Selected", f"Found {len(self.music_files)} music files")
                           self.current_index = 0
                       else:
                           messagebox.showwarning("No Music Files", "No MP3 files found in the selected folder")
           
               def play_music(self):
                   if self.music_files:
                       if self.is_paused:
                           pygame.mixer.music.unpause()
                           self.is_paused = False
                       else:
                           pygame.mixer.music.load(self.music_files[self.current_index])
                           pygame.mixer.music.play()
                           pygame.mixer.music.set_endevent(pygame.USEREVENT)
                           self.root.bind("<space>", self.next_song)
                   else:
                       messagebox.showwarning("No Music Files", "Please select a folder first")
           
               def pause_music(self):
                   if pygame.mixer.music.get_busy():
                       pygame.mixer.music.pause()
                       self.is_paused = True
           
               def stop_music(self):
                   pygame.mixer.music.stop()
                   self.is_paused = False
           
               def next_song(self, event):
                   self.current_index += 1
                   if self.current_index >= len(self.music_files):
                       self.current_index = 0
                   self.play_music()
           
           if __name__ == "__main__":
               root = tk.Tk()
               app = MusicPlayer(root)
               root.mainloop()
