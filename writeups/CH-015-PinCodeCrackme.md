# Pincode Crackme - GDB ile Statik + Dinamik Analiz Writeup

## Özet

Bu writeup'ta, Linux üzerinde çalışan basit bir ELF binary'sinin ("pincode") beklediği 4 haneli PIN kodunu, kaynak koda erişmeden, sadece statik ve dinamik analiz teknikleriyle nasıl bulduğumu anlatıyorum. Kullanılan araçlar: `file`, `strings`, `objdump`, `gdb` (PEDA eklentisi ile).

**Hedef:** Binary'nin doğru kabul ettiği 4 haneli PIN kodunu bulmak.

---

## 1. Ön Bilgi Toplama - `file` Komutu

İlk adım olarak dosyanın türünü ve temel özelliklerini `file` komutuyla inceledim. Bunu yaptım çünkü hangi mimari ve platform için derlendiğini bilmeden doğru analiz stratejisini seçmek mümkün değil.

<img width="1393" height="433" alt="image" src="https://github.com/user-attachments/assets/44816c64-5fd2-47e1-bb56-0c54604cd66a" />

```bash
$ file pincode
pincode: ELF 32-bit LSB pie executable, Intel 80386, version 1 (SYSV),
dynamically linked, interpreter /lib/ld-linux.so.2, BuildID[sha1]=557a3
c8bca2794bd0a435d364cb44bb94f92ccd4, for GNU/Linux 3.2.0, not stripped
```

**Çıkarımlar:**

