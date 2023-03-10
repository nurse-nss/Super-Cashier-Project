# Super-Cashier-Project
Python Super Cashier Project

# Latar Belakang
Self-service cashier merupakan program kasir untuk memudahkan pelanggan dalam melakukan transaksi. Pelanggan dapat memasukkan nama brang, jumlah barang dan harga barang yang dibeli secara mandiri.

## Requirements

Sebuah program kasir dengan alur berikut ini:

 1. Pelanggan memasukkan nama barang, jumlah barang, dan harga barang
 2. Jika pelanggan melakukan kesalahan input atau mau mengubah pesanan dapat dilakukan dengan:
    update nama, jumlah, ataupun harga barang yang sudah dimasukkan
 3. Jika pelanggan ingin membatalkan salah satu barang maka pelanggan dapat menghapus data yang sudah dimasukkan
 4. Pelanggan dapat mereset daftar data transaksi yang dimasukkan
 5. Pelanggan dapat melakukan validasi barang yang sudah di masukkan ke dalam transaksi
 6. Pelanggan dapat menghitung total harga dari transaksi yang sudah dimasukkan
 11. Pelanggan akan mendapatkan diskon jika ketentuan berikut:
    - 5% untuk total transaksi diatas Rp. 200.000
    - 8% untuk total transaksi diatas Rp. 300.000
    - 10% untuk total transaksi diatas Rp. 500.000


## Penjelasan Fungsi Python

Berikut penjelasan mengenai setiap method yang ada di dalam class `Transaction`.
 - **Constructor function**

```python
   class Transaction():

      def __init__(self)-> None:
          #inisiasi untuk class Transaction
          #trans(dict) = dictionary untuk menyimpan data transaksi (dict)
          #trans_valid(boolean) = untuk mem-validasi transaksi 
          self.trans = dict()
          self.trans_valid = False
```

 - Fungsi untuk **Menambahkan Barang**

```python
    def add_item(self, nama: str, jumlah: int, harga: float) -> None:
          '''fungsi untuk menambahkan item ke dalam dictionary transaksi
          nama (str) = nama barang yang dibeli
          jumlah (int) = jumlah barang yang dibeli
          harga (float) = harga barang'''

         #Uji validitas input
          error_message = []
          if type(nama)!=str:
              error_message.append("Jangan Masukkan Angka")
          if type(jumlah)!=int:
              error_message.append("Masukkan Angka!")
          if type(harga)!= float and type(harga) != int:
              error_message.append ("Masukkan Angka!")
    
          #Jika terjadi kesalahan akan ditampilkan pesan error
          if len(error_message) > 0:
              print("\n".join(error_message))
              return
    
          transaksi = ({nama: [nama, jumlah, harga, jumlah*harga]})
          self.trans.update(transaksi)
```
- Fungsi untuk **mengupdate nama** barang di dalam transaksi

```python
    def update_name(self, nama, update_nama)-> None:
          '''Fungsi untuk merubah nama barang dalam dictionary.
          nama(str)= nama barang sebelum diganti
          update_nama (str) = nama barang yang telah dirubah'''

          temp = self.trans[nama]
          self.trans.pop(nama)
          self.trans.update({update_name: temp})

          #Data yang ditampilkan setelah di update
          self.print_transaksi()
          return(f'Merubah nama barang {nama} menjadi {update_nama}')
```

 - Fungsi untuk **mengupdate jumlah** barang di dalam transaksi

```python
    def update_quantity(self, nama, update_jumlah):
          '''Fungsi untuk merubah jumlah barang dalam dictionary yang sudah diinput.
          nama(string) = nama barang yang akan diubah jumlahnya
          update_jumlah(int) = jumlah barang yang telah diubah''' 
    
          #validasi tipe data
          if type (update_jumlah)!=int:
            return("Masukkan Angka!")
          else:
              #Input data ke dalam dictionary
              self.transaksi[nama][0] = update_jumlah
              self.transaksi[nama][2] = update_jumlah*self.transaksi[nama][1]
              #data yang telah diubah
              self.print_transaksi()
              return(f"Jumlah Barang {nama} dirubah menjadi {update_jumlah}")
```

 - Fungsi untuk **mengupdate harga** barang di dalam transaksi

