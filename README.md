## Online marketplace data cleaning

Salah satu tahap dari teknik pengelola data dalam bidang data scientists adalah melakukan persiapan data dengan cara melakukan data cleaning dan data wrangling, data cleaning ada pemrosesan data oleh seorang data scientists dengan cara melakukan normalisasi raw data dari sumber data seperti database, data hasil scraping dan lain-lain sehingga menghindari terjadinya kesalahan analisis selanjutnya. Proses yang dilakukan saat dilakukan data cleaning antara lain memfilter data null atau kosong, menghapus outlier, mengelola data yang inkonsisten dan menghapus data duplicate dari data yang akan di analisis.
Untuk proses data wrangling sendiri adalah proses mengatur ulang data menjadi bentuk yang lebih terstruktur sehingga sesuai dengan permasalahan yang akan di eksplorasi oleh seorang data scientists, proses ini mencakup melakukan agregasi data, manipulasi data dan melakukan transformasi waktu. 

### Dataset:
1.	Olist customers
-	customer_id :  nilai unik yang dipakai dalam dataset untuk menghubungkan ke dataset yang lain.
-	Customer_uniq_id: nilai untuk mengidentifikasi pelanggan.
-	Customer_zip_code_prefix: 5 digit pertama dari zip code customer
-	Customer_city: nama kota kostumer
-	Customer_state : nama negara bagian kostumer
2.	Olist geolocation
-	Geolocation_zip_code_prefix: 5 digit pertama zip code
-	Geolocation_lat: nilai latitude 
-	Geolocation_lng:  nilai longtitude
-	Geolocation_city :  nama kota
-	Geolocation _state:  negara bagian
3.	Olist order items:
-	Order_id;  nomer unik pesanan 
-	Order_item_id: nomer yang berujuk pada item yang sama dalam satu pesanan
-	Product_id: nomer unik produk
-	Seller_id:  nomer unik penjual
-	Shipping_limit_data:  tanggal pengiriman terakhir untuk penjual
-	Price: harga barang
-	Freight_value:  biaya pengiriman barang
4.	Olist order payments
-	Order_id:  nomer unik pesanan 
-	Payement_sequential:  nilai yang menyatakan berapa kali pembayaran yang dilakukan oleh customer
-	Payment_type: metode pembayaran yang dipilih oleh customer
-	Payment_installments:  banyaknya angsuran yang dilakuikan oleh customer
-	Payment_value: nilai transaksi
-	Olist order review
-	Review_id: nomer unik review
-	Order_id:  nomer unik pesanan 
-	Review_score: nilai yang diberikan oleh customer dalam rentan 1 sampai 5
-	Review_comment_message: komentar yang diberikan oleh kostumer dalam bahasa portugis
-	Review_creation_date: menunjukan tanggal dikirimnya survey pada customer
-	Review_answer_timestamp: menunjukan tanggal setalah customer mengisi survey yang diberikan
6.	Olist order
-	Order_id:  nomer unik pesanan 
-	customer_id :  nilai unik yang dipakai dalam dataset untuk menghubungkan ke dataset yang lain.
-	Order_status: acuan status pesanan (pengiriman, diantar dan lain-lain)
-	Order_purchase_timestamp:  menunjukan waktu pembelian
-	Order_approved_at: menunjukan waktu pembayaran
-	Order_deliver_carrier_date: menunjukan tanggal dikirimnya pesanan oleh kurir
-	Order_deilver_customer_date: menunjukan tanggal dikirimnya pesanan ke kostumer
-	Order_estimated_delivery_date: menunjukan estimasi lama waktu pengiriman ke pada kostomer
7.	Olist product
-	Product_id: nomer unik produk
-	Product_category_name: ketogy produk dalam portugis
-	Product_name_lenght: panjang huruf nama produk
-	Product_description_lenght: panjang huruh deskripsi dari produk
-	Product_photos_qty: banyaknya photo produk
-	Product_weight _g: nilai berat produk dalam gram
-	Product_lenght_cm: nilai panjang produk dalam cm
-	Product_height_cm: nilai tinggi produk dalam cm
-	Product_width_cm: nilai lebar produk dalam cm
8.	Olist seller
-	Seller_id: nilai unik penjual
-	Seller_zip_code_prefix: 5 digit pertama dari zip code penjual
-	Seller_city: kota penjual
-	Seller_state: negara bagian penjual
9.	Product category name transalation
-	Product_category_name: nama kategori dalam portugis
-	Product_category_name_english: nama kategori dalam bahasa inggris

![RED](https://user-images.githubusercontent.com/115323333/205472050-14a98c5c-0a33-4446-92f3-943963a13d7a.png)


Data proses 
Dalam riset ini  proses data cleaning dan wrangling data dilakukan menggunakan python dibantu dengan library yang tersedia untuk python diantaranya:
-	Library pandas
-	Library numpy
-	Library Sqlite3
-	Library IPython
-	Library seaborn
-	Library dataFrame_image

Tahap pertama dalam penelitian ini adalah membuka data set yang akan dilakukan, dengan menggunakan library sqlite3 maka Python dapat membaca format .db dengan menggunakan query
          # create connection to database file
          database = "olist.db"
          connection = sql.connect(database)
          cur = connection.cursor()
          print all table in database
          table_list = [a for a in cur.execute("SELECT name FROM sqlite_master WHERE type = 'table'")] # fungsi ini akan membuat list table yang terdabat dalam file database   yang ada
          print(table_list)

setelah mendapat list table dari data set maka peneliti dapat menggunakan library pandas untuk membuat tabular data dari dataset tersebut:

          # make geolocation table
          df_geoloc = "SELECT * FROM olist_geolocation_dataset"
          geoloc_table = pd.read_sql_query(df_geoloc, connection)

setelah mendapatkan table pada data tersebut maka kita bisa melanjutkan dalam cleaning data dan wrangling data menggunakan python. Tahap selanjutnya adalah cleaning dan wrangling menggunakan objek tujuan yang sudah ditentukan.

### objek penelitian:
1.	Mencari sales summary per item
Objek penelitian pertama adalah mencari jumlah transaksi yang dilakukan per item dalam data yang ada

          #make order_items table
          df_order_items = "SELECT * FROM olist_order_items_dataset"
          order_items_table = pd.read_sql_query(df_order_items, connection)

          #make products table 
          df_products = "SELECT * FROM olist_products_dataset"
          products_table = pd.read_sql_query(df_products, connection)

          #make products eng name table 
          df_products_translate = "SELECT * FROM product_category_name_translation"
          products_eng_table = pd.read_sql_query(df_products_translate, connection)

          #Merge two table with product id as join
          sales_products = order_items_table.merge(products_table, on = "product_id")
          sales_products = sales_products.merge(products_eng_table, on = "product_category_name")
          
tabel 1. table pergabungan table olist_order_items_dataset, olist_products_dataset dan product_category_name_translation	
![image](https://user-images.githubusercontent.com/115323333/205472174-4311cb9b-0df0-445a-92ea-aa490b43deaf.png)
