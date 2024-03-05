# tkinter kütüphanesinden gerekli modülleri ekliyoruz
from tkinter import *
from tkinter.font import Font
from tkinter import filedialog, messagebox
import pickle


# Listbox'tan seçilen öğeleri takip etmek için kullanılan fonksiyon
def on_select(event):
    selected_index = my_list.curselection()
    if selected_index:
        print(f"Selected Index: {selected_index[0]}")
        selected_task = my_list.get(selected_index)
        print(f"Selected Task: {selected_task}")


# Listbox'ta seçili öğeyi silen fonksiyon
def delete_item():
    my_list.delete(ANCHOR)


# Seçili öğeleri gri renge döndüren fonksiyon (bitmiş görevler için)
def cross_off_item():
    my_list.itemconfig(
        my_list.curselection(),
        fg="#dedede"
    )
    my_list.selection_clear(0, END)


# Gri renkteki öğeleri tekrar eski renklerine döndüren fonksiyon
# Kullanıcı yanlışlıkla bitmemiş görevi bitti olarak işaretlerse diye
def uncross_item():
    my_list.itemconfig(
        my_list.curselection(),
        fg="#30001a"
    )
    my_list.selection_clear(0, END)


# Yapılmış görevleri silen fonksiyon
def delete_crossed_items():
    reversed_indices = reversed(range(my_list.size()))
    for index in reversed_indices:
        if my_list.itemcget(index, "fg") == "#dedede":
            my_list.delete(index)



# Listbox'a yeni görev ekleyen fonksiyon
def add_item():
    new_task = my_entry.get().strip()  # Girişten boşlukları temizleyerek al
    if new_task:  # Eğer görev boş değilse
        my_list.insert(END, new_task)
        my_entry.delete(0, END)
    else:
        # Boş giriş eklenemez uyarısı
        messagebox.showwarning("Uyarı", "Boş görev eklenemez!")


# Listeyi belirtilen dosyaya kaydeden fonksiyon
def save_list():
    file_name = filedialog.asksaveasfilename(
        initialdir="C:python_odev\170423822_1_ipekakpinar\data",
        title="Dosyayı Kaydet",
        filetypes=(
            ("JSON Dosyaları", "*.json"),
            ("Tüm Dosyalar", "*.*")
        )
    )

    if file_name:
        try:
            if not file_name.endswith(".json"):
                file_name = f'{file_name}.json'

            count = 0

            while count < my_list.size():
                if my_list.itemcget(count, "fg") == "#dedede":
                    my_list.delete(my_list.index(count))
                else:
                    count += 1

            task = my_list.get(0, END)

            with open(file_name, 'wb') as output_file:
                pickle.dump(task, output_file)

        except Exception as e:
            # Veri kaydetme hatası uyarısı
            messagebox.showerror("Hata", f"Veri kaydedilirken bir hata oluştu:\n{str(e)}")

# Kaydedilmiş listeyi açan fonksiyon
def open_list():
    file_name = filedialog.askopenfilename(
        initialdir="C:\python_odev\170423822_1_ipekakpinar\data",
        title="Open File",
        filetypes=(
            ("JSON Files", "*.json"),
            ("All Files", "*.*")
        )
    )

    if file_name:
        my_list.delete(0, END)
        input_file = open(file_name, 'rb')
        tasks = pickle.load(input_file)

        for item in tasks:
            my_list.insert(END, item)


# Listeyi temizleyen fonksiyon
def clear_list():
    my_list.delete(0,END)


# Ana Tkinter penceresi
root = Tk()
root.title('TO-DO LIST')
root.geometry("500x650")
root.configure(bg="#FFE5EC")

# Özel font ayarları
my_font = Font(
    family="Sagoe UI",
    size=15,
    slant="italic"
)


# Arayüzdeki başlıklar
Label(root, text="TO-DO LIST", font=("Cooper Siyah", 25, "bold"), bg="#FB6F92", fg="#FFE5EC", bd=2,
      relief="groove").place(x=10, y=10)
Label(root, text="My Tasks:", font=my_font, bg="#FF8FAB", fg="#FFE5EC", bd=2, relief="groove").place(x=10, y=60)


# Frame oluşturma
my_frame = Frame(root)
my_frame.place(x=10, y=100)  # Adjust coordinates

# Listbox oluşturma
my_list = Listbox(my_frame,
                  font=my_font,
                  width=42,
                  height=10,
                  fg="#30001a",
                  bd=2,
                  relief="groove",
                  highlightthickness=0,
                  selectbackground="#FF8FAB",
                  activestyle="none")

# Scrollbar oluşturma
my_scrollbar = Scrollbar(my_frame, orient=VERTICAL, command=my_list.yview)
my_scrollbar.pack(side=RIGHT, fill=Y)
my_list.config(yscrollcommand=my_scrollbar.set)

my_list.pack(side=LEFT, fill=BOTH)  #listeyi sola yerleştirme

my_list.bind("<<ListboxSelect>>", on_select)


# Butonlar ve frame'ler ekleniyor
button_frame1 = Frame(root, bg="#FFE5EC")
button_frame1.place(x=10, y=360)

button_frame2 = Frame(root, bg="#FFE5EC")
button_frame2.place(x=10, y=400)

button_frame3 = Frame(root, bg="#FFE5EC")
button_frame3.place(x=10, y=570)


# Menü oluşturuluyor
my_menu = Menu(root)
root.config(menu=my_menu)

# Dosya menüsü oluşturuluyor
file_menu = Menu(my_menu, tearoff=False)
my_menu.add_cascade(label="File", menu=file_menu)

# Dosya menüsüne seçenekler ekleniyor
file_menu.add_command(label="Save List", command=save_list)
file_menu.add_command(label="Open List", command=open_list)
file_menu.add_separator()
file_menu.add_command(label="Clear List", command=clear_list)


# Butonlar oluşturuluyor
delete_button = Button(button_frame1, text="Delete Task", bg="#FF8FAB", bd=2, relief="groove", command=delete_item)
cross_off_button = Button(button_frame1, text="Done", bg="#FF8FAB", bd=2, relief="groove", command=cross_off_item)
uncross_button = Button(button_frame1, text="Not Done Yet", bg="#FF8FAB", bd=2, relief="groove", command=uncross_item)
delete_crossed_button = Button(button_frame2, text="Clear Finished Tasks", bg="#FF8FAB", bd=2, relief="groove", command=delete_crossed_items)
add_button = Button(button_frame3, text="Add Task", bg="#FF8FAB", bd=2, relief="groove", command=add_item)

space_label = Label(button_frame1,
                    text="                                                                                    ",
                    bg="#FFE5EC")
delete_button.pack(side=LEFT)
space_label.pack(side=LEFT)
uncross_button.pack(side=RIGHT, padx=20)
cross_off_button.pack(side=RIGHT)
delete_crossed_button.pack(side=LEFT, ipadx=175)
add_button.pack(side=LEFT)


# "Add Tasks" etiketi ekleniyor
Label(root, text="Add Tasks:", font=my_font, bg="#FF8FAB", fg="#FFE5EC", bd=2, relief="groove").place(x=10, y=480)


# Entry kutusu ekleniyor
my_entry = Entry(root, font=(my_font, 26), width=25)
my_entry.place(x=10, y=520)


# Tkinter ana döngüsü başlatılıyor
root.mainloop()