```python
    def update_price(self, nama, update_harga):
          '''fungsi untuk merubah harga barang dalam dictionary yang telah diinput.
          nama (string) = nama item yang aan diubah harganya
          update_harga (int) = harga yang telah diubah'''
          #validasi tipe data
          if type (update_harga)!=int:
            return("Masukkan Angka!")
          else:
              #Input data ke dalam dictionary
              self.transaksi[nama][1] = update_harga
              self.transaksi[nama][2] = update_harga*self.transaksi[nama][0]
              #data yang telah diubah
              self.print_transaksi()
              return(f"Harga Barang {nama} dirubah menjadi {update_harga}")
              
```

 - Fungsi untuk **menghapus barang**

```python
     def delete_item(self, nama: str)-> None:
          '''fungsi untuk menghapus data barang dari dictionary
           nama(str) = nama barang yang akan dihapus'''

          #cek nama barang
          error_message = []
          if type(nama) != str:
            error_message.append("Jangan Masukkan Angka")
          #jika terjadi kesalahan 
          if len(error_message) > 0:
              print("\n".join(error_message))
              return

          #cek daftar barang yang ingin dihapus dalam transaksi
          if nama not in self.trans:
            return("Barang tidak terdapat dalam daftar transaksi")
          #Barang yang ingin dihapus dari daftar transaksi
          self.trans.pop(nama)

          #tampilkan pesan
          return(f"Barang {nama} telah terhapus")
```

 - Fungsi untuk **menghapus seluruh barang** di dalam transaksi

```python
     def reset_transaksi(self)-> None:
          '''fungsi untuk menghapus data dalam dictionary'''

          self.trans.clear()
          return("Semua Pesanan Telah Terhapus")
```

 - Fungsi untuk **menampilkan daftar barang** di dalam transaksi

```python
     def print_transaksi(self) -> None:
          '''Fungsi untuk menampilkan seluruh data yang ada dalam dictionary'''

          #Validasi transaksi
          if len(self.trans) <=0:
            return("Anda belum memilih barang")

          #merubah data dictionary transaksi menjadi data frame
          trans_tabel = pd.DataFrame(self.trans).T

          #transaksi ditampilkan dalam bentuk tabel
          headers = ["Nama Barang", "Jumlah Barang", "Harga", "Total Harga"]
          print(tabulate(trans_tabel, headers, tablefmt="github", showindex=False))
```

 - Fungsi untuk **mengecek validitas** seluruh barang di dalam transaksi

```python
def cek_transaksi(self) -> bool:
          '''Fungsi untuk memvaliditas dan memperlihatkan pesan kesalahan jika terdapat data yang tidak valid.'''

          #validasi transaksi
          if len(self.trans) <= 0:
            return("Anda belum memilih barang")
                  
          # menampilkan pesan error jika terdapat data item yang tidak valid
          if len(self.trans) > 0:
            self.trans_valid = False
            print("\n".join(self.trans))
          else:
              self.trans_valid = True

              return self.trans_valid
```

 - Fungsi untuk **mengecek total transaksi dan discount**

```python
     def total_price(self):
          '''Fungsi untuk menampilkan seluruh pesanan yang ada dalam transaksi'''

          #pastikan pesanan valid
          self.print_transaksi()

          #perhitungan jika mendapatkan diskon
          #hitung total belanja
          total_belanja = 0
          for item in self.trans:
            total_belanja += self.trans[item][2]
          
          #perhitungan diskon
          if total_belanja > 500_000:
            diskon = int(total_belanja*0.1)
            total_belanja=int(total_belanja-diskon)
            return(f"Selamat anda mendapat diskon 10% sebesar {diskon}. Total Belanja anda sebesar Rp{total_belanja}")

          elif total_belanja > 300_000:
             diskon = int(total_belanja*0.08)
             total_belanja=int(total_belanja-diskon)
             return(f"Selamat anda mendapat diskon 8% sebesar {diskon}. Total Belanja anda sebesar Rp{total_belanja}")
              
          elif total_belanja > 200_000:
             diskon = int(total_belanja*0.05)
             total_belanja=int(total_belanja-diskon)
             return(f"Selamat anda mendapat diskon 5% sebesar {diskon}. Total Belanja anda sebesar Rp{total_belanja}")

          else:
             return(f"Total belanja Anda sebesar Rp{total_belanja}")
```
## Test Case

Berikut adalah hasil test yang telah dilakukan sesuai dengan test case yang diberikan

### Test Case 1: Menambahkan Barang

Menambahkan dua item baru menggunakan fungsi `add_item()`. Item yang ditambahkan sebagai berikut:
- Nama item: Ayam Goreng, Qty: 2, Harga: 20000
- Nama item: Pasta Gigi, Qty: 3, Harga: 15000

