import tkinter as tk
from tkinter import ttk, messagebox, filedialog
import csv

class Siswa:
    def __init__(self, id_siswa, nama, usia, kursus):
        self.id_siswa = id_siswa
        self.nama = nama
        self.usia = usia
        self.kursus = kursus

class ManajemenKursus:
    def __init__(self):
        self.siswa_list = []
        self.next_id = 1

    def tambah_siswa(self, siswa):
        siswa.id_siswa = self.next_id
        self.siswa_list.append(siswa)
        self.next_id += 1
        self.siswa_list = sorted(self.siswa_list, key=lambda x: x.nama)

    def lihat_siswa(self, sort_by='id'):
        if sort_by == 'id':
            return sorted(self.siswa_list, key=lambda x: x.id_siswa)
        elif sort_by == 'umur':
            return sorted(self.siswa_list, key=lambda x: x.usia)
        elif sort_by == 'kursus':
            return sorted(self.siswa_list, key=lambda x: x.kursus)
        else:
            messagebox.showwarning("Input Salah", "Kriteria sorting tidak valid.")
            return []

    def update_siswa(self, id_siswa, nama_baru, usia_baru, kursus_baru):
        index = self.cari_siswa(id_siswa)
        if index != -1:
            siswa = self.siswa_list[index]
            siswa.nama = nama_baru
            siswa.usia = usia_baru
            siswa.kursus = kursus_baru
            self.siswa_list[index] = siswa
            self.siswa_list = sorted(self.siswa_list, key=lambda x: x.nama)
        else:
            messagebox.showwarning("ID Tidak Ditemukan", f"Tidak dapat menemukan siswa dengan ID {id_siswa}")

    def hapus_siswa(self, id_siswa):
        index = self.cari_siswa(id_siswa)
        if index != -1:
            del self.siswa_list[index]
            self.perbarui_id_siswa()
        else:
            messagebox.showwarning("ID Tidak Ditemukan", f"Tidak dapat menemukan siswa dengan ID {id_siswa}")

    def perbarui_id_siswa(self):
        for i, siswa in enumerate(self.siswa_list):
            siswa.id_siswa = i + 1
        self.next_id = len(self.siswa_list) + 1

    def cari_siswa(self, id_siswa):
        for i, siswa in enumerate(self.siswa_list):
            if siswa.id_siswa == id_siswa:
                return i
        return -1

    def impor_data_csv(self, file_path):
        with open(file_path, mode='r') as file:
            csv_reader = csv.DictReader(file)
            for row in csv_reader:
                nama = row['Nama']
                usia = int(row['Usia'])
                kursus = row['Kursus'].split(", ")
                siswa = Siswa(None, nama, usia, kursus)
                self.tambah_siswa(siswa)

    def ekspor_data_csv(self, file_path):
        with open(file_path, mode='w', newline='') as file:
            fieldnames = ['ID', 'Nama', 'Usia', 'Kursus']
            writer = csv.DictWriter(file, fieldnames=fieldnames)
            writer.writeheader()
            for siswa in self.siswa_list:
                writer.writerow({
                    'ID': siswa.id_siswa,
                    'Nama': siswa.nama,
                    'Usia': siswa.usia,
                    'Kursus': ", ".join(siswa.kursus)
                })

