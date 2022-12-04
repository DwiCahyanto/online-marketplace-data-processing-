## Online marketplace data cleaning

Salah satu tahap dari teknik pengelola data dalam bidang _data scientists_ adalah melakukan persiapan data dengan cara melakukan _data cleaning dan data wrangling_, data cleaning ada pemrosesan data oleh seorang _data scientists_ dengan cara melakukan normalisasi raw data dari sumber data seperti database, data hasil scraping dan lain-lain sehingga menghindari terjadinya kesalahan analisis selanjutnya. Proses yang dilakukan saat dilakukan data cleaning antara lain memfilter data null atau kosong, menghapus outlier, mengelola data yang inkonsisten dan menghapus data duplicate dari data yang akan di analisis.
Untuk proses _data wrangling_ sendiri adalah proses mengatur ulang data menjadi bentuk yang lebih terstruktur sehingga sesuai dengan permasalahan yang akan di eksplorasi oleh seorang _data scientists_, proses ini mencakup melakukan agregasi data, manipulasi data dan melakukan transformasi waktu. 

### Dataset:
**1.	Olist customers**
-	customer_id :  nilai unik yang dipakai dalam dataset untuk menghubungkan ke dataset yang lain.
-	Customer_uniq_id: nilai untuk mengidentifikasi pelanggan.
-	Customer_zip_code_prefix: 5 digit pertama dari zip code customer
-	Customer_city: nama kota kostumer
-	Customer_state : nama negara bagian kostumer

**2.	Olist geolocation**
-	Geolocation_zip_code_prefix: 5 digit pertama zip code
-	Geolocation_lat: nilai latitude 
-	Geolocation_lng:  nilai longtitude
-	Geolocation_city :  nama kota
-	Geolocation _state:  negara bagian

**3.	Olist order items:**
-	Order_id;  nomer unik pesanan 
-	Order_item_id: nomer yang berujuk pada item yang sama dalam satu pesanan
-	Product_id: nomer unik produk
-	Seller_id:  nomer unik penjual
-	Shipping_limit_data:  tanggal pengiriman terakhir untuk penjual
-	Price: harga barang
-	Freight_value:  biaya pengiriman barang

**4.	Olist order payments**
-	Order_id:  nomer unik pesanan 
-	Payement_sequential:  nilai yang menyatakan berapa kali pembayaran yang dilakukan oleh customer
-	Payment_type: metode pembayaran yang dipilih oleh customer
-	Payment_installments:  banyaknya angsuran yang dilakuikan oleh customer
-	Payment_value: nilai transaksi

**5.	Olist order review**
-	Review_id: nomer unik review
-	Order_id:  nomer unik pesanan 
-	Review_score: nilai yang diberikan oleh customer dalam rentan 1 sampai 5
-	Review_comment_message: komentar yang diberikan oleh kostumer dalam bahasa portugis
-	Review_creation_date: menunjukan tanggal dikirimnya survey pada customer
-	Review_answer_timestamp: menunjukan tanggal setalah customer mengisi survey yang diberikan

**6.	Olist order**
-	Order_id:  nomer unik pesanan 
-	customer_id :  nilai unik yang dipakai dalam dataset untuk menghubungkan ke dataset yang lain.
-	Order_status: acuan status pesanan (pengiriman, diantar dan lain-lain)
-	Order_purchase_timestamp:  menunjukan waktu pembelian
-	Order_approved_at: menunjukan waktu pembayaran
-	Order_deliver_carrier_date: menunjukan tanggal dikirimnya pesanan oleh kurir
-	Order_deilver_customer_date: menunjukan tanggal dikirimnya pesanan ke kostumer
-	Order_estimated_delivery_date: menunjukan estimasi lama waktu pengiriman ke pada kostomer