```python
tran=Transaction()

tran.add_item("Ayam goreng", 2, 20_000)
tran.add_item("Pasta Gigi", 3, 15_000)

tran.print_transaksi()

| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Ayam goreng   |               2 |   20000 |         40000 |
| Pasta Gigi    |               3 |   15000 |         45000 |
```
### Test Case 2: Menghapus Barang

Menghapus barang menggunakan fungsi `delete_item()`. Barang yang dihapus adalah Pasta Gigi.
```python
tran.print_transaksi()

| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Ayam goreng   |               2 |   20000 |         40000 |
| Pasta Gigi    |               3 |   15000 |         45000 |

tran.delete_item("Pasta Gigi")
print()
tran.print_transaksi()

| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Ayam goreng   |               2 |   20000 |         40000 |
```
### Test Case 3: Reset Transaksi

Mereset transaksi menggunakan fungsi `reset_transaction()`.
```python
tran.print_transaksi()

| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Ayam goreng   |               2 |   20000 |         40000 |

tran.reset_transaksi()
'Semua Pesanan Telah Terhapus'
```
### test Case 4: Menghitung Total Belanja

Menghitung total belanja menggunakan fungsi `total_price()`. Sebelum mengeluarkan output total belanja, akan menampilkan barang yang dibeli.
```python
tran.add_item("Ayam goreng", 2, 20_000)
tran.add_item("Pasta Gigi", 3, 15_000)

tran.total_price()
| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Ayam goreng   |               2 |   20000 |         40000 |
| Pasta Gigi    |               3 |   15000 |         45000 |
Total belanja Anda sebesar Rp35000
```
### Test Update Nama Barang

Mengubah nama item yang sudah diinput ke dalam transaksi menggunakan fungsi `update_name()`.
```python
tran = Transaction()

tran.add_item("Gula Pasir", 2, 20_000)
tran.add_item("Susu", 3, 15_000)

tran.print_transaksi()
| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Gula Pasir    |               2 |   20000 |         40000 |
| Susu          |               3 |   15000 |         45000 |

tran.update_name("Gula Pasir","Gula Merah")
| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Gula Merah    |               2 |   20000 |         40000 |
| Susu          |               3 |   15000 |         45000 |
Merubah nama barang Gula Pasir menjadi Gula Merah
```

## Test Update Jumlah Barang

Mengubah jumlah item yang sudah diinput ke dalam transaksi menggunakan method `update_qty()`.
```python
tran.print_transaksi()
| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Gula Merah    |               2 |   20000 |         40000 |
| Susu          |               2 |   10000 |         20000 |

tran.update_quantity("Susu", 2)
| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Gula Merah    |               2 |   20000 |         40000 |
| Susu          |               2 |   10000 |         20000 |
Jumlah Barang Susu dirubah menjadi 2
```
## Test Update Harga barang
```python
tran.print_transaksi()
| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Gula Merah    |               2 |   20000 |         40000 |
| Susu          |               2 |   10000 |         20000 |

tran.update_price("Susu",10_000)
| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Gula Merah    |               2 |   20000 |         40000 |
| Susu          |               2 |   10000 |         20000 |
Harga Barang Susu dirubah menjadi 10000
```

## Menghitung Total Belanja dengan Diskon

Menghitung total belanja menggunakan fungsi total_price(). Sebelum mengeluarkan output total belanja, akan menampilkan barang yang dibeli. Sistem akan menampilkan diskon dengan ketentuan
 - 5% untuk total transaksi diatas Rp. 200.000
 - 8% untuk total transaksi diatas Rp. 300.000
 - 10% untuk total transaksi diatas Rp. 500.000
```python
tran.print_transaksi()
| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Gula Merah    |               2 |   20000 |         40000 |
| Susu          |               2 |   10000 |         20000 |

tran.add_item("Butter", 5, 50_000)
tran.total_price()
| Nama Barang   |   Jumlah Barang |   Harga |   Total Harga |
|---------------|-----------------|---------|---------------|
| Gula Merah    |               2 |   20000 |         40000 |
| Susu          |               2 |   10000 |         20000 |
| Butter        |               5 |   50000 |        250000 |
Selamat anda mendapat diskon 8% sebesar 24800. Total Belanja anda sebesar Rp285200
```
## Conclusion
Beberapa test case yang dilakukan dalam pengembangan sistem Super Cashier telah berhasil dilakukan dengan hasil yang sesuai.