| Bilgi | Anlamı |
|---|---|
| ELF 32-bit | Linux çalıştırılabilir formatı, 32-bit mimari (register'lar `EAX`, `EBX`... formatında olacak) |
| LSB pie executable | Position Independent Executable — adres uzayı her çalıştırmada değişebilir (ASLR ile) |
| Intel 80386 | x86 mimarisi |
| dynamically linked | Program çalışırken paylaşımlı kütüphaneleri (`libc.so.6` gibi) kullanıyor |
| not stripped | Sembol tablosu silinmemiş — fonksiyon isimleri (`main` gibi) hâlâ görünür durumda, bu analizi kolaylaştırıyor |

Bu komut çıktısında programlama dili hakkında net bir bilgi yoktu, bu yüzden bir sonraki adıma geçtim.

---

## 2. String Analizi - `strings` Komutu

Programın hangi dilde yazıldığını ve içinde ne tür sabit metinler barındırdığını anlamak için `strings` aracını kullandım. Bunu yaptım çünkü bir binary'nin içindeki düz metin ifadeleri (mesajlar, dosya yolları, bazen kimlik bilgileri) programın davranışı hakkında hızlıca fikir verir ve dinamik analizde nereye odaklanacağımı belirlemede referans noktası olur.

```bash
$ strings pincode
```

<img width="1182" height="330" alt="image" src="https://github.com/user-attachments/assets/02fe11ff-83d1-49d5-9883-61c92488a3ce" />

Çıktıda dikkat çeken kısımlar:

```
puts
printf
```

Bu isimler C standart kütüphanesine ait fonksiyon isimleridir; bu programın **C dilinde** yazıldığına işaret eder.

<img width="885" height="378" alt="image" src="https://github.com/user-attachments/assets/47df451e-3fef-4d5c-86c2-4e20779b0ee2" />

```
PIN(4 Digits):
Correct :)
Wrong :(
Try Harder !!
```

Bunlar programın kullanıcıya gösterdiği mesajlardır. Program çalıştırıldığında ekrana basılan yazıların, dosya içinde düz metin olarak saklandığını doğruladım.

<img width="990" height="659" alt="image" src="https://github.com/user-attachments/assets/e22aca78-12ce-490e-b9e1-7db7ff819711" />

```
crtstuff.c
pincode.c
```

`.c` uzantılı dosya isimleri, kaynak dosyanın C dilinde yazıldığını kesinleştiriyor.

> **Not:** `strings` çıktısı uzun olabileceği için `strings pincode | more` komutuyla sayfa sayfa okumak pratik bir yöntem.

---

## 3. Davranış Gözlemi - Programı Çalıştırma

Debug etmeden önce programın normal kullanıcı akışını gördüm, çünkü aradığım karşılaştırma noktasını bulmak için bu davranışı bir "harita" olarak kullanacaktım.

<img width="744" height="191" alt="image" src="https://github.com/user-attachments/assets/6dcbdf05-acc8-4dcb-87db-1094f847e095" />

```bash
$ ./pincode
PIN(4 Digits):1234
Wrong :(
Try Harder !!
```

Buradan çıkardığım sonuç: Program önce bir mesaj basıyor (`printf`), sonra kullanıcıdan girdi alıyor (`scanf`), sonra girilen değeri kontrol edip sonuç mesajı basıyor. Dinamik analizde bu üç adımı sırasıyla arayacağım.

---

## 4. Statik Disassembly - `objdump`

Dinamik analize geçmeden önce `main` fonksiyonunun assembly kodunun genel yapısına `objdump` ile göz attım. Bunu, GDB içinde neyle karşılaşacağıma dair önceden fikir sahibi olmak için yaptım.

```bash
$ sudo objdump -d pincode -M intel
```

`-d` parametresi disassemble işlemini, `-M intel` parametresi ise çıktının Intel sözdizimiyle (AT&T yerine) gösterilmesini sağlıyor. Çıktıda `main` fonksiyonunun tüm komutlarını statik olarak gördüm, ancak bu görünüm register içeriklerini veya çalışma anındaki gerçek değerleri göstermiyor — bunun için dinamik analiz (GDB) gerekiyor.

<img width="1268" height="1112" alt="image" src="https://github.com/user-attachments/assets/2529e53e-26ff-4f09-b691-02b4740327c7" />

---

## 5. Dinamik Analiz - GDB ile Debug

### 5.1 GDB'yi açma ve dosyayı tanıtma


```bash
$ gdb -q
gdb-peda$ file pincode
gdb-peda$ info functions
```

<img width="1047" height="876" alt="image" src="https://github.com/user-attachments/assets/d27fce1a-f81f-4022-bf7f-36ff5c99d08b" />

`info functions` çıktısında `main` fonksiyonunu gördüm ve analize buradan başlamaya karar verdim, çünkü C programlarında program akışı bu fonksiyondan başlar.

### 5.2 Breakpoint koyma ve çalıştırma

```bash
gdb-peda$ b main
gdb-peda$ run
```

`main` fonksiyonuna bir breakpoint koydum ve programı çalıştırdım. Program `main`'in başında durdu, register/kod/stack durumunu PEDA arayüzünde görebildim.

<img width="1324" height="1092" alt="image" src="https://github.com/user-attachments/assets/7e35fd82-db54-4971-88ab-620eaf24aac9" />


### 5.3 Referans noktalarını takip ederek ilerleme

Doğrudan "karşılaştırma nerede yapılıyor" diye aramak yerine, adım 3'te tespit ettiğim davranış sırasını (`printf` → `scanf` → karşılaştırma) referans alarak `ni` (nexti) komutuyla adım adım ilerledim. Bunu yaptım çünkü büyük bir binary'de rastgele her yere bakmak yerine, bilinen davranışı iz sürerek kritik noktaya çok daha hızlı ulaşılabiliyor.

**`printf` çağrısını tespit ettim:**

<img width="1325" height="1081" alt="image" src="https://github.com/user-attachments/assets/c3a21a6e-460e-4055-a752-4784f24a6098" />


```
=> 0x565561e7 <main+46>: call 0x56556030 <printf@plt>
```



Stack üzerinde `"PIN(4 Digits):"` string'inin adresini görerek, bu çağrının ekrana bu mesajı basacağını doğruladım.

**`scanf` çağrısını tespit ettim:**

<img width="1271" height="1123" alt="image" src="https://github.com/user-attachments/assets/2b0df320-5f08-4d85-83ae-d6614f6aabd4" />


```
=> 0x565561fd <main+68>: call 0x56556060 <__isoc99_scanf@plt>
```

Bu komutu `ni` ile çalıştırdığımda terminal benden PIN kodu istedi. Doğru şifreyi tahmin etmek yerine, kontrol mekanizmasını görmek için bilinçli olarak rastgele bir değer girdim:

<img width="1309" height="1133" alt="image" src="https://github.com/user-attachments/assets/08638cad-e53d-45ee-8552-c5481fde8d09" />

```
PIN(4 Digits):1111
```

### 5.4 Karşılaştırma noktasını bulma - `cmp` komutu

`ni` ile ilerlemeye devam ettiğimde şu üç komuta ulaştım:

<img width="1244" height="1117" alt="image" src="https://github.com/user-attachments/assets/ffe4e606-81a6-46bb-8fbc-c6f23cde6c7d" />

```
0x56556205 <main+76>: mov  eax, DWORD PTR [ebp-0xc]
0x56556208 <main+79>: cmp  eax, 0x1b96
0x5655620d <main+84>: jne  0x56556223 <main+106>
```

Bu üç satırı sırasıyla açıklıyorum:

**`mov eax, DWORD PTR [ebp-0xc]`** — `[ebp-0xc]`, `main` fonksiyonunun stack alanında, benim `scanf` ile girdiğim PIN kodunun tutulduğu yerel değişkenin adresidir. Bu komut, o değeri `EAX` register'ına kopyalıyor. Komutu çalıştırdıktan sonra GDB'de `EAX` register'ının değerinin `0x457` olduğunu gördüm.

<img width="1268" height="1119" alt="image" src="https://github.com/user-attachments/assets/c24e1bde-0e0f-49fe-99ee-09d9749b72c5" />

`0x457` hexadecimal değerini decimal'e çevirdim:

```
0x457 = 4×16² + 5×16¹ + 7×16⁰ = 1024 + 80 + 7 = 1111
```

Bu, benim girdiğim `1111` değeriyle birebir eşleşti — yani `EAX` register'ında şu anda benim girdiğim PIN kodu duruyordu. Bu, doğru adımı takip ettiğimi doğruladı.

**`cmp eax, 0x1b96`** — Bu komut `EAX`'i (benim girdiğim değer) sabit bir değer olan `0x1b96` ile karşılaştırıyor. Bu sabit değer, derleyicinin kaynak koddaki doğru PIN kodunu makine koduna gömdüğü yerdi. Bu değeri decimal'e çevirdim:

```
0x1b96 = 1×16³ + 11×16² + 9×16¹ + 6×16⁰ = 4096 + 2816 + 144 + 6 = 7062
```

**`jne 0x56556223 <main+106>`** — "Jump if Not Equal" komutu, bir önceki `cmp` sonucuna göre karar veriyor. Karşılaştırılan değerler eşit değilse akış `main+106` adresine atlıyor (muhtemelen "Wrong :(" mesajının basıldığı blok), eşitse akış normal devam ediyor (muhtemelen "Correct :)" mesajının basıldığı blok). Bu, yüksek seviye dildeki `if (girilen_pin != 7062) { ... } else { ... }` mantığının assembly karşılığı.

Bu noktada doğru PIN kodunun **7062** olduğuna dair güçlü bir kanıt elde ettim.

---

## 6. Doğrulama

Analiz sonucunda elde ettiğim değeri gerçek programı çalıştırarak test ettim:

<img width="754" height="373" alt="image" src="https://github.com/user-attachments/assets/00b98063-18f8-4f14-a76b-27284a81882f" />

```bash
$ ./pincode
PIN(4 Digits):7062
Correct :)
```

Doğru PIN kodunun **7062** olduğu doğrulandı.

---

## Sonuç

Kullandığım yöntemi adım adım özetliyorum:

1. `file` ile binary'nin platform/mimari bilgisini aldım.
2. `strings` ile programın dilini ve içindeki mesajları tespit ettim.
3. Programı bir kez normal çalıştırıp kullanıcı akışını gözlemledim.
4. `objdump` ile `main` fonksiyonunun statik disassembly'sine göz attım.
5. GDB'de `main`'e breakpoint koyup çalıştırdım.
6. Bilinen davranış sırasını (`printf` → `scanf` → karşılaştırma) referans alarak `ni` komutuyla adım adım ilerledim.
7. `cmp` komutunun operandındaki sabit hex değeri decimal'e çevirerek doğru PIN kodunu (`7062`) elde ettim.
8. Bulduğum değeri gerçek binary üzerinde deneyerek doğruladım.

---

*Bu writeup, eğitim amaçlı bir reverse engineering egzersizi kapsamında hazırlanmıştır.*