**7.	Olist product**
-	Product_id: nomer unik produk
-	Product_category_name: ketogy produk dalam portugis
-	Product_name_lenght: panjang huruf nama produk
-	Product_description_lenght: panjang huruh deskripsi dari produk
-	Product_photos_qty: banyaknya photo produk
-	Product_weight _g: nilai berat produk dalam gram
-	Product_lenght_cm: nilai panjang produk dalam cm
-	Product_height_cm: nilai tinggi produk dalam cm
-	Product_width_cm: nilai lebar produk dalam cm

**8.	Olist seller**
-	Seller_id: nilai unik penjual
-	Seller_zip_code_prefix: 5 digit pertama dari zip code penjual
-	Seller_city: kota penjual
-	Seller_state: negara bagian penjual

**9.	Product category name transalation**
-	Product_category_name: nama kategori dalam portugis
-	Product_category_name_english: nama kategori dalam bahasa inggris

![RED](https://user-images.githubusercontent.com/115323333/205472050-14a98c5c-0a33-4446-92f3-943963a13d7a.png)


### Data proses 
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
**1.	Mencari sales summary per item**
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

Kemudian dilakukan pencarian kolom yang terdapat nilai null dan duplicate:

	# find missing value
          sales_products.isna().sum()
          # find duplicated data in dataset
          sales_products.duplicated(keep=False).sum()

selanjutnya kita dapat melakukan grouping dan agregasi 
		
          # define coloum that needed for analysis
          coll_1 = ["product_category_name_english", "price"]
          category_grouping = sales_products[coll_1].groupby("product_category_name_english").agg(['max', 'min', 'sum', 'mean'],inplace=True)
          table_1 = category_grouping.sort_values(by =["product_category_name_english"], ascending= True )

tabel 2. table hasil grouping dan agregasi

![image](https://user-images.githubusercontent.com/115323333/205473219-036b4a17-4f92-4e13-a3ab-46a8cd8ba771.png)

          
**2.	Mencari Total penjualan per region**
Objektif penelitian ini mencari nilai total transaksi penjualan masing-masing region.
Tahapan pertama adalah mengecek nilai customer state apakah sudah benar dengan data geografis pada brazil, brazil mempunyai 27 negara bagian.

          # finding how many state in the table
          customer_table["customer_state"].unique()

hasil data fungsi unique adalah menampilkan data yang berbeda dari kolom customer_state terdapat 27 negara bagian sesuai dengan hasil geografis di brazil.
# nilai unique dari customer_state

          array(['SP', 'SC', 'MG', 'PR', 'RJ', 'RS', 'PA', 'GO', 'ES', 'BA', 'MA', 'MS', 'CE', 'DF', 'RN', 'PE', 'MT', 'AM', 'AP', 'AL', 'RO', 'PB','TO', 'PI', 'AC', 'SE', 'RR'], dtype=object)

![image](https://user-images.githubusercontent.com/115323333/205473229-f4e57b49-ac3c-488f-96e5-34baf2920ab2.png)

kemudian dilakukan data cleaning dan mencari nilai null yang sama seperti objektivitas yang pertama. 

          table_2 = sales_per_state.groupby(by="customer_state").sum().sort_values(by="price", ascending=False)

Tabel 3. tabel hasil grouping dan sort value dari wilayah yang mempunyai transaksi penjualan paling tinggi

![image](https://user-images.githubusercontent.com/115323333/205473238-7c0f8adf-8159-4fb1-874c-3bc0d7e32e65.png)

**3.	mencari produk yang paling banyak pada region yang paling banyak dibeli **
	
Analisa ini adalah mencari produk yang paling sering di beli pada setiap negara bagian yang ada, table yang dibutuhkan adalah table order_customer dan order_dataset.

Tabel 4. Tabel hasil dari merge tabel dan grouping menggunakan produk

![image](https://user-images.githubusercontent.com/115323333/205473255-71991a34-d01b-4822-9421-1fdc8c27a001.png)

          region_sales_condition = product_sales_region.groupby(["customer_state","product_category_name_english"]).agg({"product_category_name_english":"count"})

Tabel 5. Tabel hasil dari grouping negara bagian dengan category produk

![image](https://user-images.githubusercontent.com/115323333/205473265-3757b910-5b2f-443c-8bdf-d383c627d688.png)

**4.	menghitung probability seorang akan melakukan transaksi bila dia berada dalam satu region(city,atau provinsi) yang sama?**

table yang dibutuhkan adalah table sellers_dataset, customer_dataset  dan order_dataset

tabel 6. Tabel hasil dari merge data sellers_dataset, customer_dataset  dan order_dataset

![image](https://user-images.githubusercontent.com/115323333/205473283-7c7537d1-ffd2-44a0-81b5-bcd628804e36.png)

Setelah mendapat data yang dibutuhkan maka tahap selanjutnya adalah memfilter data dimana order_delivered_customer_data terisi karena kita hanya mencari nilai dimana sudah dilakukan pengiriman produk dari penjual kepada pembeli. Dan juga mencari data dimana bila pembeli dan penjual dalam satu kota maka kolom lokasi_transaksi akan memberikan nilai “satu wilayah” dan bila tidak maka akan menampilka “beda wilayah”.

          location_condition["lokasi_transaksi"] =np.where(location_condition["customer_city"] == location_condition["seller_city"],"Satu wilayah","beda wilayah")

**tabel 7.** Hasil penyortiran lokasi pembeli dan penjual

![image](https://user-images.githubusercontent.com/115323333/205473300-19ae57a7-046e-49d2-a1f5-34b3fc5296e1.png)

          # calculate data in tabel
          total_data = len(location_condition)
          kondisi_1 = len(location_condition[location_condition["lokasi_transaksi"] == "Satu wilayah"])
          kondisi_2 = len(location_condition[location_condition["lokasi_transaksi"] == "beda wilayah"])

          # mencari conditional probability
          prob_sama_wilayah = kondisi_1/total_data
          prob_beda_wilayah = kondisi_2/total_data

          # print
          print(f"-probability customer yang berada dalam wilayah yang sama dengan seler adalah {prob_sama_wilayah*100:.2f}% dan yang berbeda wilayah adalah {prob_beda_wilayah*100:.2f}%")

dengan menggunaka conditional probability bisa mendapatkan probability customer yang berada dalam wilayah yang sama dengan seler adalah 96.04% dan yang berbeda wilayah adalah 3.96%.

**5.	mencari rata-rata review bintang yang diterima seller**

table yang dibutuhkan adalah table order_item_dataset,  order_reviews_dataset dan order_dataset

tabel 8. Hasil merge data order_item_dataset,  order_reviews_dataset dan order_dataset

![image](https://user-images.githubusercontent.com/115323333/205473319-cf6e997f-27ed-4c6d-984f-23cfe0b53942.png)

          # grouping and aggregate data
          customer_review["avg_review_score"] = customer_review.groupby("seller_id").agg({"review_score" : "mean"}).reset_index(drop=True)
          # search for null in coloum
          seller_statistic.isna().sum()
          #filter null in data
          review_score_seller = seller_statistic.dropna(subset= ["avg_review_score"])

tabel 9.  Hasil agregasi review skor

![image](https://user-images.githubusercontent.com/115323333/205473327-aa372bdf-edf3-4172-a27d-9ec68190afe9.png)

Buat histogram dari data yang ada menggunakan library seaborn, dengan menggunaka nilai avg_review_score

          # plotting a histogram
          sns.distplot(review_score_seller["avg_review_score"], hist=True, kde=True, 
                       bins=int(5), color = 'darkblue', 
                       hist_kws={'edgecolor':'black'},
                       kde_kws={'linewidth': 1})
          gambar 1.  Histogram review score 

![image](https://user-images.githubusercontent.com/115323333/205473337-b99175f4-0392-499c-9b7f-646521e06923.png)