class Aplikasi:
    def __init__(self, root):
        self.root = root
        self.root.title("Sistem Manajemen Kursus Online")
        self.manajemen_kursus = ManajemenKursus()

        self.setup_ui()

    def setup_ui(self):
        frame = ttk.Frame(self.root, padding="10")
        frame.grid(row=0, column=0, sticky=(tk.W, tk.E, tk.N, tk.S))

        ttk.Label(frame, text="Nama").grid(row=0, column=0, sticky=tk.W)
        self.nama_entry = ttk.Entry(frame, width=20)
        self.nama_entry.grid(row=0, column=1, sticky=tk.W)

        ttk.Label(frame, text="Usia").grid(row=1, column=0, sticky=tk.W)
        self.usia_entry = ttk.Entry(frame, width=20)
        self.usia_entry.grid(row=1, column=1, sticky=tk.W)

        ttk.Label(frame, text="Kursus").grid(row=2, column=0, sticky=tk.W)
        self.kursus_var = tk.StringVar(value=[])
        self.kursus_listbox = tk.Listbox(frame, listvariable=self.kursus_var, selectmode='multiple', height=4)
        kursus_options = ["Bahasa Indonesia", "Matematika", "Fisika", "Biologi"]
        for kursus in kursus_options:
            self.kursus_listbox.insert(tk.END, kursus)
        self.kursus_listbox.grid(row=2, column=1, sticky=tk.W)

        ttk.Button(frame, text="Tambah Siswa", command=self.tambah_siswa).grid(row=3, column=0, columnspan=2)
        ttk.Button(frame, text="Lihat Siswa Berdasarkan ID", command=lambda: self.lihat_siswa(sort_by='id')).grid(row=4, column=0, columnspan=2)
        ttk.Button(frame, text="Lihat Siswa Berdasarkan Umur", command=lambda: self.lihat_siswa(sort_by='umur')).grid(row=5, column=0, columnspan=2)
        ttk.Button(frame, text="Lihat Siswa Berdasarkan Kursus", command=lambda: self.lihat_siswa(sort_by='kursus')).grid(row=6, column=0, columnspan=2)
        ttk.Button(frame, text="Impor Data CSV", command=self.impor_data_csv).grid(row=7, column=0, columnspan=2)
        ttk.Button(frame, text="Ekspor Data CSV", command=self.ekspor_data_csv).grid(row=8, column=0, columnspan=2)

        self.siswa_listbox = tk.Listbox(frame, width=50, height=10)
        self.siswa_listbox.grid(row=9, column=0, columnspan=2)

        ttk.Label(frame, text="ID yang akan dihapus").grid(row=10, column=0, sticky=tk.W)
        self.id_hapus_entry = ttk.Entry(frame, width=10)
        self.id_hapus_entry.grid(row=10, column=1, sticky=tk.W)

        ttk.Button(frame, text="Hapus Siswa", command=self.hapus_siswa).grid(row=11, column=0, columnspan=2)

        ttk.Label(frame, text="ID yang akan diupdate").grid(row=12, column=0, sticky=tk.W)
        self.id_update_entry = ttk.Entry(frame, width=10)
        self.id_update_entry.grid(row=12, column=1, sticky=tk.W)

        ttk.Button(frame, text="Update Siswa", command=self.update_siswa).grid(row=13, column=0, columnspan=2)

    def tambah_siswa(self):
        nama = self.nama_entry.get()
        usia = self.usia_entry.get()
        kursus_indices = self.kursus_listbox.curselection()
        kursus = [self.kursus_listbox.get(i) for i in kursus_indices]
        if not nama or not usia or not kursus:
            messagebox.showwarning("Input Salah", "Semua kolom harus diisi.")
            return
        try:
            usia = int(usia)
        except ValueError:
            messagebox.showwarning("Input Salah", "Usia harus berupa angka.")
            return
        siswa = Siswa(None, nama, usia, kursus)
        self.manajemen_kursus.tambah_siswa(siswa)
        self.lihat_siswa()
        self.nama_entry.delete(0, tk.END)
        self.usia_entry.delete(0, tk.END)
        self.kursus_listbox.selection_clear(0, tk.END)

    def lihat_siswa(self, sort_by='id'):
        self.siswa_listbox.delete(0, tk.END)
        for siswa in self.manajemen_kursus.lihat_siswa(sort_by=sort_by):
            self.siswa_listbox.insert(tk.END, f"ID: {siswa.id_siswa} - {siswa.nama} - {siswa.usia} - {', '.join(siswa.kursus)}")

    def hapus_siswa(self):
        id_siswa = self.id_hapus_entry.get()
        if not id_siswa:
            messagebox.showwarning("Input Salah", "ID harus diisi.")
            return
        try:
            id_siswa = int(id_siswa)
        except ValueError:
            messagebox.showwarning("Input Salah", "ID harus berupa angka.")
            return
        self.manajemen_kursus.hapus_siswa(id_siswa)
        self.lihat_siswa()
        self.id_hapus_entry.delete(0, tk.END)

    def update_siswa(self):
        id_siswa = self.id_update_entry.get()
        nama = self.nama_entry.get()
        usia = self.usia_entry.get()
        kursus_indices = self.kursus_listbox.curselection()
        kursus = [self.kursus_listbox.get(i) for i in kursus_indices]
        if not id_siswa or not nama or not usia or not kursus:
            messagebox.showwarning("Input Salah", "Semua kolom harus diisi.")
            return
        try:
            id_siswa = int(id_siswa)
            usia = int(usia)
        except ValueError:
            messagebox.showwarning("Input Salah", "ID dan Usia harus berupa angka.")
            return
        self.manajemen_kursus.update_siswa(id_siswa, nama, usia, kursus)
        self.lihat_siswa()
        self.id_update_entry.delete(0, tk.END)
        self.nama_entry.delete(0, tk.END)
        self.usia_entry.delete(0, tk.END)
        self.kursus_listbox.selection_clear(0, tk.END)

    def impor_data_csv(self):
        file_path = filedialog.askopenfilename(filetypes=[("CSV files", "*.csv")])
        if file_path:
            self.manajemen_kursus.impor_data_csv(file_path)
            self.lihat_siswa()

    def ekspor_data_csv(self):
        file_path = filedialog.asksaveasfilename(defaultextension=".csv", filetypes=[("CSV files", "*.csv")])
        if file_path:
            self.manajemen_kursus.ekspor_data_csv(file_path)
            messagebox.showinfo("Ekspor Berhasil", f"Data berhasil diekspor ke {file_path}")

if __name__ == "__main__":
    root = tk.Tk()
    app = Aplikasi(root)
    root.mainloop()
