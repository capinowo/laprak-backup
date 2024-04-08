# Lighting dan Shadow - Laporan Praktikum GKV 
> Alya Safina - 24060122140123 - Lab B1 GKV

Laporan praktikum ini membahas tentang konsep lighting dan shadow dalam komputer grafis. 

## Cahaya?

### Cahaya

Cahaya adalah bentuk radiasi elektromagnetik yang terlihat oleh mata manusia. Manusia hanya dapat melihat cahaya yang terletak dalam spektrum tampak, yang mencakup warna-warna seperti merah, hijau, dan biru. Dengan cahaya, mata dapat menangkap bentuk rupa benda-benda yang ada di sekitar kita.

### Apa yang mempengaruhi pencahayaan?

Ada beberapa faktor yang mempengaruhi tampak cahaya pada netra, yaitu: 

- Arah dan sudut datang cahaya

Arah dan sudut datang cahaya mempengaruhi sudut pemantulan, sehingga mempengaruhi bagaimana cahaya nampak di citraan manusia. Ada 2 macam pemantulan yang mungkin terjadi, yakni _specular_ (cerminan), dan _diffuse_ (baur)


![](https://publish-01.obsidian.md/access/6d76c2f411599a3e53a071b11b6c0c46/Modul%20-%20GKV/Attachments/Pasted%20image%2020240316051003.png)

_Specular reflection_ (pemantulan spekular) adalah pemantulan cahaya yang terjadi dengan cara yang sama seperti pantulan pada cermin, di mana cahaya dipantulkan secara langsung dan terfokus pada sudut yang sama dengan sudut datang cahaya. _Diffuse reflection_ (pemantulan difusi) adalah pemantulan cahaya yang terjadi secara merata ke segala arah, sehingga menciptakan penyebaran cahaya yang tidak terfokus. Kasus ini terjadi pada permukaan yang kasar atau tidak rata.

- Rata permukaan

Permukaan yang kasar cenderung memantulkan cahaya secara _diffused_ (menyebar), sedangkan permukaan yang halus cenderung memantulkan cahaya secara _specular_ (langsung). Contohnya adalah permukaan licin yang terlihat lebih mengilat daripadada permukaan kasar.

- Jenis permukaan

Masing-masing bahan memiliki indeks bias material yang memisahkan dua medium dapat mempengaruhi sudut pemantulan. Misalnya, pada pemantulan total dalam, cahaya memantul kembali ke medium asal karena sudut datang melebihi sudut kritis.

### Mengapa cahaya penting?

Dalam komputer grafis, cahaya merupakan elemen kunci dalam menciptakan realisme visual. Selain memberikan dimensi dan profil pada objek, cahaya dapat membantu menciptakan atmosfer dan perasaan dalam sebuah adegan.

## Cahaya  pada grafik komputer
> Untuk mensimulasikan penerapan cahaya pada komputer grafis, saya menggunakan [Template Praktikum 6](https://github.com/Praktikum-GKV-2024/Template-Pertemuan-6) dengan perubahan yang dilakukan.

### Apa pengaruh cahaya?

Seperti dalam kehidupan nyata, ketika tidak ada cahaya dalam lingkungan, kita tidak dapat melihat objek karena tidak ada bayangan yang dipantulkan ke mata kita. Jika kita menghilangkan pencahayaan dari sebuah objek, maka objek tersebut akan terlihat gelap karena tidak ada sumber cahaya yang memantulkan cahaya ke mata kita, seperti yang dicontohkan pada media berikut.

![](https://cdn.discordapp.com/attachments/854029224977104926/1226902968826200146/image.png?ex=6626756e&is=6614006e&hm=7b8a964e462dfe1032d66b07854e8c3d1ac3b88e3fb179752813d8096b8f0fdc&)

Setelah kita mengembalikan pencahayaan ke semula, maka tampilan media akan berubah cerah, dan objek barel di dalamnya terlihat.

![](https://cdn.discordapp.com/attachments/854029224977104926/1226926250459926619/image.png?ex=66268b1d&is=6614161d&hm=caea200d33a83d10135700df3d13d5f8debd926354365ed851084ee528676d25&)

### Posisi sumber cahaya

Dimisalkan suatu tempat dengan satuan tinggi sumbu `x`, `y`, dan `z` yang diketahui, kita dapat menentukan dari mana cahaya dihasilkan, dengan menentukan koordinat `x`, `y`, dan `z` untuk asal cahaya tersebut. Berikut akan dilakukan perubahan posisi kamera, untuk mendapatkan detail

Diketahui scene sebagai berikut:

![](https://cdn.discordapp.com/attachments/854029224977104926/1226926988560699442/image.png?ex=66268bcd&is=661416cd&hm=b465e8e845d469e72b3428794ac21318a07315e8c9abff14f150d4b9ede0e454&)

![](https://cdn.discordapp.com/attachments/854029224977104926/1226926250459926619/image.png?ex=66268b1d&is=6614161d&hm=caea200d33a83d10135700df3d13d5f8debd926354365ed851084ee528676d25&)

Diketahui bahwa cahaya berada pada posisi `(0, 10, 0)` pada bidang, yang diatur pada
```
LightPosition = vec3(0, 10, 0);
```

Melalui inisialisasi posisi vektor, kita dapat mengatur posisi sumber cahaya. Ada 2 cara yakni 
1. Langsung menginisialisasi posisi cahaya dalam satu langkah
```
LightPosition = vec3(x, y, z);
```
2. Mengatur koordinat `x`, `y`, dan `z` secara terpisah untuk menentukan posisi cahaya
```
LightPosition.x = x.f;
LightPosition.z = y.f;
LightPosition.y = z.f;
```

### Komponen cahaya

Seperti yang kita ketahi, cahaya yang ada akan membaur dan memantul pada objek yang ada dalam bidang. Pada grafik komputer sendiri, interaksi antara sumber cahaya dan objek biasanya dijelaskan oleh tiga komponen utama: _ambient_, _diffuse_, dan _specular_.

![](https://upload.wikimedia.org/wikipedia/commons/f/ff/Phong_components_revised.png )

Untuk mempermudah visualisasi, akan digunakan model barel pada Praktikum  6.

Berikut adalah penjelasan dari masing-masing komponen:

- Ambient

Ambient mensimulasikan permukaan objek yang terkena cahaya tidak langsung, dengan menggunakan konstanta penerangan ambiens yang selalu memberikan warna pada objek. Pada dunia nyata, sejatinya setiap cahaya tidak hanya sekali memantul ke permukaan benda dan hilang begitu saja Tiap partikel cahaya yang masih memiliki energi akan terus memantul hingga habis. Untuk mensimulasikan fenomena ini, kita hanya akan membuat bahwa setiap warna yang ada di objek memiliki tingkat keterangan minimal. 

Berikut adalah gambar barel dengan komponen pencahayaan ambient.

![](https://cdn.discordapp.com/attachments/854029224977104926/1226936457709617212/image.png?ex=6626949e&is=66141f9e&hm=e7a1d7808773a8039fcc6780a06db02a4060e00b7321123dc70bf30193a029b5&)
![](https://cdn.discordapp.com/attachments/854029224977104926/1226936458070462545/image.png?ex=6626949e&is=66141f9e&hm=68f4cbce4d3684a60f78ac9568b76be3781b0066df4ad1e886bec08bea0c7c75&)
```
	color = 
		// Ambient : simulates indirect lighting
		MaterialAmbientColor;
```

- Diffuse

Diffuse mensimulasikan dampak arah cahaya dari objek cahaya pada suatu objek. Ketika cahaya jatuh di suatu permukaan, maka cahaya akan memantul dari permukaan dengan sebagian energi dari cahaya akan memencar ke segala arah. Komponen diffuse membantu memetakan arah cahaya dari sumber cahaya. Pencahayaan diffuse lebih terang ketika objeknya semakin dekat dengan sumber cahaya.

Berikut adalah gambar barel dengan komponen pencahayaan diffuse.

![](https://cdn.discordapp.com/attachments/854029224977104926/1226936802837794897/image.png?ex=662694f1&is=66141ff1&hm=a2831021511318b5263e0dd74036a62f4937734c440329e4dbb0e9fd07e36948&)
![](https://cdn.discordapp.com/attachments/854029224977104926/1226936803219738715/image.png?ex=662694f1&is=66141ff1&hm=ffa17226f95365dacf1c24b0cc4201689ebc72535515aaff0d644481db4f1f7e&)

```
	color = 
		// Ambient : simulates indirect lighting
		MaterialAmbientColor +
		// Diffuse : "color" of the object
		MaterialDiffuseColor * LightColor * LightPower * cosTheta / (distance*distance);
```

Unsur-unsur pembentuk material Diffuse yaitu:
1. `MaterialDiffuseColor`: Warna yang dipantulkan jika terjadi pemantulan baur
2. `LightColor`: Warna dari cahaya yang datang
3. `LightPower`: Kekuatan cahaya
4. `cosTheta`: Sudut insiden cahaya dan normal
5. `distance`: Jarak posisi vertex ke cahaya

Kedua komponen yang sudah kita ketahui, yakni ambient dan diffuse, dapat digabung untuk menghasilkan tampilan objek sebagai berikut: 

![](https://cdn.discordapp.com/attachments/854029224977104926/1226936663217799249/image.png?ex=662694cf&is=66141fcf&hm=8caeec665c4647cac56ece52d0db1de0458ecdf21c018bc5ff10c741b3933e68&)
![](https://cdn.discordapp.com/attachments/854029224977104926/1226936662739783791/image.png?ex=662694cf&is=66141fcf&hm=59d737f35f1c5be4fa5c5255e7d3c88562ea84a4033708113ab4438182892021&)

Sekarang, tampilan objek sudah makin terlihat cantik!

- Specular

Specular mensimulasikan titik terang cahaya yang muncul pada objek berkilau. Pencahayaan spekular didasarkan pada vektor arah cahaya dan vektor normal objek, serta arah pandang pengamat, misalnya dari arah mana pengamat melihat fragmen tersebut. Pencahayaan spekular didasarkan pada sifat-sifat reflektif permukaan. Dimisalkan kita menganggap permukaan objek sebagai cermin, maka  pencahayaan spekular paling kuat di mana pun kita melihat cahaya terpantul pada permukaan tersebut. 

Berikut adalah gambar barel dengan komponen pencahayaan specular.

![](https://cdn.discordapp.com/attachments/854029224977104926/1226936995306143744/image.png?ex=6626951f&is=6614201f&hm=153239e1332820d1c6950c3b4ea4944be4bbe273e06ee2939df587e2d16d580b&)
![](https://cdn.discordapp.com/attachments/854029224977104926/1226936995608264734/image.png?ex=6626951f&is=6614201f&hm=ecd54003d1c8b90e606ed80977fb9b566d7d4f2321d17831eb40cdef6e0f13d8&)

```
	color = 
		// Ambient : simulates indirect lighting
		MaterialAmbientColor +
		// Diffuse : "color" of the object
		MaterialDiffuseColor * LightColor * LightPower * cosTheta / (distance*distance) + //; // +
		// Specular : reflective highlight, like a mirror
		MaterialSpecularColor * LightColor * LightPower * pow(cosAlpha,5) / (distance*distance);
```
Unsur-unsur pembentuk material Specular yaitu:
1. `MaterialSpecularolor`: Warna yang dipantulkan jika terjadi pemantulan teratur
2. `LightColor`: Warna dari cahaya yang datang
3. `LightPower`: Kekuatan cahayanya
4. `cosAlpha`: Sudut insiden camera dan normal 
5. `k`: Angka yang mengatur seberapa besar sudut yang dapat memantulkan cahaya secara teratur
6. `distance`: Jarak posisi vertex ke cahaya

Ketiga komponen ini dapat digabung untuk menghasilkan tampilan objek sebagai berikut: 

![](https://cdn.discordapp.com/attachments/854029224977104926/1226937109139554385/image.png?ex=6626953a&is=6614203a&hm=5d05cd840b68ec868cae0bb960714311effc3acc0f9f4640b62d0a2bd8101818&)
![](https://cdn.discordapp.com/attachments/854029224977104926/1226937148192591882/image.png?ex=66269543&is=66142043&hm=cd1f7652c3d25023db8bee8d592fc35923332fda9a3af258eaba4d78be9533b4&)

Selamat, barel kita sudah lebih cantik!

## Bayang-bayang
Bayang-bayang adalah konsekuensi dari adanya cahaya, sebagaimana bayang-bayang dihasilkan karena ketiadaan cahaya. Ketika sinar cahaya dari sumber cahaya tidak mengenai suatu objek karena disembunyikan oleh objek lain, objek tersebut berada dalam bayangan.

![](https://learnopengl.com/img/advanced-lighting/shadow_mapping_theory.png)

Garis biru mewakili fragmen yang dapat dilihat oleh sumber cahaya. Fragmen yang terhalangi cahaya ditunjukkan sebagai garis hitam, yang kemudian akan dirender sebagai bagian yang tidak tersorot cahaya, sehingga menjadi sisi gelap a.k.a. bayang-bayang. 

- Pemetaan bayangan 

Untuk mensimulasikan sisi gelap objek, digunakan sumber cahaya yang sama, namun dengan kekuatan cahaya yang diperbesar dua kali lipat.

```
	float LightPower = 100.0f;
```

Melalui proses di atas, diperoleh tampilan berikut:

![](https://cdn.discordapp.com/attachments/854029224977104926/1227017310150070403/image.png?ex=6626dfeb&is=66146aeb&hm=379e13301320cce28b3ff3a946be4f764a663b44c3315d904099403659b2ec18&)

Pada barel pertama yang lebih  dekat dengan sumber cahaya, sisi kanan terkena cahaya sehingga menjadi terang. Sisi kiri menjadi gelap, karena tidak terkena cahaya.

![](https://cdn.discordapp.com/attachments/854029224977104926/1227017310993125436/image.png?ex=6626dfeb&is=66146aeb&hm=ef5327577f70ea8dc5aeb1131a216da4696724d56eb6e886c460f24cb0c03257&)

Melalui tampilan ini, dapat diamati perbedaan antara sisi gelap dan sisi terang pada barel. Adapun sisi gelap adalah bayang-bayang yang dihasilkan, karena cahaya yang terhalang sisi lain barel.

# Reference
- [Amygdala](https://amygdala.shariyl.cloud/Modul+-+GKV/Pertemuan+4+-+Lighting)
- [LearnOpenGL](https://learnopengl.com/Advanced-Lighting/Shadows/Shadow-Mapping)
- [Wikipedia](https://en.wikipedia.org/wiki/Lighting)

and [others](https://emojicombos.com/cute-kaomoji).

## terima kasih!  ദ്ദി(˵ •̀ ᴗ - ˵ ) ✧
